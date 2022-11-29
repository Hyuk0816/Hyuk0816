# [Data Structures] Ep3. Linked List

## **1. Linked List**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669736275921/uDPvDO6xA.png align="center")

- 연결리스트라고 하고 순서대로 연결된 공간에 데이터를 나열하는 구조 

- 데이터를 연결 선(Node)으로 연결하여 연결된 리스트를 만듬 

- 데이터의 양 옆으로 연결되어 있는 링크드 리스트를 더블 링크드 리스트(이중 연결 리스트)라고도 함

- 장점: 

 - 미리 데이터 공간을 할당하지 않아도 된다 (기존 배열은 미리 공간을 할애 해야함 )

-단점: 

- 연결을 위한 별도 데이터 공간도 필요하므로, 저장공간 효율성이 떨어짐 

- 연결 정보를 찾는 시간이 필요하므로 접근 속도가 느림 

- 중간 데이터 삭제시, 앞뒤 데이터의 연결을 재구성하는 메서드 구현 해야함


### **EX) Single Linked List 구현 **


```
class SingledLinkedList<T>{
	
	public Node<T> head = null;
	//이너 클래스로 Node 생성 
	public class Node<T>{
		
		T data;
		Node<T> next = null;
		
		public Node(T data){
			this.data = data;
		}
		
	}
	
	//노드를 추가하는 메서드 생성 
	public void addNode(T data) {
		if(head == null) { //헤드가 없는 상황 즉, 데이터가 없으면 데이터를 추가하여 헤드로 만들자 
			head = new Node<T>(data); 
		}
		else {
			Node<T> node = this.head;
			while(node.next != null) {
				node = node.next;
			}
			node.next = new Node<T>(data);
		}
	}
	//데이터를 출력하는 매서드 생성 
	public void printAll() {
		
		if(head != null) {
			Node<T> node = this.head;
			System.out.println(node.data);
			while(node.next != null) {
				node = node.next;
				System.out.println(node.data);
				//System.out.println("===============");
			}
		}
		
	}
	
	//데이터를 조회하는 메서드 생성 
	public Node<T>Search(T data){
		
		if(this.head == null) { //헤드가 없는 경우는 데이터가 없는 경우이니 null로 반환 
			return null;
		}
		else {
			Node<T> node = this.head;
			while(node != null){ //node가 널이 아니라면 
				if(node.data == data) { 
					return node; //입력한 데이터가 내가 찾는 데이터면 그 데이터 반환 
				}
				else {
					node = node.next; //아니면 데이터 넘어가고 계속 위에 if 문을 반복 
				}
				
			}
			return null; //찾는 데이터가 없으면 null로 반환 
		}
	}
	
	//중간에 데이터를 추가해주는 메서드 생성 
	public void addNodeInside(T data, T isData) {
		Node<T> SearchedNode = this.Search(isData);
		
		if(SearchedNode == null) {
			this.addNode(data); //찾는 데이터가 없으면 데이터를 맨 뒤로 추가 시킴 
		}
		else {
			Node<T> nextNode = SearchedNode.next; 
			SearchedNode.next = new Node<T>(data); //찾는 데이터가 있으면 그 next에 노드를 생성 
			SearchedNode.next.next = nextNode; //찾는데이터 다 다음 데이터가 추가한 데이터 다음이 됨 
		}
	}
	
	//데이터를 삭제하는 메서드 생성 
	public boolean delNode(T isData) {
		if(this.head == null) {
			return false;
		}
		else {
			Node<T> node = this.head;
			if(node.data == isData) { //입력한 데이터가 head로 존재하는 데이터면 
				this.head = this.head.next; // head를 head 다음 데이터로 지정 
				return true;
			}
			else {
				while(node.next != null) { //head가 아니고 node 다음 데이터가 null이       
                                                          //아닐 시 
					if(node.next.data == isData) { //입력한 데이터가  head가 아니고 조회했을 때 나오는 데이터면 
						node.next = node.next.next; //그 조회 된 데이터 다음 데이터를 한칸씩 밀어버림 
						return true;
					}
					node = node.next;  
				}
				return false;
			}
		}
	}
	
}


``` 

**main Method로 구현**

```
	public static void main(String[] args) {
		SingledLinkedList<Integer> myLink = new SingledLinkedList<Integer>();
		
		myLink.addNode(1);
		myLink.addNode(2);
		myLink.addNode(3);
		myLink.printAll();
		System.out.println("============");
		
		myLink.addNodeInside(5, 2);
		myLink.printAll();
		System.out.println("============");
		
		myLink.delNode(2);
		myLink.printAll();
		
	}
``` 
**Single Linked List출력결과**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669736664744/ZlUQdckwD.png align="center")
 
