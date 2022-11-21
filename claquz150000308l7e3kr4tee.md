# [Data Structures] Ep1. Queue

**1. 큐 구조 (QUE) **


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669038813438/ub3vGlHrT.png align="center")

-  줄을 서는 행위와 유사 

- 가장 먼저 넣은 데이터를 가장 먼저 꺼내는 구조 (선입선출(FIFO)) 

- LIFO(Last In, First Out)구조인 스택과는 반대 

- Enque로 que에 데이터를 넣고, Deque로 데이터를 꺼냄 


**2. JAVA에서의 큐 구현**

-  JAVA 에서는 java.util 패키지에서 Queue 클래스를 제공 

- Enque에 해당하는 기능으로 add(), offer() 메서드를 제공함 

- Deque에 해당하는 기능으로 poll() 또는 remove() 메서드를 제공함 

- Queue 클래스의 데이터 생성을 위해서는 java.util 패키지에 있는 LinkedList 클래스를 사용해야 함


**2-1 코드로 큐 구현해보기**

**Q. ArrayList를 활용하여 enque, deque의 기능을 구현해보기 **


```
import java.util.ArrayList;

class Myque<T>{
	
	private ArrayList<T> queue = new ArrayList<T>();
	
	public void enqueue(T item) {
		
		queue.add(item);
	}
	
	public T dequeue() {
		
		if(queue.isEmpty()) {
			return null;
		}
		
		return queue.remove(0);
	}
	
	public boolean isEmpty() {
		
		return queue.isEmpty();
	}
}

public class Queue_test {
	public static void main(String[] args) {
		
		Myque<Integer> mq = new Myque<Integer>();
		
		mq.enqueue(1);
		mq.enqueue(2);
		mq.enqueue(3);
		
		System.out.println(mq.dequeue());
		System.out.println(mq.dequeue());
		System.out.println(mq.dequeue());
		
		System.out.println(mq.isEmpty());
	}

}

``` 

**ArrayList의 IsEmpty를 활용하여 더이상 Deque 할 데이터가 없으면 null로 반환 **

**출력결과**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669039326130/Br-p3XjYj.png align="left")

***Que는 운영체제에서 프로세스 스케줄링 방식에 많이 쓰인다 하니, 운영체제를 공부하면서 더 자세히 다뤄보자!!***
 




 