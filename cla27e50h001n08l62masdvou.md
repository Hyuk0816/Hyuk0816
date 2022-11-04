# [AWS-SAA-Study] Ep2. LoadBalancing, ASG

**1. Scalability & High Availability (확장성 & 고가용성)**

1) Scalability (확장성) 

- 확장성은 애플리케이션 시스템이 조정을 통해 더 많은 양을 처리할 수 있다는 의미 

Vertical Scalability (수직 확장성)

-	수직 확장성은 인스턴스의 크기를 증가시키는 것 -> 인스턴스의 개수가 아닌 인스턴스의 성능을 업그레이드 하는 것 ex) t2.micro -> t2.large 
-	수직 확장성은 분산시스템이 아닌 데이터베이스 같은 작업에 적합 (RDS, Elastic Cache) 
-	RDS, Elastic Cache는 하위 인스턴스의 유형을 업그레이드해 수직적으로 확장할 수 있는 시스템들
-	일반적으로 수직적으로 확장하는데 한계가 있음 (하드웨어 제한) 


Horizontal Scalability (수평 확장성) 

-	수평 확장성은 애플리케이션에서 인스턴스나 시스템의 수를 늘리는 방법 
-	같은 인스턴스를 여러 개 확장하는 것! (멀티 코어) 
-	수평 확장은 분산 시스템 – 웹 app 이나 현대 app 이 분산시스템 
-	EC2 덕분에 수평 확장이 쉬워짐 



2) High Availability (고가용성) 

- 고가용성이란 APP이나 시스템을 적어도 두개의 AZ에서 돌리고 있는 걸 의미 
- 고가용성의 목표는 Data Center의 손실로부터 살아남는 것 
- RDS나 다수의 AZ일 경우 수동형 고가용성 확보 
- 수평 확장을 할 때 고가용성이 활성형이 됨 
- 동일 애플리케이션의 동일 인스턴스를 다수의 AZ에 걸쳐 실행함 (ASG, ELB에 사용)  

**2. Load Balancing (로드 밸런싱) **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667547681774/SrnM4a-Vs.png align="center")

- 로드 밸런서는 트래픽을 백엔드나 다운스트림 EC2 인스턴스 또는 서버들로 전달하는 역할을 함 
- 트래픽이 인스턴스에 가기 전 로드 밸런서를 거쳐서 트래픽을 여러 인스턴스로 분산시키면서 인스턴스의 부하를 막음 

- ELB(Elastic Load Balancer)는 관리형 로드 밸런스를 의미함

- ELB는 AWS가 관리하며 어떤 경우에도 작동할 것을 보장, AWS가 업그레이드와 유지 관리 및 고가용성을 책임지며, 로드 밸런서의 작동 방식을 수정할 수 있게끔 일부 구성 놉도 제공 

- ELB를 사용하는 것이 자체 로드밸런서를 사용하는 것보다 비용도 싸고 덜 복잡함 

- ELB는 EC2, ASG, ECS 등 AWS 서비스와 통합이 가능함 

2-1) Health Checks (상태확인)
 
- 상태확인은 ELB가 EC2 인스턴스의 작동이 올바르게 되고 있는지 여부를 확인하기 의해 사용 함 
- 만약 인스턴스가 제대로 작동 중이 아니라면 해당 인스턴스로는 트래픽을 보낼 수 없기 때문에 상태확인을 통해 비정상적인 인스턴스로는 트래픽이 가지 않게 함 

- 정상일 때 HTTP 상태코드로 200, 나머지는 비정상 

3. Load Balancers (CLB, ALB, NLB, GWLB) 

1) CLB (Classic Load Balancers) 

- TCP(Layer 4) , HTTP, HTTPS(Layer 7)를 지원 

2) ALB (Application Load Balancers)
 
- HTTP(Layer 7) 전용 로드 밸런서 
- 머신 간 다수 HTTP 애플리케이션의 **라우팅**에 사용이 됨  

