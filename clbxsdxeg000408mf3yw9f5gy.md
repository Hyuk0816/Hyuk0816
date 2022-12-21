# [Jaba Study] Ep4. Adapter Pattern 연습

# 정의:

1) 호환성이 없는 기존 클래스의 인터페이스를 변환하여 사용자가 기대하는 인터페이스 형태로 변환시키는 패턴

2) 코드의 재활용성을 증가하고 기존의 코드를 수정하지 않는 장점

## 언제 사용?

1) 외부구성 요소를 기존 시스템에 재사용하고 싶지만 호환되지 않는 경우

2) 애플리케이션이 Client가 기대하는 인터페이스와 호환이 안 될 경우

3) 원본 코드를 수정하지 않고 애플리케이션에서 레거시 코드를 재사용 할 때

### **Q. 110V를 사용하는 국가에 220V를 사용하는 청소기를 작동 시켜라! (220V -&gt; 110V 변환)**

**Electronic 110V.Interface**

```java
public interface Electronic110V {
    void poweron();
}
```

**Electronic 220V.Interface**

```java
public interface Electronic220V {
    void connect();
}
```

**HairDryer(110V).class**

```java
public class HairDryer implements  Electronic110V{


    @Override
    public void poweron() {
        System.out.println("헤어 드라이기 110v on");
    }
}
```

**Cleaner(220V).class**

```java
public class Cleaner implements Electronic220V{
    @Override
    public void connect() {
        System.out.println("청소기 220V on!");
    }
}
```

**SocketAdapter(변환기).class**

```java
public class SocketAdapter implements  Electronic110V{
    private Electronic220V electronic220V;

    public SocketAdapter(Electronic220V electronic220V){
        this.electronic220V = electronic220V;
    }
    @Override
    public void poweron() {
        electronic220V.connect();
    }
}
```

**TestAdapter(main).class**

```java
public class TestAdapter {
    public static void main(String[] args) {
        HairDryer hairDryer = new HairDryer();
        connect(hairDryer);

        Cleaner cleaner = new Cleaner();
        //connect(cleaner);

        Electronic110V adpter = new SocketAdapter(cleaner);
        connect(adpter);        
    }
    public static void connect(Electronic110V electronic110V){
        electronic110V.poweron();
    }
}
```

**출력결과**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671634885887/xc7fdt96x.png align="center")

**\--&gt; 110V 인터페이스를 상속하는 SocketAdapter에 220V를 받는 생성자를 생성 후 110V의 poweron 메서드를 구현 TestAdapter 의 connect메서드를 220V poweron으로 생성하여 220V 객체가 SocketAdapter로 110V로 변환되어 잘 작동하는것을 볼 수 있다!**