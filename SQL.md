create_table.sql

```
CREATE TABLE classmates (
	id INT PRIMARY KEY,
    name TEXT,
    age INT,
    address TEXT
);
```

```
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY AUTOINCREMENT ,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);
# NOT NULL = 필수조건
```

-> .schema classmates 를 통해 확인 가능함.



insert.sql

```
INSERT INTO classmates (name,age)
VALUES ('홍길동',23);
INSERT INTO classmates (name,age)
VALUES ('리재찬',5);

```

```
INSERT INTO classmates (name,age,address)
VALUES ('홍길동',23,'서울');
INSERT INTO classmates
VALUES (2,'홍길동',30,'서울');
```



실행시 

1. `.read insert.sql`
2. `.headers on`
3. `.mode column`
4. `SELECT * FROM table;`



```
INSERT INTO classmates
VALUES (2,'홍길동',30,'서울');
# 모든 데이터를 넣을 경우 column이 필요없다.
```







