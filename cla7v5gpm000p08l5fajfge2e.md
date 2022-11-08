# [Java Study] Ep2. ArrayList를 활용한 간단한 프로그램 만들기

**Q. 1001학번 Lee와 1002학번 Kim, 두 학생이 있습니다. 
Lee 학생은 국어와 수학 2과목을 수강했고, Kim 학생은 국어, 수학, 영어 3 과목을 수강하였습니다.
Lee 학생은 국어 100점, 수학 50점입니다. 
Kim 학생은 국어 70점, 수학 85점, 영어 100점입니다. 
Student와 Subject 클래스를 만들고 ArrayList를 활용하여 두 학생의 과목 성적과 총점을 출력하세요**

**실행해야하는 Test Code **


```
public class StudentTest {
	public static void main(String[] args) {
		Student studentLee = new Student(1001, "Lee");
		
		studentLee.addSubject("국어", 100);
		studentLee.addSubject("수학", 50);
		
		Student studentKim = new Student(1002, "Kim");
		
		studentKim.addSubject("국어", 70);
		studentKim.addSubject("수학", 85);
		studentKim.addSubject("영어", 100);
		
		studentLee.showStudentInfo();
		System.out.println("======================================");
		studentKim.showStudentInfo();
	}

}
``` 
**출력 예시**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667890521819/iBybR0AKb.png align="center")

1) Student class를 만들어 addsubject 메서드안에 들어갈 과목명, 점수 자료를 만들어주고 Student 객체가 생성되도록 메서드를 만들어 준다. 

2) addSubject 메서드와 showStudentInfo 메서드를 만든다

3) ArrayList를 활용하여 과목 리스트를 만들어주고 그 안에 과목: 점수 그 리스트를 Student 생성 메서드에 넣어준다  


```
public class Student {
	
	int studentId;
	String studentName;
	ArrayList<Subject> subjectList;
	
	public Student(int studentId, String studentName) {
		this.studentId = studentId;
		this.studentName = studentName;
		
		subjectList = new ArrayList<Subject>();
		
	}
	
	public void addSubject(String name, int score) {
		Subject subject = new Subject();
		 
		subject.setName(name);
		subject.setScorePoint(score);
		subjectList.add(subject);
		
	}
	
	public void showStudentInfo() {
		int total=0;
		
		for(Subject s : subjectList) {
			
			total += s.getScorePoint();
			System.out.println("학생" + studentName + "의" + s.getName() + "성적은" +
			s.getScorePoint() + "점 입니다.");			
		}
		System.out.println("총점은" + total +"점 입니다.");
	}
	
	
	

}
``` 
4) Subject class를 만들지 않으면 addSubject메서드와 showStudentInfo 메서드에서 과목명과 점수를 불러올 수 없기 때문에 Subject class를 생성해준다

5) Subject class 내에 addSubject 메서드에서 쓰인 과목명 String name과 과목점수 int 
scorePoint를 만들어준다. 


```
public class Subject {
	private String name;
	private int scorePoint;
	
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}

	public int getScorePoint() {
		return scorePoint;
	}

	public void setScorePoint(int scorePoint) {
		this.scorePoint = scorePoint;
	}

	

}

``` 

**출력 결과**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667890810871/Dgo-M5mIP.png align="left")




