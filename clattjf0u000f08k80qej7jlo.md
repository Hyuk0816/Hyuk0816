# [Java Study] Ep3. Generic (2)

# **1. T extends Class 사용**

- 상위 클래스를 상속 받음으로 써 T 자료형의 범위를 제한할 수 있음

- 상위 클래스에서 선언하거나 정의하는 메서드 사용가능 

- 상속을 받지 않는 경우 T type은 Object로 변환되어 Obj 클래스가 기본으로 제공되는 메서드만 사용가능 


**Q. 재료의 범위를 제한함으로써 3D 프린터의 기능을 구현하라 (재료는 Powder와 Plastic)**

**Powder와 Plastic을 상속하는 material 추상 클래스 생성**

```
public abstract class Material {
	
	public abstract void doPrinting();
}

``` 

**material을 상속받는 Powder와 Plastic Class 생성**
 
```
public class Powder extends Material{
		
	public void doPrinting() {
		System.out.println("Powder 재료로 출력합니다");
	}
	
	public String toString() {
		return "재료는 Powder 입니다";
	}
}

``` 


```
public class Plastic extends Material{

	public void doPrinting() {
		System.out.println("Plastic 재료로 출력합니다");
	}
	
	public String toString() {
		return "재료는 Plastic 입니다";
	}
}

``` 
**material을 상속받는 GenericPrinter Class 생성 **

```
public class GenericPrinter<T extends Material> {
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
	
	public void printing() {
		material.doPrinting();
	}
}

``` 

**Powder와 Plastic만 생성되는 프린터 구현**

```
public class GenericPrinterTest {

	public static void main(String[] args) {

		GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
		powderPrinter.setMaterial(new Powder());
		Powder powder = powderPrinter.getMaterial(); 
		System.out.println(powderPrinter);
		
		GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
		plasticPrinter.setMaterial(new Plastic());
		Plastic plastic = plasticPrinter.getMaterial(); 
		System.out.println(plasticPrinter);
		

		//GenericPrinter<Water> printer = new GenericPrinter<Water>();
           //--> material를 상속하지 않아 생성불가 	
	}
}

``` 