*라우팅: 어떤 네트워크 안에서 통신 데이터를 보낼 때 최적의 경로를 선택하는 과정. 최적의 경로는 주어진 데이터를 가장 짧은 거리로 또는 가장 적은 시간 안에 전송 할 수 있는 경로*

- 동일 EC2 인스턴스 상의 여러 애플리케이션에 부하를 분산 (컨테이너 & ECS 사용)
 
**- HTTP/2 **와 **WebSocket**을 지원

*HTTP2: 월드 와이드 웹에서 쓰이는 HTTP 프로토콜의 두 번째 버전이다. SPDY에 기반하고 있으며, 국제 인터넷 표준화 기구(IETF)에서 개발되고 있다.*

*WebSocket: WebSocket은 서버와 클라이언트 간에 Socket Connection을 유지해서 언제든 양방향 통신 또는 데이터 전송이 가능하도록 하는 기술이다. Real-time web application구현을 위해 널리 사용되어지고 있다. (SNS어플리케이션, LoL같은 멀티플레이어 게임, 구글 Doc, 증권거래, 화상채팅 등) *

- **redirects**도 지원 (HTTP 에서 HTTPS 로)

*Redirect: 리다이렉트란 말 그대로 re(다시) + 지시하다(direct) 다시 지시하는 것을 말한다.예를 들어 브라우저가 www.webstone.com/blogA URL을 웹 서버에 요청했다고 하자 그러면 서버는 HTTP 응답 메시지를 통해 "www.webstone.com/blogB 로 다시 요청해봐!~" 라고 브라우저에게 다른 URL(길, 방향) 을 지시할 수 있는 것을 리다이렉트라 한다*
 
*EX) 네이버 카페 등급 권한 없는 글 열람 시 다른 페이지로 리다이렉트*

- URL 경로에 따른 라우팅 (example.com/users나 example.com/posts) 

- URL 호스트 이름에 기반한 라우팅 (naver.com 또는 never.com)

- 쿼리 문자열과 헤더에 기반한 라우팅도 가능 
(example.com/usersid=123&order=false가 다른 대상 그룹에 라우팅 )

- ALB는 마이크로 서비스나 컨테이너 기반 애플리케이션에 가장 좋은 로드 밸런서 (Amazon ECS, Docker에 적합) 

- CLB는 여러 개의 애플리케이션을 실행하면 그 수만큼의 CLB가 필요하지만, ALB는 하나의 ALB로 다수의 애플리케이션 처리 가능 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548056358/K_SZiBzzi.png align="center")

**2-1) ALB Target Group**

- 타겟그룹이란 EC2인스턴스를 오토스케일링 할 수 있는 단위로 사용됩니다. 각각의 타겟그룹에 있는 인스턴스들은 정의된 Health Checks(상태 확인)를 수행하게됩니다.

- EC2, ECS, 람다, IP 주소가 대상 그룹이 될 수 있음 

- ALB는 다수의 대상 그룹에 라우팅 가능 

- 상태 확인은 대상 그룹 레벨에서 확인 가능 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548117080/gBUNssQi3.png align="center")

- 클라이언트가 사용하려는 URL에 ?Platform=Mobile이 있는 경우 첫 번째 대상 그룹으로 리다이렉팅 되도록ALB에서 리다이렉션 라우팅 규칙을 만들 수 있고

- ?Platform=Desktop이 있는, 즉 쿼리 문자열 또는 매개변수인 경우 두 번째 대상 그룹으로 리다이렉트 하도록 만들 수 있음

**3) NLB (Network Load Balancer) **

- Layer 4 (TCP or UDP) 밸런서로 낮은 계증의 밸런서 

- 초당 수백만 건의 요청을 가능 (매우 고성능) 

- ALB 보다 지연시간이 짧음 

- NLB의 가장 큰 장점은 고정 IP를 갖는 것으로 IP를 통해 접근 제어를 수행하고자 할 경우, 변하지 않는 IP는 매우 중요한 요소가 됨 반면 ALB는 IP가 끊임없이 변화함 

