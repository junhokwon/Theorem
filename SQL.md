#SQL이란?

: SQL은 데이터베이스의 데이터를 저장, 조작 및 검색하기위한 표준 언어입니다.

: mysql, sql server, ms access, oracle, sybase, informix, postgres 및 기타 데이터베이스 시스템에서 sql을 사용하는 법

: sql은 구조화된 쿼리 언어(데이터 베이스를 통해 특정한 정보를 추출하고 보여주는 데이터베이스언어)

: sql을 사용하면 데이터베이스에 액세스하고 조작 할 수 있습니다.

: sql은 ANSI 표준입니다.

* SQL의 기능

(1) 데이터베이스에 대해 쿼리를 실행

(2) 데이터베이스에서 데이터를 검색 

(3) 데이터베이스에 레코드를 삽입

(4) 데이터베이스의 레코드를 업데이트 가능

(5) 데이터베이스에서 레코드를 삭제

(6) 새로운 데이터베이스를 생성

(7) 데이터베이스에 새로운 테이블 생성

(8) 데이터베이스에 저장프로시저 생성

(9) 데이터베이스에 뷰를 생성

(10) 테이블,프로시저 및 뷰에 대한 사용권한 설정

* 웹사이트에서 SQL 사용

: 데이터베이스의 데이터를 보여주는 웹사이트를 구축하려면 다음이 필요!

(1) RDBMS 데이터베이스  프로그램(예 : ms access, SQL server, Mysql등)

(2) PHP또는 ASP와 같은 서버측 스크립팅 언어 사용하려면

(3) SQL을 사용하여 원하는 데이터를 얻으려면

(4) html/css를 사용하여 페이지의 스타일을 얻으려면

* RDBMS

: RDBMS는 관계형 데이터베이스 관리 시스템의 약자

: RDBMS는 SQL과 MS SQL Server, IBM DB2, Oracle, MySQL 및 Microsoft Access와 같은 모든 최신 데이터베이스 시스템의 기초

: RDBMS의 데이터는 테이블이라는 데이터베이스에 저장됩니다. **테이블은 관련 데이터 항목의 모음이며 열과 행으로 구성**

```
"고객"테이블을 보십시오 :

SELECT * FROM Customers;
```

: 모든 테이블은 > 필드라는 더 작은 엔티티로 나뉜다. Customers 테이블의 필드는 CustomersID,CustomersName,ContactName,Address,City 및 PostalCode로 구성돈다. **필드는 테이블의 모든 레코드에 대한 특정 정보를 유지 관리하도록 설계된 테이블의 열이다**

: **레코드는 테이블에 있는 개별 항목이다. Customers 테이블에는 91개의 레코드가 있다.**
**레코드는 테이블의 가로 엔터티**
**컬럼은 테이블의 특정 필드와 연관된 정보를 포함하는 테이블의 수직 엔터티**

: 테이블 (열과행) > 필드 (가로엔터티(레코드),수직엔터티(컬럼))

#데이터베이스 테이블

: 데이터베이스는 대게 하나 이상의 테이블 포함

CustomerID|CustomerName|	ContactName|Address	|City	PostalCode|Country
---|---|---|---|---|---|
Alfreds Futterkiste|Maria Anders|	Obere Str.|57|Berlin|12209|	Germany

: 1개의 레코드(가로엔터티) 1개의필드(제목열)
* sql키워드는 대소문자 구분 안함, select는 SELECT와 같다. 대문자로 작성하자

* sql문 다음에 오는 세미콜론?

: 세미콜론은 데이터베이스 시스템에서 각 sql 문을 분리하여 서버에 대한 동일한 호출에서 둘 이상의 sql문을 실행할 수 있게 해주는 표준 방법이다.

# 중요한 SQL

* select : 데이터베이스에서 데이터를 추출
* update : 데이터 업데이트
* delete : 데이터 삭제
* insert into : 새로운 데이터 삽입
* create database : 새 데이터베이스 만듬
* alter datebase : 데이터베이스 수정
* create table : 새 테이블 만듬
* alter table : 테이블 수정
* drop table : 테이블 삭제
* create index :  검색(키) 생성
* drop index : 색인(검색키) 삭제

#SQL SELECT 문 

