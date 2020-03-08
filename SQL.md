# SQL

## 1. SQL_SELECT

```sql
SELECT * FROM table_name;

/* '*' means all select */
```

```sql
SELECT DISTINCT blah1 FROM table_name;

/* DISTINCT means select only the DISTINCT values from the "blah1" column in the "blah2" table / 중복을 제거하고 보여준다 */
```

```sql
SELECT COUNT (DISTINCT blah1) FROM table_name;

/* SELECT COUNT는 기본적으로 카운트를 세주고, (DISTINCT blah1)을 넣음으로써, blah1에 대한 중복을 제거하고 개수가 몇 개가 되는 지 알려준다 */

/* MS용 */

SELECT Count (*) AS Distinctblah1 FROM (SELECT DISTINCT blah1 FROM table_name)
```

#### 2. SQL_WHERE

```sql
SELECT blah1 FROM table_name WHERE condition

ex)
SELECT * FROM Customers WHERE Country='Mexico';
/* country가 Mexico로 지정되어 있는 모든(*) customers를 보여준다 */

SELECT * FROM Customers WHERE CustomerID=1;
/* customerID가 1인 customer를 보여준다 */
```

**Operators in The WHERE Clause**

=, >, <, >=, <=, <>, BETWEEN, LIKE, IN

- <> means NOT EQAUL ( '!=' 버전에 따라 이렇게도 표기 가능)

- LIKE means serch for a pattern

  ```sql
  SELECT * FROM Customers WHERE City LIKE 'a%';

  /* a로 시작하는 city만 선택되어져서 나옴 */
  /* '%e' 라고 쓰면 마지막 글자가 e로 끝나는 city만 선택되어져서 나옴 */
  /* 'm%e'도 가능 - m으로 시작하고 e로 끝남 */
  ```

- IN means to specify multiple values

  ```sql
  SELECT * FROM Customers WHERE City IN ('Paris', 'London');
  ```

#### 3. SQL_AND, OR AND NOT

1. AND - 하위 카테고리를 같이 설정하고 싶을 때

```sql
SELECT * FROM Customers WHERE Country='Germany' AND City='Berlin';
```

2. OR - 같은 카테고리 내에서 '또는' 연산자

```sql
SELECT * FROM Customers WHERE City='Berlin' OR City='Munchen';
```

3. NOT - 제외하고 싶은 카테고리

```sql
SELECT * FROM Customers WHERE NOT Country='Germany';
```

4. Combining AND, OR and NOT

```SQL
SELECT * FROM Customers WHERE Country='Germany' AND (City='Berlin' OR City='Munchen');
```

```sql
SELECT * FROM Customers WHERE NOT Country='Germany' AND NOT Country='USA';
```

#### 4. SQL_Order By

```SQL
SELECT * FROM Customers ORDER BY Country;

/* 기본적으로 abc순으로 나열해준다(Country를 기준으로) */
```

```sql
SELECT * FROM Customers ORDER BY Country DESC;

/* DESC means z-a 순으로 나열 */
```

```sql
SELECT * FROM Customers ORDER BY Country, CustomerName;

/* Country를 먼저 a-z순으로 나열, 그리고 그 안에서 Name을 나열해서 보여줌 */
```

```sql
SELECT * FROM Customers ORDER BY Country ASC, CustomerName DESC;

/* ASC means a-z, Country는 a-z 순으로 나열, 그 안의 이름을 z-a순으로 나열 */
```

#### 5. SQL_Insert Into

```sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country) VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');

/* INSERT INTO로 무슨 카테고리를 추가할 것인 지 카테고리명을 입력 후, VALUES로 적은 카테고리명 순서대로 값을 입력한다 */
```

```sql
INSERT INTO Customers (CustomerName, City, Country) VALUES ('Cardinal', 'Stavanger', 'Norway');

/* 특정한 카테고리에만 값을 넣어줄 수도 있으며, 지정해주지 않은 카테고리에는 'null'로 표기됨 */
```

#### 6. SQL_Null Values

```sql
SELECT CustomerName, ContactName, Address FROM Customers WHERE Address IS NULL;

/* address가 null인 것만 보여줌 */
```

```sql
SELECT CustomerName, ContactName, Address FROM Customers WHERE Address IS NOT NULL;

/* Address가 null이 아닌 것을 보여줌 */
```

#### 7. SQL_Wildcard Characters

```sql
SELECT * FROM Customers WHERE City LIKE 'ber%';
/* ber로 시작하는 모든 패턴을 찾음 */

SELECT * FROM Customers WHERE City LIKE '%es%';
/* 가운데에 'es'가 들어가는 모든 패턴을 찾음 */

SELECT * FROM Customers WHERE City LIKE '_ondon';
/* 뒤에 'ondon'이 따라오는 어떤 첫 시작 알파벳 한 글자라도 ㅇㅋ */

SELECT * FROM Customers WHERE City LIKE 'L_n_on';

SELECT * FROM Customers WHERE City LIKE '[bsp]%';
/* b, s, p가 첫시작 하는 모든 패턴을 찾음 */

SELECT * FROM Customers WHERE City LIKE '[a-c]%';
/* a부터 c까지 시작하는 모든 패턴을 찾음 */

SELECT * FROM Customers WHERE City LIKE '[!bsp]%';
SELECT * FROM Customers WHERE City NOT LIKE '[bsp]%';
/* b, s, p로 시작하지 않는 모든 패턴을 찾음 */
```

#### 8. SQL_Alias

```sql
SELECT CustomerID AS ID, CustomerName AS Customer FROM Customers;

SELECT CustomerName AS Customer, ContactName AS [Contact Person] FROM Customers;

/* 기존의 카테고리명 CustomerID 대신에 ID라는 이름으로 바꿔서 불러오기 */
/* 띄어쓰기는 [] 안에 넣기 */
```

```sql 
SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address FROM Customers;

SELECT CustomerName, CONCAT(Address, ', ', PostalCode,', ',City,', ',Country) AS Address FROM Customers

/* 한 카테고리에 여러 카테고리를 더할 수 있으며 AS로 한 이름으로 다시 재할당 할 수 있음 */
```

```sql
SELECT o.OrderId, o.OrderDate, c.CustomerName FROM Customers AS c, Orders AS o WHERE c.CustomerName='Around the Horn' AND c.CustomerID=o.CustomerID;

SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName FROM Customers, Orders WHERE Customers.CustomerName='Around the Horn' AND Customers.CustomerID=Orders.CustomerID;
```

#### 9. SQL_Joins

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;

/* orders에서 customerID랑 customers에서 customerID랑 똑같은 값을 가져와서 orderID, CustomerName, OrderDate를 orders 테이블에 나타낸다 */
```

INNER JOIN (교집합) / LEFT JOIN (왼쪽만, 왼쪽에 포함되는 오른쪽 자료) / RIGHT JOIN (오른쪽만, 오른쪽에 포함되는 왼쪽 자료) / FULL OUTER JOIN (총집합)
