# [Java Study] Ep3. Generic (1)

# **1. 제네릭의 정의**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669216024130/7OemKz6A1.png align="center")

- 클래스에서 사용하는 변수의 자료형이 여러개 일수 있고, 그 메서드가 동일한 경우 클래스의 자료형을 특정하지 않고 추후 해당 클래스를 사용할 때 지정 할 수 있도록 선언 --> 즉 메타몽 같은 녀석 

- 실제로 사용 중인 자료형의 변환은 컴파일러가 검증하므로 안정적인 프로그래밍 방식 구축 

- 컬렉션 프레임워크에서 많이 사용됨 


### EX) 제네릭을 사용하지 않는 경우:

**Q. 여러 material을 사용하여 출력하는 ThreeDPrinter 클래스를 구현하라 **

**material이 Powder인 경우**

```
public class ThreeDPrinter1{
	private Powder material;
	
	public void setMaterial(Powder material) {
		this.material = material;
	}
	
	public Powder getMaterial() {
		return material;
	}
}

``` 

**material이 Plastic인 경우**

```
public class ThreeDPrinter2{
	private Plastic material;
	
	public void setMaterial(Plastic material) {
		this.material = material;
	}
	
	public Plastic getMaterial() {
		return material;
	}

}

``` 
**material이 많아지니 최상위 클래스인 Object로 생성**

```
public class ThreeDPrinter{

	private Object material;
	
	public void setMaterial(Object material) {
		this.material = material;
	}
	
	public Object getMaterial() {
		return material;
	}
}

``` 
**But Object를 사용하는 경우는 다운캐스팅을 해줘야함**

```
ThreeDPrinter printer = new ThreeDPrinter();

Powder powder = new Powder();
printer.setMaterial(powder);

Powder p = (Powder)printer.getMaterial();

``` 

**--> 이처럼 재료가 계속 추가 될 수록 ThreeDPrinter Class 생성을 계속 해야하고 , Object 클래스를 사용하자니 계속 다운 캐스팅을 해야하는 불편함에 제네릭을 사용하게 됨 **

### EX) 제네릭을 사용하는 경우:

**material 클래스 생성 **

**Powder**
```
public class Powder {
	
	public String toString() {
		return "재료는 Powder 입니다";
	}
}

``` 

**Plastic**

```
public class Plastic {

	public String toString() {
		return "재료는 Plastic 입니다";
	}
}

``` 

**Generic을 사용하는 Printer 클래스 생성**

```
public class GenericPrinter<T> {
	private T material;   
	
	public void setMaterial(T material) {
		this.material = material;
	}
	
	public T getMaterial() {   
		return material;
	}
	
	public String toString(){
		return material.toString();
	}
}

``` 
**Printer 인스턴스를 생성하여 구현해보자**

```
public class GeneriPrinterTest {

	public static void main(String[] args) {

		GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
		powderPrinter.setMaterial(new Powder());
		System.out.println(powderPrinter);
		
		GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
		plasticPrinter.setMaterial(new Plastic());
		System.out.println(plasticPrinter);
		
	}

}

``` 

**출력결과**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669217084328/yxHzH9Buw.png align="center")

**--> Generic을 사용하면 재료별 Printer Class를 생성하지 않아도 되고 재료 클래스만 생성하여 '<>' 다이아몬드 연산자에 어떤 타입을 쓸건지 지정해주기만 하면 됨 또한, 다운 캐스팅을 하지 않아도 된다. **
















 
