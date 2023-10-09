## **SQL Injection**
악의적인 사용자가 응용 프로그램 보안 상의 허점을 의도적으로 이용해, 악의적인 SQL문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위이다.

주로 사용자가 입력한 데이터를 제대로 필터링, 이스케이핑하지 못했을 경우에 발생한다. 이러한 Injection 계열의 취약점들은 테스트를 통해 발견하기는 힘들지만 스캐닝툴이나 코드 검증절차를 거치면 보통 쉽게 발견되기 때문에 탐지하기는 쉬운 편이다.

인젝션 공격은 OWASP에서도 1순위로 분류되었던 만큼 공격이 성공할 경우 큰 피해를 입힐 수 있어 보안에 각별한 주의가 필요하다.

## **공격 종류 및 방법**

### **1) Error based SQL Injection**
가장 대중적인 기법으로 SQL 쿼리에 고의적으로 오류를 발생시키고 이때 출력되는 에러의 내용으로 필요한 정보를 찾아내는 공격 기법이다.

기본적으로 '(싱글 쿼테이션) 또는 "(더블 쿼테이션)을 이용하면 GET, POST 요청 필드나 HTTP 헤더 값, 쿠키 값 등에 삽입하여 SQL 에러를 통해 정보를 예상하게 된다. 

#### **- 로그인 공격 예시**
로그인 페이지가 있고, 로그인을 할 때 USER_ID와 INPUT_PW를 입력받아 로그인이 진행된다고 했을 때 

+ 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```

+ 공격 예시 : 로그인 창의 ID 부분에 'OR 1 = 1--를 입력한다.
```sql
SELECT user FROM Users WHERE uid = '' OR 1 = 1 --USRE_ID' AND upw = 'INPUT_PW';
```

+ WHERE절에 있는 싱글 쿼터를 닫아주게 되고,
+ OR 1 = 1로 모두 참을 만들어 준 후
+ --를 이용해 그 뒤의 모든 쿼리문을 주석처리해주게 된다.

결과적으로 Users 테이블에 있는 모든 정보를 조회하게 되며 가장 먼저 만들어진 계정(보통 관리자 계정)으로 로그인할 수 있게 되어 관리자 계정을 탈취하게 된다.

### **2) Union base SQL Injection**
UNION 키워드를 사용하여 원래의 요청에 추가 정보를 얻는 공격 기법으로 UNION 하려는 두 테이블의 컬럼 수와 데이터 형식이 같아야 한다. ORDER BY 절이나 HAVING을 이용한 오류 메시지를 통해 컬럼의 수를 유추할 수 있다.

#### **- 게시글 조회 공격 예시**
게시판이 있고, 게시글을 검색할 때 INPUT을 받아 검색이 진행된다고 했을 때

+ 기본 쿼리문
```sql
SELECT * FROM Board WHERE title LIKE '%INPUT%' OR contents '%INPUT%'
```

+ 공격 예시 : 검색 창에 'UNION SELECT null, id, password FROM Users --를 입력한다.
```sql
SELECT * FROM Board WHERE title LIKE '%' UNION SELECT null, id, passwd FROM Users --%' OR contents '%INPUT%'
```

사전 공격을 통해 컬럼명과 테이블명을 얻은 후 사용자의 ID와 PW를 요청하는 쿼리문을 함께 입력하게 되면 사용자의 개인 정보가 게시글과 함께 보이게 된다.

### **3) Blind SQL Injection**
에러가 발생되지 않는 사이트에서 데이터 베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용하는 공격 기법이다. limit, SUBSTR, ASCII를 사용해서 조건이 참이면 페이지가 정상적으로 출력되고 그렇지 않을 경우 출력되지 않음으로 구분이 가능하다.

최근에는 에러 메시지를 출력하지 않는 웹 서버를 구축하고 있어 주로 사용하는 방식이다.

#### **- Boolean 기반 공격 예시**
+ 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```

+ 공격 예시 : 로그인 폼에 DB 테이블 명을 알아내기 위한 쿼리문을 주입, 이때 임의로 가입한 idd3이라는 아이디와 함께 구문을 주입한다.
```sql
SELECT * FROM Users WHERE uid = 'idd3' and ASCII(SUBSTR((SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0,1),1,1)) > 100 -- USRE_ID' AND upw = 'INPUT_PW';
```
+ limit키워드를 통해 하나의 테이블만 조회하고, SUBSTR함수로 첫 글자만 찾게 된다.
+ ASCII를 통해 값이 변환되고 조회되는 테이블 명의 첫 번째 글자가 U면 테이블이 조회된다.
+ 참(로그인)이 될 때까지 100 숫자를 변경하며 비교를 하게 된다.
+ 자동화 스크립트로 만들어 단기간 내에 테이블 명을 알아낼 수도 있다.

#### **- Time 기반 공격 예시**
+ 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```

+ 공격 예시
```sql
SELECT user FROM Users WHERE uid = 'idd3' OR (LENGTH(DATABASE())=1 AND SLEEP(2)) -- USRE_ID' AND upw = 'INPUT_PW';
```
+ 숫자 1을 조작해 현재 사용하고 있는 데이터 베이스의 길이를 알아낼 수 있다.
+ LENGTH를 사용해 문자열 길이를 반환하도록 한다.
+ DATABASE()를 사용해 데이터베이스의 이름을 반환한다.

SLEEP단어가 치환 처리되었을 경우, BENCHMARK나 WAIT함수를 사용할 수 있다.

### **4) Stored Procedure based SQL Injection**
저장 프로시저(Stored Procedure)는 쿼리들이 모아 하나의 함수처럼 사용하기 위한 것이다.

웹에서 저장 프로시저에 대한 접근 권한을 가짐으로써 실행이 가능해진다. 공격 난이도가 높으나 성공 시 직접적인 피해를 입힐 수 있는 공격 기법이다.

### **5) Mass SQL Injection**
한 번의 공격으로 다량의 DB를 조작해 큰 피해를 입히는 공격 기법이다.

## **대응 방안**

### **1) 입력값 검증**
사용자의 입력을 받을 때 검증 로직을 추가하여 값이 유효한지 검증한다.
+ ', ", #, --, = 등 특수문자와 명령어 필터링
+ 데이터 길이 제한

### **2) 저장 프로시저 사용**
동적 SQL 쿼리를 생성하지 않는 것이 SQL 인젝션을 막기 위한 가장 효과적인 방법일 것이다. 저장 프로시저를 사용하여 지정된 형식의 데이터가 아니면 쿼리가 실행되지 않도록 한다.

### **3) 서버 보안**
+ 데이터 베이스 권한을 제한한다.
+ 신뢰 가능한 네트워크와 서버에 대해서만 접근을 허용한다.
+ SQL 서버 오류 발생 시, 해당하는 에러 메시지를 볼 수 없도록 한다.