# [Java Study] Ep7. Facade Pattern

## 정의:

여러개의 객체와 실제 사용하는 서브 객체의 사이에 복잡한 의존관계가 있을 때 중간에 Facade라는 객체를 두고 여기서 제공하는 Interface 만을 활용하여 기능을 사용하는 방식

\--&gt; Facade는 자신이 가지고 있는 각 클래스의 기능을 명확히 알아야함!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671637646541/aRJNnfzbc.png align="center")

### Q. 위 이미지 처럼 FTP, Writer, Reader를 가지고 있는 Facade를 만들어 Facade 패턴을 구현하라!

FTP.Class

```java
public class Ftp {

    private String host;
    private int port;
    private String path;

    public Ftp(String host, int port, String path){
        this.host = host;
        this.path = path;
        this.port = port;
    }

    public void conncet(){
        System.out.println("FTP Host :" + host + " Port :" + port + " 로 연결합니다.");
    }

    public void moveDirectory(){
        System.out.println("path :" + path + "로 이동합니다.");
    }

    public void disConnect(){
        System.out.println("FTP 연결을 종료 합니다.");
    }
}
```

Writer.Class

```java
public class Writer {
    private String fileName;

    public Writer(String fileName){
        this.fileName = fileName;

    }

    public void write(){
        String msg = String.format("Writer %s 로 파일쓰기를 합니다." , fileName);
        System.out.println(msg);
    }

    public void fileDisconnect(){
        String msg = String.format("Writer %s 로 연결 종료합니다." , fileName);
        System.out.println(msg);
    }

    public void fileConnect(){
        String msg = String.format("Writer %s 로 파일을 연결합니다." , fileName);
        System.out.println(msg);
    }
}
```

Reader.Class

```java
public class Reader {

    private  String fileName;

    public Reader(String fileName){
        this.fileName = fileName;
    }

    public void fileConnect(){
        String msg = String.format("Readr %s 로 연결 합니다." , fileName);
        System.out.println(msg);
    }

    public void fileRead(){
        String msg = String.format("Readr %s 로 내용을 읽어 옵니다." , fileName);
        System.out.println(msg);
    }

    public void fileDisconnect(){
        String msg = String.format("Readr %s 로 연결 종료합니다." , fileName);
        System.out.println(msg);
    }
}
```

SftpClient(Facade).Class

```java
public class SftpClient {

    private Ftp ftp;
    private Reader reader;
    private Writer writer;

    public SftpClient(Ftp ftp, Reader reader, Writer writer){
        this.ftp = ftp;
        this.reader = reader;
        this.writer = writer;
    }
    public SftpClient(String host, int port, String path, String fileName){
        this.ftp = new Ftp(host, port, path);
        this.reader = new Reader(fileName);
        this.writer = new Writer(fileName);
    }

    public void conncet(){
        ftp.conncet();
        ftp.moveDirectory();
        writer.fileConnect();
        reader.fileConnect();
    }

    public void disConnect(){
        writer.fileDisconnect();
        reader.fileDisconnect();
        ftp.disConnect();
    }

    public void read(){
        reader.fileRead();
    }

    public void write(){
        writer.write();
    }
}
```

TestFacade.Class

```java
public class TestFacade {
    public static void main(String[] args) {
//      Facade 사용 안할 시         
//        Ftp ftpClient = new Ftp("www.foo.co.kr", 22, "/home/etc");
//        ftpClient.conncet();
//        ftpClient.moveDirectory();
//
//
//        Writer writer = new Writer("text.tmp");
//        writer.fileConnect();
//        writer.write();
//
//        Reader reader = new Reader("text.tmp");
//        reader.fileConnect();
//        reader.fileRead();
//
//        reader.fileDisconnect();
//        writer.fileDisconnect();
//        ftpClient.disConnect();


        //Facade 사용 
        SftpClient sftpClient = new SftpClient("www.foo.co.kr", 22, "/home/etc", "text.tmp");
        sftpClient.conncet();

        sftpClient.write();

        sftpClient.read();

        sftpClient.disConnect();
    }
}
```

**\--&gt; Facade 패턴을 사용하고 안하고 코드의 간결함이 차이가 큰 거같다.,.! 사용 안할 시 객체를 다 만들어줘야하는 반면, Facade 사용 시 Facade 객체인 SftpClient를 구현하기만 하면 된다,,!**