: select문은 데이터베이스에서 데이터를 선택하는데 사용, 리턴 된 데이터는 `결과 세트`라고 하는 결과 테이블에 저장

```
SELECT colunm1, colunm2, ...
FROM table_name;

SELECT * FROM table_name;
```

: colunm1,colunm2는 데이터를 선택할 **테이블 필드의 이름**

```
SELECT CustomersName ,City FROM Customers;

SELECT * FROM Customers;
```
#SQL SELECT DISTINCT문

: select distinct.은 고유한(다른)값만 리턴하는데 사용된다. 테이블 내에서 열은 종종 많은 중복값을 포함하기에 뚜렷한 값만 나열하길 원할 경우

```
SELECT DISTINCT colunm1,column2
FROM table_name;

SELECT Country FROM Customers;

: Customers 테이블의 'country'열에 있는 모든 중복된 값을 선택

SELECT DISTINCT Country FROM Customers;

: customers의 테이블의 'country'열에서 distinct값만 선택합니다.

SELECT COUNT(DISTINCT Country) FROM Customers;

: 서로 다른 별개의 고객 국가의 수를 나열
```

#SQL WHERE 절
: where절은 레코드(가로 엔터티)를 필터링 하는데 사용된다.

```
SELECT column1, column2, ...
FROM table_name
WHERE condition;

SELECT * FROM Customers
WHERE Country='Mexico';

: customers테이블에서 'mexico'국가의 모든 고객을 선택한다.

SELECT * FROM Customers
WHERE CustomerID=1;
```

* WHERE절에 있는 연산자

Operator	|Description
---|---
=	|Equal
<>	|Not equal. !=
>	|Greater than
<	|Less than
>=	|Greater than or equal
<=	|Less than or equal
BETWEEN	|Between an inclusive range
LIKE	|Search for a pattern
IN	| To specify multiple possible values for a column

#SQL AND,OR 및 NOT 연산자

: where절은 and or,not연산자와 결합 할 수 있습니다. 

: and 및 or연산자는 둘 이상의 조건에 따라 레코드를 필터링 하는데 사용됩니다.

: and로 구분된 모든 조건이 true이면 and연산자가 레코드를 표시

: or로 구분된 조건이 true인 경우 or연산자는 레코드를 표시

: not 연산자는 조건이 참이 아닌경우 레코드를 표시

```
SELECT column1,column2,...
FROM table_name
WHERE Condition1 AND condition2 AND condition3 ...;

SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;

SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;

SELECT * FROM Customers
WHERE City='Berlin' OR City='München';

: sql문은 도시가 베를리니 또는 무첸인 모든 필드를 선택

SELECT * FROM Customers
WHERE Country='Germany' AND (City='Berlin' OR City='München');

: sql문은 country가 독일이고 도시가 베를린또는 무첸여야만 하는 customer의 모든 필드를 선택

SELECT * FROM Customers
WHERE NOT Country='Germany' AND NOT Country='USA';

: 도시가 독일도 아니고 미국도 아닌 모든 필드를 선택

```

#sql order by 키워드

: 결과 집합을 오름차순 또는 내림차순으로 정렬되는데 사용됩니다.

: order by 키워드는 레코드를 기본적으로 오름차순으로 정렬합니다.내림차순으로 레코드를 정리하면 `DESC`키워드를 사용

```
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;

SELECT * FROM Customers
ORDER BY Country;

: sql문은 고객 테이블의 모든 고객을 국가열로 정렬하여 선택합니다.

SELECT * FROM Customers
ORDER BY Country DESC;

: 고객테이블의 모든 국가열로 내림차순으로 정렬

SELECT * FROM Customers
ORDER BY Country, CustomerName;

: 고객테이블의 모든 고객을 도시 및 고객이름 열로 정렬하여 선택
```

#sql insert into문

: 테이블에 새 레코드를 삽입하는데 사용

```
INSERT INTO table_name (column1, column2, column3,...)
VALUES (value1,value2,valu3,...);

INSERT INTO table_name
VALUES (value1,value2,value3,...);

INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');

* 지정된 열에만 데이터 삽입

INSERT INTO Customers(CustomersName,City,Country)
VALUES('Cardinal','Stavanger,'Norway');

```

#sql null값
: null 값이 이쓴 필드는 값이 없는 필드

