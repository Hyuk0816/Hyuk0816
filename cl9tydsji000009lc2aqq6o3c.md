# [Network Study] Ep2. Port 번호, IP 주소, MAC 주소의 식별자

**본 글은 유튜브 '널널한 개발자 TV' 의 네트워크 기초 강의를 보고 정리하여 작성한 글 입니다.**


**1) MAC 주소 (NIC, LAN 카드에 대한 식별자) **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667049098657/hc54wxbBc.png align="left")


- LAN 카드라고 하는 것들은 무조건 MAC 주소를 가지고 있다. 

- 유/무선 LAN 카드를 동시에 가질 경우 MAC 주소는 2개! 

- 자주 변경되는 것은 아니지만 변경 가능하다

- H/W 주소라고도 한다. 

**2) IP 주소 (Host에 대한 식별자) **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667049333105/8EpIGObOv.png align="left")


- Host란 인터넷에 연결된 컴퓨터를 말한다. 

- IP 주소는 컴퓨터(Host)에 부여됨 

- 한 컴퓨터가 N개의 IP 주소를 가질 수 있음 

- IP 주소는 NIC 하나에 여러 개를 맵핑(바인딩) 할 수 있다. 

**3) Port 번호 (어떤 Layer에서 관련된 일을 하는지에 따라 여러가지 형태로 식별의 대상이 달라진다 **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667049557475/74Ik31Sm0.png align="left")

- SW 개발자: 프로세스 식별자 

- 네트워크 관리자(4계층) : 서비스 식별자 

- 하드웨어 기사: 인터페이스 번호 - 공유기의 LAN 케이블 번호 