### **EX2) Double Linked List 구현**


```
class DoubleLinkedList<T>{
	public Node<T>head = null;
	public Node<T>tail = null;
	
	public class Node<T>{
		T data;
		Node<T> prev = null;
		Node<T> next = null;
		
		public Node(T data) {
			this.data = data;
		}
	}
	
	//데이터를 추가한 메서드 생성 
	public void addNode(T data) {
		
		if(this.head == null) { //head가 없는경우 즉, 데이터가 아무것도 없는 경우 
			this.head = new Node<T>(data); //입력 데이터를 head로 
			this.tail = this.head;   // 입력 데이터를 tail로 
		}
		else {
			Node<T> node = this.head;
			while(node.next != null) {
				node = node.next;
			}
			
			node.next = new Node<T>(data);
			node.next.prev = node; //현재 데이터를 next가 가리키게 prev로 추가 
 			this.tail = node.next; //tail을 node의 다음 데이터로 지정 
		}
	}
	
	//데이터를 출력하는 메서드 생성 
	public void printAll() {
		if(this.head != null) {
			
			Node<T> node = this.head;
			System.out.println(node.data);
			while(node.next != null) {
				node = node.next;
				System.out.println(node.data);
			}
		}	
	}
	
	//head에서 특정 노드를 찾는 메서드 
	public T SearchedFromHead(T isData) {
		if(this.head == null) { //head가 없는 경우 데이터가 없는 거니까 null 반환 
			return null;
		}
		else {
			Node<T> node = this.head;
			while(node.next != null) {
				if(node.data == isData) { //head가 있고 node가 isdata면 그 데이터를 반환 
					return node.data;
				}
				else {
					node = node.next; //아니라면 다음 데이터로 넘겨서 계속 조회 
				}
			}
			return null;  //입력한 데이터가 리스트 안에 없으면  null 반환 
		}
	}
	
	public T SearchedFromTail(T isData) {		
		if(this.head == null) {
			return null;
		}
		
		else {
			Node<T> node = this.tail;
			while(node != null) { //tail이라 next가 없음 
				if(node.data == isData) {
					return node.data;
				}
				else {
					node = node.prev; // tail이니까 반대로 
				}
			}
		}
		return null; //데이터가 없으면 null로 반환 
	}
	//데이터를 기존 데이터 앞에 추가하는 메서드 구현 
	public boolean insertToFront(T existedData, T addData) {
		if(this.head == null) {
			this.head = new Node<T>(addData);
			this.tail = this.head;
			return true;
		}
		else if(this.head.data == existedData) { //head에 있는 데이터가 있는 데이터면 
				Node<T> newHead = new Node<T>(addData); //그 데이터를 새로운 head로 지정
				newHead.next = this.head; //새로운 head 데이터 next를 기존의 head로 지정 
				this.head = newHead;
				this.head.next.prev = this.head; // 기존 head (지금 2번째) 데이터를 추가한 head와 양뱡향 연결 
			}
		else {
			Node<T> node = this.head; 
			while(node != null) { //현재 node가 null이 아니라면 
				if(node.data == existedData) { //그 데이터가 현존 데이터면 //만약에 head가 아니라 head 이후의 데이터가 exist 면  
					Node<T> nodePrev = node.prev;   //이전 데이터의 주소를 가리키는 prev라는 인스턴스를 하나 만들어 주고 
					
					nodePrev.next = new Node<T>(addData); //prev의 다음 주소를 가리키는 데이터를 추가 데이터로 설정 
					nodePrev.next.next = node; //next next는 추가된 데이터 전의 next를 말함 (앞 if 문의 node) 
					
					nodePrev.next.prev = nodePrev; //새로 추가된 데이터의 이전 데이터를 기존 prev로 설정 
					node.prev = node.prev.next; //기준점이 된 node 의 prev를 node.prev.next로 설정 
					return true;
				}
				else {
					node = node.next;
				}
			}
			
		}return false;
		
	}
}

``` 
**main method 구현**


```
public static void main(String[] args) {
		
		DoubleLinkedList<Integer> myLink = new DoubleLinkedList<Integer>();
		myLink.addNode(1);
		myLink.addNode(2);
		myLink.addNode(3);
		myLink.printAll();
		System.out.println("===========");
		
		myLink.insertToFront(2, 7);
		myLink.printAll();
		
	}
``` 

**Double Linked List 출력결과**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669736920724/PAtnIwq0B.png align="center")