* null 값을 테스트하는 방법

`IS NULL`연산자와 `IS NOT NULL`연산자 사용

```
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
```
```
SELECT column_names
FROM table_name
WHERE column_name IS NOR NULL;
```

#SQL UPDATE 문

: 기존 레코드를 수정하는데 사용

```
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

#SQL DELETE 문
: 기존 레코드를 삭제하는데 사용

```
DELETE FROM table_name
WHERE condition;
```
* 테이블을 삭제하지 않고, 테이블의 모든 행을 삭제할 수 있습니다. 이는 테이블구조, 속성 및 인덱스가 손상되지 않는것을 의미

```
DELETE FROM table_name;
DELETE * FROM table_name;
```

#sql select top 절

: 리턴할 레코드 수를 지정하는데 사용

```
* sql server/ms엑세스 구문

SELECT TOP number/percent column_name(s)
FROM table_name
WHRER condition;

* mysql구문

SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;

* 오라클 구문

SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;


SELECT TOP 3*FROM customers;

: 고객 테이블에서 세개의 레코드 선택

SELECT * FROM customers
LIMIT 3;

SELECT * FROM customers
WHERE ROWRUN <=3

SELECT TOP 50 PERCENT * FROM customers
: 고객테이블에서 처음 50%의 레코드를 선택합니다.

SELECT TOP 3 * FROM Customers
WHERE Country='Germany';

SELECT * FROM Customers
WHERE Country='Germany'
LIMIT 3;

SELECT * FROM Customers
WHERE country='germany' AND ROWNUM <= 3;

: 국가가 독일인 고객 테이블에서 처음 세개의 테이블 선택

```
#SQL MIN() 및 max()함수

: MIN()함수는 선택된 컬럼의 가장 작은 값을 리턴합니다.

: MAX()함수는 선택된 열의 가장 큰값을 반환

 ```
 SELECT MIN(column_name)
 FROM table_name
 WHERE condition;
 
 SELECT MAX(coumn_name)
 FROM table_name
 WHERE condition;
 ```
 
 #SQL COUNT() ,AVG() 및 SUM() 함수
 
 : count()함수는 지정된 조건과 일치하는 **행**수를 반환
 
 : avg()함수는 숫자 **열**의 평균값을 반환
 
 : sum()함수는 숫자열의 총 **합계**를 반환
 
 
```
SELECT COUNT(column_name)
 FROM table_name
 WHERE condition;
 
SELECT AVG(column_name)
FROM table_name
WHERE condition;

SELECT AVG(column_name)
FROM table_name
WHERE condition;
```


# SQL LIKE 연산자

: like연산자는 where절에서 열의 지정된 패턴을 검색하는데 사용한다.

* like 연산자와 함께 사용되는 두개의 방법
* % : 백분율 기호는  0,1또는 복수문자
* _ : 밑줄은 한 문자를 나타냄


요소 | 설명
---|---
'a%'|a로 시작하는 문자
'%a'|a로 끝나는 문자
'%or%' | 'or'인 문자
'_r%'| 두번째문자가 r
'a_%_%'| a로 시작하고 , 길이가 3자이상
'a%o'| a에서 시작에서 o로 끝남

```
SELECT * FROM Customers
WHERE CustomerName NOT LIKE 'a%';

: a로 시작하지 않은 모든 고객 선택
```
#sql 와일드 카드

: 와일드 카드 문자는 다른 문자를 대체하는데 사용

: 와일드 카드 문자는 SQL LIKE 연산자와 함께 사용, LIKE연산자는 where절에서 열의 지정된 패턴을 검색하는데 사용

```
SELECT * FROM Customers
WHERE City LIKE 'ber%';

: ber로 시작하는 모든 고객 선택

SELECT * FROM Customers
WHERE City LIKE '%es%';

: 'es'패넡이 포함된 모든 도시가 있는 고객 선택

SELECT * FROM Customers
WHERE City LIKE '_erlin';

: 임의자로 시작하여 erlin으로 끝나는 도시가 있는 고객 선택

SELECT * FROM Customers
WHERE City LIKE 'L_n_on';

: L로 시작하고 모든 문자,'n'다음에 문자가 오고 'on'다음에 오는 모든 고객 선택

