# [Java Study] Ep1. Singletone 패턴 연습

**Q. 자동차 공장이 있습니다. 자동차 공장은 유일한 객체이고, 이 공장에서 생산되는 자동차는 제작될 때마다 고유의 번호가 부여됩니다. 
자동차 번호가 10001부터 시작되어 자동차가 생산될 때마다 10002, 10003 이렇게 번호가 붙도록 자동차 공장 클래스, 자동차 클래스를 구현하세요
다음 CarFactoryTest.java 테스트 코드가 수행 되도록 합니다.
**

**Test Code**
```
       public class CarFactoryTest {
	   public static void main(String[] args) {
	   CarFactory factory = CarFactory.getInstance();
		
		Car mySonata = factory.createCar();
		Car yourSonata = factory.createCar();
		
		
		System.out.println(mySonata.getCarNum());
		System.out.println(yourSonata.getCarNum());
		
	}
		
}
``` 

1) 자동차 공장은 유일한 객체이고 --> 유일한 객체이므로 Private으로 지정 후 인스턴스를 얻을 수  있도록 getInstance 메서드 생성 

2) factory.createCar() 이므로 Car를 생성하는 createCar 메서드 생성 


```
public class CarFactory {
	
	
	private CarFactory() {}
	
	
	private static CarFactory instance = new CarFactory();
	
	public static CarFactory getInstance() {
		
		if( instance == null) {
			instance = new CarFactory();
		}
		return instance;
			
	}
	
	public Car createCar() {
	
		Car car = new Car();
		return car; // 새로운 차를 생성하는 createCar()메서드를 구현
	}
	

}
``` 
3) 새로운 Car 객체를 생성해 Car 객체에서 getCarNum 이라는 메서드를 사용하여 차의 고유 번호를 뽑아야 하기 때문에 Car 클래스 생성 

4) carNum 이라는 int 자료형과 static int 자료형인 serialNum을 생성하여 get, set 메서드와 Car() 메서드 안에서 serialNum++ 와 계속 더해진 serialNum이 carNum이 되게 serialNum = carNum  적용 

```
public class Car {
	

	
	public int carNum;
	static int serialNum = 10000;
	
	
	Car() {
		serialNum++;
		carNum = serialNum;
	}
	
	public int getCarNum() {
		return carNum;
	}

	public void setCarNum(int carNum) {
		this.carNum = carNum;
		serialNum++;
		carNum = serialNum;
	}	

}
``` 

**출력**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667401979783/v3fJHWd4-.png align="left")

매우 장황하게 적어버렸다,,, 계속 쓰면서 보완해 나가야지 :) 




