# [Java Study] Ep5. Decorator Pattern

## 정의:

주어진 상황 및 용도에 따라 어떤 객체에 책임 또는 기능을 추가하는 패턴

## 장점:

1) 기존 작성한 코드를 수정하지 않고 Decorator 패턴을 이용해 확장 가능

2) 구성과 위임을 통해 실행중에 새로운 기능 or 행동 추가 가능

## 단점:

1) 의미 없는 객체가 너무 많아져서 코드가 복잡해짐

## 언제사용?

1) 클래스의 요소들을 계속해서 수정작업하면서 사용하는 구조가 필요한 경우

2) 여러 요소들을 조합해서 사용하는 클래스 구조인 경우

### Q. Audi를 Decorator 패턴을 이용하여 여러 종류의 Audi를 구현

**ICar.Interface**

```java
public interface ICar {

    int getPrice();
    void showPrice();
}
```

**Audi.Class**

```java
public class Audi implements ICar{


    private int price;

    public Audi(int price){
        this.price = price;
    }

    @Override
    public int getPrice() {
        return price;
    }

    @Override
    public void showPrice() {
        System.out.println("Audi의 가격은 " + this.price + "원 입니다.");
    }
}
```

**AudiDecorator.Class**

```java
public class AudiDecorator implements ICar{

    protected ICar audi;
    protected String modelName;
    protected int modelPrice;

    public AudiDecorator(ICar audi, String modelName, int modelPrice){
        this.audi = audi;
        this.modelName = modelName;
        this.modelPrice = modelPrice;
    }

    @Override
    public int getPrice() {
        return audi.getPrice() + modelPrice;
    }

    @Override
    public void showPrice() {
        System.out.println(modelName + "의 가격은" + getPrice() + "원 입니다.");
    }
}
```

**A3.Class**

```java
public class A3 extends AudiDecorator{

    public A3(ICar audi, String modelName) {
        super(audi, modelName, 1000);
    }
}
```

**A4.Class**

```java
public class A4 extends AudiDecorator{

    public A4(ICar audi, String modelName) {
        super(audi, modelName, 2000);
    }
}
```

**A5.Class**

```java
public class A5 extends AudiDecorator{

    public A5(ICar audi, String modelName) {
        super(audi, modelName, 3000);
    }
}
```

**Test.Class**

```java
public class Test {
    public static void main(String[] args) {

        ICar audi = new Audi(1000);
        audi.showPrice();

        //a3
        ICar audi3 = new A3(audi, "A3");
        audi3.showPrice();
        //a4
        ICar audi4 = new A4(audi, "A4");
        audi4.showPrice();
        //a5
        ICar audi5 = new A5(audi, "A5");
        audi5.showPrice();
    }
}
```

**AudiDecorator를 상속받는 각각의 A3,A4,A5 의 클래스 및 객체를 사용하여 기존의 Audi 클래스는 조작하지 않고 Decorator를 통해 기능 또는 다른 종류를 만들 수 있다!**