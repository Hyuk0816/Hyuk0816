# [Java Study] Ep6. Observer Pattern

## 정의:

1) 옵저버 패턴은 변화가 일어났을 때, 미리 등록된 다른 크래스에 통보해주는 패턴을 구현한 것이다.

2) 주체 객체와 상태의 변경을 알아야하는 관찰객체가 존재하며 이들의 관계는 1:1, 1:N이 될 수 있다.

## 장점:

1) 실시간으로 한 객체의 변경사항을 다른 객체에 전파가능

2) **느슨한 결합**으로 시스템이 유연하고 객체간 의존성 제거

## 단점:

1) 너무 많이 사용하게되면 상태관리가 힘들 수 있음

2) 데이터 배분에 문제가 생기면 자칫 큰 문제 야기

### Q. 클릭 할 때마다 알려주어 메세지를 출력하는 옵저버 패턴을 구현해라!

**IButtonListener.Interface**

```java
public interface IButtonListener {
    void clickEvent(String event);
}
```

**Button.Class**

```java
public class Button {

    private String name;
    private IButtonListener buttonListener;

    public Button(String name){
        this.name = name;
    }

    public void click(String message){
        buttonListener.clickEvent(message);
    }

    public void addListener(IButtonListener buttonListener){
        this.buttonListener = buttonListener;
    }
}
```

**TestObserver.Class**

```java
public class TestObserver {
    public static void main(String[] args) {
        Button button = new Button("버튼");

        button.addListener(new IButtonListener() {
            @Override
            public void clickEvent(String event) {
                System.out.println(event);
            }
        });
        button.click("메세지 전달: click1");
        button.click("메세지 전달: click2");
        button.click("메세지 전달: click3");
        button.click("메세지 전달: click4");
    }
}
```