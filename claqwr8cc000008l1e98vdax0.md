# [Data Structures] Ep2. Stack

**1. Stack 이란? **

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669041198574/YhAQ-Jvta.png align="center")

- 흔히 **'스택을 쌓는다'** 라고 이야기 하듯이,  LIFO(Last in, First Out) 형식의 자료구조 

- 책을 쌓고 맨 위에 책을 먼저 빼듯이, 맨 마지막에 입력된 데이터를 제일 첫 번째로 반출 

-  push()로 데이터를 스택에 넣고, pop()으로 데이터를 스택에서 반출 


**2. JAVA 에서의 Stack**

- java.util 패키지에서 stack 클래스를 제공함 (Que와 마찬가지)

- push(data), pop() 메서드로 데이터를 추가 또는 반출 


**Q. push와 pop으로 데이터를 추가 제거하는 것을 구현하고 더 이상 pop 할게 없을 시 "데이터가 없습니다" 라는 문구를 출력하도록 구현하라**


```
import java.util.ArrayList;

class MyStack<T>{
	
	private ArrayList<T> mystack = new ArrayList<T>();
	
	public void push(T item) {
		
		mystack.add(item);
		
	}
	
	public T pop() {		
		if(mystack.isEmpty()) {
			System.out.print("데이터가 없습니다. ");
			return null;		
		}
		
		return mystack.remove(mystack.size()-1);
		//인덱스 번호를 지정하여 맨 마지막 데이터 뺴기 
		//ex) 데이터를 1,2,3 을 넣었다면 stack.size를 이용해 3도출 
		//거기에 -1을 해주어 2 값을 도출하여 
		// 0번부터 시작하는 인덱스 처리
	}
	
	public boolean isEmpty() {
		
		return mystack.isEmpty();
	}
}



public class Stack_test {
	public static void main(String[] args) {
		
		MyStack<Integer> st = new MyStack<Integer>();
		
		st.push(1);
		st.push(2);
		System.out.println(st.pop());
		st.push(3);
		
		System.out.println(st.pop());
		System.out.println(st.pop());
		System.out.println(st.pop());
		
		System.out.println(st.isEmpty());
		
		
	}

}

``` 

**출력결과**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669042297389/O_kAgVqlU.png align="center")

***스택 또한 운영체제에서 프로세스 구조의 함수 동작 방식에 사용되니 운영체제 공부할 때 자세히 보자!***


 
  