- NLB는 고성능이나 **TCP**, **UDP** 수준의 트래픽을 원할 때 사용함 

*TCP: TCP (전송 제어 프로토콜)은 두 개의 호스트를 연결하고 데이터 스트림을 교환하게 해주는 중요한 네트워크 프로토콜이다. TCP는 데이터와 패킷이 보내진 순서대로 전달하는 것을 보장해준다. *

*UDP: 간단하게 TCP의 모든 신뢰성 기능이 없다고 보면 된다. 상대와 접속했고, 전송속도를 48 kbps로 설정했으면 48 kbps로 데이터를 전송하기만 한다. 받는 쪽에서 데이터를 제대로 받고 있는지조차도 신경 안 쓴다. 그렇기 때문에 UDP로 자료를 제공할 경우 32 kbps, 48 kbps, 64 kbps와 같이 다양한 전송률 옵션을 선택하는 형태로 제공된다.
신뢰성이 보장되지 않기 때문에 UDP로 데이터를 보내면 손실되는 데이터가 발생할 수 있다.[1] 하지만 동영상의 경우를 생각할 때 데이터가 왕창 소실됐다면 괴이한 화면이 나올 수도 있지만, 데이터 몇 개 소실되어봤자 전체 화면에서 일부 구역이 제대로 안나오는 수준에 불과하다.[2] 그렇기에 사람들이 크게 불평하지 않을 수준의 영상만 제공할 수 있다면 느린 TCP를 쓸 이유가 없는 셈이다. 그렇기에 실시간 스트리밍을 하는 곳에서 주로 사용한다.*


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548186345/xQ8njBax1.png align="center")

- NLB는 Layer 4에 있는 밸런서 답게 EC2, 사설 IP, ALB를 대상 그룹으로 가질 수 있음
 
- ALB 앞에 NLB를 두어 고정IP를 활성해 ALB의 IP 변환 문제를 해결 


4) GWLB (Gateway Load Balancer) 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548205636/KOPGnJwRW.png align="center")

- GWLB는 배포 및 확장과 AWS의 타사 네트워크 가상 어플라이언스의 플릿 관리에 사용 

- GWLB는 네트워크의 모든 트래픽이 방화벽을 통과하게 하거나 침입 탐지 및 방지 시스템에 사용 

- 모든 사용자 트래픽은 GWLB를 통과 -> GWLB에서 가상 어플라이언스로 확대 -> 가상 어플라이언스에서 트래픽 분석 처리 후 이상이 없으면 다시 GWLB 있으면 트래픽 드롭 -> 애플리케이션으로 트래픽 도착 

- Transparent network gateway: VCP의 모든 트래픽이 GWLB가 되는 단일 엔트리와 출구를 통과 

- 대상 그룹은 EC2, 사설 IP 주소

**4. Sticky Session (고정세션, Session Affinity , 세션 밀접성) **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548292672/IHNvpQvrN.png align="center")

-  고정성이란 같은 사용자가 LB에 요청을 했을 경우 동일한 인스턴스로 연결되는 것을 말함
 
- 이는 애플리케이션 밸런서가 모든 EC2 전반으로 모든 요청을 확산하는 것과는 다른 동작임

- CLB와 ALB 에도 설정할 수 있음 

- 기간 만료도 있으며, 기간이 만료되면 다른 인스턴스로 리디렉션 됨 

- 세션 만료 사용 시, 사용자의 로그인 데이터와 같은 중요한 데이터를 잃지 않기 위해 사용자가 동일한 백엔드 인스턴스에 연결, 고정성을 활성화 하면 백엔드 EC2 인스턴스 부하에 불균형을 초래 할 수 있음 

Application-based Cookie 

 사용자 정의 쿠키 

 -	애플리케이션에 필요한 모든 사용자 정의 속성 포함 가능 
 -	쿠키 이름은 각 대상 그룹별 개별적으로 지정 (AWSALB,AWSALBAPP x) 

Application Cookie

