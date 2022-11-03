# [Network Study] Ep3. Host ,Switch, Network의 관계

**본 글은 유튜브 '널널한 개발자 TV' 의 네트워크 기초 강의를 보고 정리하여 작성한 글 입니다.**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667050581701/G9jXA1nAh.png align="left")

**Host는 Network를 이용하는 주체냐 Network 그 자체냐에 따라 분류** 

1) Network 이용 주체 = End-Point 

- Host가 존재하는 역할과 기능에 따라 Peer, Server, Client로 나눔 

- Client, Server가 제일 흔하고, 별개 없이 존재 시 Peer  


2) Netwrok 자체 = Switch 

- Router, FireWall, IPS 는 Switch라 부를 수 있고 역할에 따라 나눔. (Switching 역할) 

- Router는 경로 Switching,  FW, IPS는 보안 Switching  

--> Network 는 Router + DNS의 집합체이다. 


3) Switch 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667051233378/8HANF_NQx.png align="left")

--> 어떤 계층을 Switching 하느냐에 따라 Ln + Switch 

- Mac 주소로 Switching 하면 L2 Switch, IP 주소로 Switching 하면 L3 Switching, port로 하면 L4 Switching, HTTP에서 하면 L7 Switching 

- Layer가 높을수록 연산이 복잡해져서 가격이 비싸지고, 낮을수록 싸진다.

- 무엇으로 Switching 하는지가 정말 중요

- L3 Switch가 대표적으로 Router 

- Ethernet은 유선연결, 802.11~ 는 무선연결  




