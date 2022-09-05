# 문제
+ 삼각형을 나타내는 Triangle 클래스를 작성하라. 삼각형의 속성으로는 실수값의 밑변과 높이를, 동작으로는 넓이 구하기와 접근자가 있고 생성자도 포함한다. 작성한 클래스를 다음 코드를 사용해 테스트하라.
```java
public class TriangleTest {
    public static void main(String[] args) {
        Triagnle t = new Triangle(10.0, 5.0);
        System.out.println(t.findArea());
    }
}
```
# 답
```java
package chap04_challenge;

class Triangle {
	private double base,height;
	Triangle() {}
	Triangle(double base,double height) {
		this.base = base;
		this.height = height;
	}
	public double findArea() {
		double area = base * height / 2;
		return area;
	}

public class TriangleTest {

	public static void main(String[] args) {
		Triangle t = new Triangle(10.0,5.0);
        System.out.println(t.findArea());
	}

}    
```
## 출력
```
25
```