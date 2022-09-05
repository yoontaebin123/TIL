# 문제
+ 골프채를 모델링한 GolfClub 클래스를 작성하고, 다음 프로그램으로 테스트하라
```java
public class GolfClubTest {
    public static void main(String[] args) {
        GolfClub g1 = new GolfClub();
        g1.print();

        GolfClub g2 = new GolfClub();
        g2.print();

        GolfClub g3 = new GolfClub("퍼터");
        g3.print();
    }
}
```
# 답
```java
package chap04_challenge;

class GolfClub {
	int num = 7;
	String str;

	GolfClub() {
	}

	GolfClub(int number) {
		this.num = number;
	}

	GolfClub(String string) {
		this.str = string;
	}

	public void print() {
		if (str != null) {
			System.out.println(str + "입니다.");
		} 
		else {
			System.out.println(num + "번 아이언입니다");
		}
	}
}

public class GolfClubTest {

	public static void main(String[] args) {
		GolfClub g1 = new GolfClub();
		g1.print();

		GolfClub g2 = new GolfClub(8);
		g2.print();

		GolfClub g3 = new GolfClub("퍼터");
		g3.print();
	}

}
```
## 출력
```
7번 아이언입니다.
8번 아이언입니다.
퍼터입니다.
```