SELECT * FROM Customers
WHERE City LIKE '[bsp]%';

: city가 'b','s'또는 'p'로 시작하는 모든 고객을 선택

SELECT * FROM Customers
WHERE City LIKE '[a-c]%';

: city가 'a','b'또는 'c'로 시작하는 모든 고객을 선택

SELECT * FROM Customers
WHERE City LIKE '[!bsp]%';

SELECT * FROM Customers
WHERE City NOT LIKE '[bsp]%';

: 'b','s'또는 'p'로 시작하는 도시가 아닌 모든 고객

```
#sql in 연산자

: in연산자를 사용하면 where절에 여러값을 지정할 수 있다.

```

SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);

SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);

SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');

: 독일,프랑스,영국에 있는 모든 고객을 선택

SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);

: 공급업체와 동일한 국가의 모든 고객을 선택

```


#SQL BETWEEN 연산자

: between 연산자는 주어진 점위 내의 값을 선택, 같은 숫자 ,텍스트 날짜 일 수 있다.

```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;

SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;

: 가격이 10과 20인사이에 가격인 물건을 찾기

SELECT * FROM Products
WHERE Price NOT BETWEEN 10 AND 20;

SELECT * FROM Products
WHERE (Price BETWEEN 10 AND 20)
AND NOT CategoryID IN (1,2,3);

: 가격이 10이상 20 사이인 모든 제품을 선택하며, 카테고리id가 1,2또는 3인 제품을 표시하지 않음

SELECT * FROM Orders
WHERE OrderDate BETWEEN #07/04/1996# AND #07/09/1996#;

: OrderDate BETWEEN '04 -July-1996 'W '09-Junly-1996'이있는 모든 주문을 선택합니다.

```
#sql별칭

: sql 별명은 테이블 또는 테이블 칼럼에 임시 이름을 제공하는데 사용

```
SELECT column_name AS alias_name
FROM table_name;

SELECT CustomerID as ID, CustomerName AS Customer
FROM Customers;

: customerID을 ID,customername을 customer라고 별칭을 만든다.

SELECT CustomerName AS Customer, ContactName AS [Contact Person]
FROM Customers;

: customerName열과 contactName 열의 두가지 별칭을 만든다. 별칭 이름에 공백이 포함되어 있을경우 큰따음표나 대괄호가 필요

```
```

SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM Customers, Orders
WHERE Customers.CustomerName="Around the Horn" AND Customers.CustomerID=Orders.CustomerID;

```
```
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;

: customerID = Around the Horn인 고객의 모든 주를 선택한다. customers 및 orders테이블을 사용하고 각각 'c'및'o'테이블 별칭을 부여

```
* 별칭은 다음과 같은 경우에 유용

(1) 쿼리에 두 개 이상의 테이블이 관련
(2) 함수는 쿼리에서 사용
(3) 열 이름이 크거나 매우 읽을수 없음
(4) 두개 이상의 열이 결합

#SQL 조인

: join절은 두개 이상의 테이블에 있는 행을 결합하는데 사용

![](/Users/mac/projects/images/스크린샷 2017-05-21 오후 9.09.58.png)

: 주문 테이블의 고객 id열은 고객 테이블의 고객 id를 타나낸다. 위 두 테이블 사이의 관계는 'customerID'열입니다.

```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
**INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;**
```

![](/Users/mac/projects/images/스크린샷 2017-05-21 오후 9.14.04.png)

(1)JOIN : 두 테이블값이 일치하는 교집합
(2)LEFT JOIN : 왼쪽 테이블에서 모든 레코드를 반환하고 오른쪽 테이블에서 일치하는 레코드 반환
(3)RIGHT JOIN : 오른쪽 테이블에서 모든 레코드를 반환하고 왼쪽 테이블에서 일치하는 레코드를 반환
(4)FULL JOIN : 합집합

```
SQL LEFT JOIN 예제

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CustomerName;

: customers테이블의 customersName정렬하며 고객 ID과 같은 주문의 교집합과 FROM customers의 영역으로 정렬

```
```

SELECT column_name(s)
FROM table1
RIGHT JOIN table2 ON table1.column_name = table2.column_name;
```
![](https://www.w3schools.com/SQL/img_rightjoin.gif)

```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;

: 모든 합집합을 의미 FULL OUTER JOIN

