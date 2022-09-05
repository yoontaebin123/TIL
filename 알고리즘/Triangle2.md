# 문제
+ Triangle 클래스에 2개의 삼각형 넓이가 동일한지 비교하는 isSameArea() 메서드를 추가하라. 그리고 다음 코드를 사용해 테스트하라.
```java
public class TriangleTest {

	public static void main(String[] args) {
		Triangle t1 = new Triangle(10.0,5.0);
		Triangle t2 = new Triangle(5.0,10.0);
		Triangle t3 = new Triangle(8.0,8.0);

		System.out.println(t1.isSameArea(t2));
		System.out.println(t1.isSameArea(t3));

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
	public boolean isSameArea(Triangle obj) {
		if(base * height == obj.base * obj.height) {
			return true;
		}
		else {
			return false;
		}
	}
}

public class TriangleTest {

	public static void main(String[] args) {
		Triangle t1 = new Triangle(10.0,5.0);
		Triangle t2 = new Triangle(5.0,10.0);
		Triangle t3 = new Triangle(8.0,8.0);

		System.out.println(t1.isSameArea(t2));
		System.out.println(t1.isSameArea(t3));

	}

}
```
## 출력
```
true
false
```