---
title: "[AWS Hands on] Image Resizing Architecture"
datePublished: Tue Mar 07 2023 04:24:51 GMT+0000 (Coordinated Universal Time)
cuid: clexqz4kl000309le9zn18diu
slug: aws-hands-on-image-resizing-architecture
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1678163052591/d550fde8-49c1-4602-9f40-b703c3b59c21.png
tags: lambda, aws, amazon-s3, sqs, api-gateway

---

**Lambda 함수를 이용한 이미지 썸네일 제작 서버리스 아키텍처를 실습해보려 한다!**

1. ## Blue Print of Architecture
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677981098184/f735f3b0-ae67-4729-8c37-12a552eae3e1.png align="center")
    
    User가 Put 요청 --&gt; API GateWay가 원본 이미지 버킷에 이미지 업로드 --&gt; 람다함수 트리거 --&gt; 썸네일 제작 후 썸네일 버킷에 저장, DynamoDb 테이블에 썸네일 제작 성공한 이미지 이름 저장 --&gt; **IF : 이미지 제작 실패 시 실패 로그 SQS 대기열 저장**
    

## Hands On

1. S3 버킷생성
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677981487397/84f1c4af-e4a8-4846-aa8e-924a0bbd3445.png align="center")

원본 이미지가 저장 될 버킷과 썸네일 이미지가 저장될 버킷을 생성한다

1. Lambda 함수 생성
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677981551175/3a9090bd-bc12-43f8-a5c9-8b6e3a6af935.png align="center")