```

#sql union 연산자 

: union연산자는 두개이상의 select문을 결합하는데 사용

(1) union내의 각 select문은 같은 수의 열을 가져야한다
(2) 열은 유사한 데이터 형식을 가져야 한다
(3) 각 select 문의 열은 또한 동일한 순서로 있어야 한다.

```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
: 고객 및 공급자에서 모두 다른 도시(고유한값(하나의값)을 선택

SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers
ORDER BY City;

: 고객 및 공급자에서 모든 도시를 선택(중복값)

SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;

: 고객 및 공급자에서 모두 다른 독일 도시(고유한값만 선택)
```

#sql group by 구문

: group by 문은 집계함수(count,max,min,sum,avg)와 함께 사용되어 결과 집합을 하나이상의 열로 그룹화한다.

```
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;

: 각 국가의 고객 수를 높은 곳에서 낮은 곳으로 정렬하여 나열합니다.

SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;

: 각 발송인의 id가 일치한 교집합과 orders 테이블의 합집합이며 numberoforders라는 별칭을 만들고 shippername을 하나의 열로 그룹화한다.

```

#sql having 절

: where 키워드를 집계함수와 함께 사용할수 없기에 having절이 sql에 추가

```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID) DESC;

: customerid의 수와 도시를 고객테이블에서 꺼냄, 도시로 그룹화하여 열로 처리, 고객id수가 5이상이며, 내림차순으로 각국가의 고객수가 높은 순으로 정렬

```

#sql exists 연산자

: exists 연산자는 하위 쿼리의 레코드 조재 여부를 테스트 하는데 사용

```
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);

SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE SupplierId = Suppliers.supplierId AND Price < 20);

: 공급자이름을 공급자 테이블에서 컬럼으로 선택하는데 조건이, 제품 가격이 20미만이고 공급자 아이디가 같은 상품의 공급자를 나열

```
#SQL ANY 및 모든 연산자

: any w all 연산자는 where 또는 having절과 함께 사용

```
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY
(SELECT column_name FROM table_name WHERE condition);

SQL ANY 예제 :하나가 조건을 충족하면,

SELECT ProductName
FROM Products
WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);

: 수량이 10인 ordertdetails 테이블의 모든 레코드를 찾으면 제품이름을 나열한다.

SQL ALL 예제 : 모든 조건이 충족하면,

SELECT ProductName
FROM Products
WHERE ProductID = ALL (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);

: 수량이 10인 orderdetails테이블에서 모든 레코드에서 제품 이름을 나열
```

#SQL SELECT INTO 문

: 한테이블의 데이터를 새 테이블로 복사

```
SELECT *
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;

* 일부열만 새 테이블로 복사

SELECT column1, column2, column3, ...
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;

SQL SELECT INTO 예제

SELECT * INTO CustomersBackup2017
FROM Customers;

: 고객테이블 백업 복사본 만들기

SELECT * INTO CustomersBackup2017 IN 'Backup.mdb'
FROM Customers;

: 테이블을 다른 데이터베이스의 새 테이블에 복사

SELECT CustomerName, ContactName INTO CustomersBackup2017
FROM Customers;

: 몇개의 열만 새 테이블에 복사

SELECT * INTO CustomersGermany
FROM Customers
WHERE Country = 'Germany';

: 독일이 국가인 고객들만 새 테이블로 복사

SELECT Customers.CustomerName, Orders.OrderID
INTO CustomersOrderBackup2017
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

: 둘이상의 테이블을 새 테이블로 데이터 복사
고객 테이블 합집합 복사

```
#SQL INSERT INTO SELECT 문

: insert into select는 소스 및 목표테이블의 데이터 유형이 일치해야 한다.

```
INSERT INTO table2
SELECT * FROM table1
WHERE condition;

: 한 테이블(테이블1)의 모든열을 다른 테이블2로 복사

INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers
WHERE Country='Germany';

: 독일어 공급 업체만 공급테이블에 공급자 이름, 도시, 국가를 복사하여 고객테이블로 옮긴다.
```
#sql 주석

: sql문의 섹션을 설명, sql문의 실행을 막는데 사용

(1) --

--Select all:
SELECT * FROM Customers;

(2) /* 내용 */


-







