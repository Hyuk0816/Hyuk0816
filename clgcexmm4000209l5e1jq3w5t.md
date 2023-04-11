---
title: "[Network Study] EP5. L2"
datePublished: Tue Apr 11 2023 15:24:01 GMT+0000 (Coordinated Universal Time)
cuid: clgcexmm4000209l5e1jq3w5t
slug: network-study-ep5-l2
tags: network, switch, osi-layer, l2

---

OSI 7 Layer의 데이터 링크 계층인 L2 레이어에 대해 공부한 것을 정리하려 한다

### 이더넷

**L2 데이터 링크 계층은 네트워크 장비 간에 신호를 주고받는 규칙을 정하는 계층으로 일반적으로 '이더넷' 을 가장 많이 사용함**

데이터에 대한 목적지 정보를 추가하여 보내고 목적지 이외의 컴퓨터는 데이터를 받더라도 무시하게 되는 규칙

이더넷은 허브와 달리 여러 컴퓨터가 동시에 데이터를 전송해도 충돌이 일어나지 않는 구조로 되어 있음 --&gt; CSMA/CD 구조!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681225427522/28f20ea5-33fb-4ded-979c-6e2a1a2d5582.png align="center")

CS: 데이터를 보내려고 하는 컴퓨터가 케이블에 신호가 흐르는지 체크

MA: 케이블에 데이터가 흐르고 있지 않다면 데이터 수신이 가능하다는 규칙

CD: 충돌이 발생하고 있는지를 확인하는 규칙

\--&gt; 허나 지금은 효율이 좋지 않다는 이유로 사용하지 않음. L2 스위치가 있기 때문에

### MAC 주소

![네트워크] MAC 주소에 대해서](https://images.velog.io/images/kimyeji203/post/ae02c51c-0451-4196-844d-9169743cb008/image.png align="center")

* MAC 주소란 물리주소라고도 하며 16진수 48bit로 구성되어 있다. 그중 앞쪽 24bit는 랜카드를 만든 제조사 번호고 뒤쪽 24비트는 제조사가 랜 카드에 붙인 시리얼 번호이다. MAC 주소는 고유한 주소며 전 세계에서 유일한 번호로 할당된다.
    
* MAC 주소를 사용한 통신을 할 때 데이터는 목적지 MAC 주소, 출발지 MAC 주소, 유형 세가지 14바이트로 구성된다. 
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681226321556/e007ba03-4300-410c-b64b-718633eaedc4.png align="center")

* CMD에 ipconfig /all 을 입력하면 MAC 주소를 확인할 수 있다!
    

### L2 스위치

![네트워크] 데이터 링크 계층(Data Link Layer) - L2 스위치와 내부 동작(feat. Spanning Tree  Algorithm)](https://velog.velcdn.com/images%2Fredgem92%2Fpost%2Fe8206c2b-4781-42cc-a918-0b0262c8a37e%2Fimage.png align="left")

* 사진은 L2 스위치 허브 기계다. End-Point와 직접 연결되는 스위치이고 MAC 주소를 근거로 스위칭 한다. 가정에서 쓰는 허브보다 성능이 좋다! 가격은 10만원 언저리 즈음,,! 물리적인 전송을 담당하는 계층인 만큼 진짜 '물리'적인 장비가 있다. L2 스위치는 프레임(비트의 모음)을 전달하는 역할을 한다.
    
* 스위치 내부에는 **MAC 주소 테이블**이라는 것이 있음 MAC 주소 테이블은 스위치의 포트번호와 해당 포트에 연결되어 있는 컴퓨터의 **MAC 주소가 등록되는 DB**이다
    
* 가령, 컴퓨터1이 컴퓨터 3에 데이터를 MAC 주소를 통해 전송한다고 해보자, MAC 주소 테이블에는 전송할 컴퓨터의 MAC 주소가 등록되어 있지 않다. 이 때 그냥 모든 컴퓨터에 전송해버리는 **플러딩 현상**이 일어난다. 이는 브로드캐스트와 유사한 상황이다.
    
    ![플러딩 현상](https://cdn.hashnode.com/res/hashnode/image/upload/v1681226490324/b16236b5-abfc-4dc6-b5be-b0031f59535a.png align="center")
    
    * MAC 주소가 저장 된 컴퓨터에 데이터를 보내면 해당 컴퓨터만 데이터를 받고 나머지 컴퓨터는 받지 않는다 이를 **‘필터링’** 이라고 한다.