위와 같이 Lambda 함수를 생성해준다 이때 역할은 아래 포맷과 같다

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "sqs:DeleteMessage",
                "s3:GetObject",
                "sqs:GetQueueUrl",
                "logs:CreateLogStream",
                "dynamodb:PutItem",
                "sqs:ReceiveMessage",
                "sqs:SendMessage",
                "sqs:GetQueueAttributes",
                "logs:CreateLogGroup",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:sqs:*:sqs대기열:*",
                "arn:aws:dynamodb:*:항목 저장할 당신의 Dynamo DB/*",
                "arn:aws:s3:::당신의 원본 버켓/*",
                "arn:aws:logs:*:*:*"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:PutObject",
            "Resource": "썸네일 이미지 저장 버킷/*"
        }
    ]
}
```

1. 실패 시 로그 저장 할 SQS 대기열 생성
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678162074814/725b73b8-4394-41bd-bd11-d2af88784988.png align="center")
    
    실패시 로그를 메시지로 받을 SQS 대기열을 생성해준다
    
2. Lambda 함수에 트리거 추가
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677981968243/76331b14-5ac4-4537-887a-8c765d74968a.png align="center")
    
    원본 이미지 버킷에 객체가 업로드 되면 Lambda 함수가 실행될 수 있도록 트리거 설정 이벤트 유형은 모든 객체 생성 이벤트로 해준다
    
3. SQS 대기열 대상 추가
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677982156861/4a7cc3be-6827-4810-90ff-33c5114fcc65.png align="center")

실패 시 로그를 저장할 SQS 대기열을 대상 추가 해준다

1. Python PIL 모듈 레이어 추가
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678162145916/f420c255-88cd-453f-bc27-fcf1f536dd23.png align="center")
    
    이 부분이 가장 힘들었는데 Lambda 함수 Python은 최소한의 모듈만 가지고 있어서 따로 레이어로 모듈을 추가해줘야 한다,,,, ***하지만,,!***
    

로컬 Python --&gt; site Package 에서 모듈을 따로 빼서 레이어에 추가해 줄 경우,,, **PIL \_iamging can not import** 오류를 볼 것이다,,, Lambda가 Pillow 패키지의 Compiled Binary 패키지를 인식하지 못해서 생기는 오류라고 한다 ,,!

***인간은 늘 해결책을 찾는 법***

[https://api.klayers.cloud/api/v2/p3.9/layers/latest/ap-northeast-2/html](https://api.klayers.cloud/api/v2/p3.9/layers/latest/ap-northeast-2/html) 에서 Pillow 패키지의 ARN 을 복사해 위 사진처럼 ARN 지정을 통해 레이어를 추가해주면 된다!!

1. Lamda 코드 작성
    

1. ```python
    import boto3
    import os
    import sys
    sys.path.append('/opt')
    import uuid
    from urllib.parse import unquote_plus
    from PIL import Image
    import PIL.Image
    
    # AWS 리소스 생성
    s3_client = boto3.client('s3')
    dynamodb = boto3.client('dynamodb')
    sqs = boto3.client('sqs')
    
    # AWS 환경 변수
    ACCESS_KEY = os.environ.get('ACCESS_KEY')
    SECRET_KEY = os.environ.get('SECRET_KEY')
    REGION_NAME = 'ap-northeast-2'
    
    # SQS 대기열 URL
    QUEUE_URL = '당신의 sqs 대기열 URL'
    
    def resize_image(image_path, resized_path):
        with Image.open(image_path) as image:
            image.thumbnail(tuple(x / 2 for x in image.size))
            image.save(resized_path)
    
    def process_image(bucket, key):
        try:
            tmpkey = key.replace('/', '')
            download_path = '/tmp/{}{}'.format(uuid.uuid4(), tmpkey)
            upload_path = '/tmp/resized-{}'.format(tmpkey)
            s3_client.download_file(bucket, key, download_path)
            resize_image(download_path, upload_path)
            s3_client.upload_file(upload_path, '썸네일 버킷 이름', 'resized-{}'.format(key))
            return True
        except:
            return False
    
    def lambda_handler(event, context):
        for record in event['Records']:
            bucket = record['s3']['bucket']['name']
            key = unquote_plus(record['s3']['object']['key'])
    
            if not process_image(bucket, key):
                # 이미지 리사이징 실패 시 SQS 대기열에 저장
                response = sqs.send_message(
                    QueueUrl=QUEUE_URL,
                    MessageBody=key
                )
            else:
                # 이미지 리사이징 성공 시 DynamoDB에 이미지 메타데이터 저장
                item = {
                    'name': {'S': key}
                 
                }
                dynamodb.put_item(
                    TableName='Dynamo DB 테이블 명',
                    Item=item
                )
    위 와 같이 코드를 작성해준다.
    ```
    
    1. API Gateway 생성
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677983420323/b4c9f812-6af6-4187-8efa-50b6bac906aa.png align="center")
    
    Rest로 API GateWay 생성 후 리소스 {bucket}/{filename} 두 가지를 생성 후 Filename 아래 PUT 메서드 생성 후 사진과 같이 세팅해준다
    
    1. URL 경로 파라미터 설정
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677983604311/428808b6-c033-423f-9f2b-720105c98ff7.png align="center")
    
    URL 경로파라미터를 사진과 같이 추가해준다
    
    1. 이진 미디어 형식 추가
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677983649171/6c73038d-6dfa-435e-9f93-e2abb17a8a78.png align="center")
    
    파일 포멧을 세부적으로 필터링 할 이진 미디어 형식을 추가해준다 이 실습에서는 모든 포멧을 업로드하는 *\*/\* 형식으로 한다 .*
    
    1. API 배포
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677983775630/03585ace-d70b-4d2e-baa2-81c5feba7617.png align="center")
    
    스테이지 이름을 설정해주어서 API를 배포한다
    
    1. PostMan을 통해 API에 PUT Request 테스트
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677983983688/59a93844-0667-470a-9f3f-d06458fda144.png align="center")
    
    프론트엔드가 없으므로 POST MAN을 이용한다. 이때 URL 형식은 아래와 같다
    
    API GateWay URL/스테이지명/버킷명/올릴파일이름.확장자 --&gt; BODY의 binary를 선택하고 파일을 업로드 해준다
    

1. 원본 버킷 확인
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678162401781/b46e31c5-840c-4ff7-b13e-607212e28883.png align="center")
    
2. 썸네일 버킷 확인
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678162812317/cbb6bbab-3103-449d-9b4e-86eaffae912a.png align="center")
    
3. Dynamo DB 테이블 확인
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678162910975/abe940af-36c9-409b-a1e4-e832a7f4f0b2.png align="center")
    

## 느낀점

별의 별 오류를 많이 마주쳤다,, 특히 Pillow 오류는 진짜 몇일 동안 구글링 하여 찾아냈다,, 기특해 내 자신,,!

403 Deny : IAM 권한과 역할 설정을 다시 체크할 것! or Lambda 함수 환경변수에 유효한 IAM Access Key와 Secret key인지 확인할 것

404 Not Found: 코드의 오타를 확인할 것,, Lambda Test시 S3 Put Object Json 문서에서 파일명과 리전과 버킷이름을 잘 수정할것,,!!

이제 다시,, JAVA 공부하자!!