-	로드 밸런서 자체에서 생성
-	쿠키 이름은 AWSALBAPP 임 

Duration-based Cookie (기간 기반 쿠키)
 
- 로드밸런서에 의해 생성 
- ALB 에서는 AWSALB , CLB 에서는 AWSELB 임 
- 특정 기간을 기반으로 만료되며 그 기간이 로드 밸런서 자체에서 생성 됨 

**5. Cross-Zone Load  Balancing (교차 영역 로드밸런싱)**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548352629/Ak8-NmTk-.png align="center")

교차 영역 로드 밸런싱 (Cross Zone Load Balancing)
 
-  모든 AZ에 걸친 인스턴스의 개수에 따라 트래픽이 분산 됨 
-  ALB: 디폴트로 설정 됨, AZ 간 데이터 전송비용 없음 
-  NLB: 디폴트로 비활성화, 활성화에 비용 지불 
-  CLB: 기본으로 비활성화, 활성화 시 AZ간 데이터 전송비용 없음


Without Cross Zone Load Balancing 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548414404/0NNSmGeoP.png align="center")

- 노드에 의해서 절반 된 트래픽에 각 AZ에 인스턴스 개수만큼 트래픽이 분산 

**6. SSL / TLS **

- SSL 인증서를 사용하면 클라이언트와 LB 사이에서 전송 중에 있는 트래픽 암호화 가능
 
- 인-플라이트 암호화: 데이터가 네트워크를 통과하는 중에 암호화가 되고 발신자와 수신자만 해독 가능 

- TLS가 SSL의 최신 기술이고 TLS도 통칭으로 SSL 이라 함
 
- .SSL 인증서는 기간이 있고 주기적으로 갱신되야 함 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548450480/S4hrV846C.png align="center")

- 유저가 HTTPS 를 통해 LB와 연결 -> LB가 내부적으로 SSL 인증서 종료 작업 수행 -> LB가 백엔드에서 사설 VPC로 트랙픽 전송 -> X.509 인증서 불러옴(SSL TLS)
 
- AWS에서 ACM을 사용해 SSL 인증서를 관리 가능 


**6-1 SNI (Server Name Indicator)**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548481609/HUpSQBPCg.png align="center")

- SNI를 사용하면 한 웹서버 상의 다수의 SSL 인증서를 발급해 단일 웹서버가 여러 개의 웹사이트를 제공하도록 하는 문제 해결 

- 클라이언트가 초기 SSL 핸드셰이크에서 대상서버의 호스트 네임을 명시해야함 

- 클라이언트가 연결하고자 하는 웹사이트의 이름을 대면 서버가 어떤 인증서를 불러올지 알수있음 

- ALB와 NLB 에서만 사용 가능 (CLB 지원 불가) 


**6-2 ELB – SSL Certificates**

-  CLB : SSL 인증서 하나만 지원, 여러 SSL 인증서가 있는 다중 호스트 이름이 필요한 경우 다수의 CLB를 사용하는게 가장 좋음

- ALB: 다중 SSL 인증서를 가진 다수의 리스너를 지원, SNI 사용
 
- NLB: 다중 SSL 인증서를 가진 다수의 리스너를 지원, SNI 사용 


**6-3 Connection Draining**

Feature naming

- Connection Draining – CLB 
- Deregistration Delay (등록 취소 지연) – ALB, NLB 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667548538581/x27y-c1V7.png align="center")

- 인스턴스가 비정상적인 상태일 때 인스턴스에 어느정도 시간을 주어 인-플라이트 요청, 활성요청을 완료하는 기능 
 
- 연결이 드레이닝되면 즉 인스턴스가 드레이닝되면 ELB는 등록 취소 중인 EC2 인스턴스로 새로운 요청을 보내지 않음
 
- 즉, ELB가 인스턴스를 등록 취소 전에 기존에 있던 연결과 요청을 완료하고 연결이 정지
 
- 1초 to 5분 까지 시간 설정 가능 (0초면 드레이닝 비활성화) 


