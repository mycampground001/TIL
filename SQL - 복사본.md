명령어

sqlite 들어가기 sqlite3 `네임`.sqlite3



* .schema `table`

* 주석은 --

* .read `create_table.sql` .read `insert.sql`  -> 파일 읽기
* .tables

* DROP TABLE `table`   -> table 삭제

* SELECT * FROM `table`;   -> table 불러오기 (*대신 id,name 이면 둘의 column만 출력)

* .mode column  |  .headers on  -> 출력 보기좋게

* SELECT id,name FROM `table` LIMIT 3;  -> id,name을 3행까지 출력
* SELECT id,name FROM `table` LIMIT 1 OFFSET 2; -> 3행만 출력한다는 의미. 시작점 지정

* SELECT * FROM classmates WHERE `age` = 23;  -> 특정 값을 가진레코드를 찾음

* DELETE FROM `table` WHARE `id` = 3; -> 특정 값을 가진 레코드를 지움

* UPDATE `table` SET `age`=30, `address`="대전" WHERE `id`=104; -> id=104인 것의 값을 바꿈



테이블 추가

* .mode csv
* .import `users.csv` `table`



count, MAX, MIN ... 사용해보기

* SELECT count(`age`) FROM `table` WHERE `age`>=30 AND `last_name`="김"; -> 갯수를 보여줌

* SELECT `first_name`, MAX(`balance`) FROM users; -> 잔액이 max인 사람의 이름과 잔액

* SELECT `first_name` FROM users WHERE `balance` = (SELECT MAX(balance) FROM users); ->잔액이 최대인 사람의 이름만 보여줌

* SELECT * FROM users WHERE `age` LIKE '2%'; -> 나이가 20대인 목록 %는 어떤값도 가능

* SELECT * FROM users WHERE `phone` LIKE '%-5114-%'; ->  가운데가 -5114-

```
2% 2로 시작하는 값
%2 2로 끝나는 값
%2% 2가 들어가는 값
"-" 는 필수로 들어가야된다는 의미
_2% 아무값이나 들어가고 두번째가 2로 시작하는 값
1___ 1로시작하고 4자리인 값
2_%_% 2로시작하고 적어도 3자리인 값
```

* SELECT * FROM users ORDER BY `age` ASC,DESC; -> 나이 오름차순 또는 내림차순, ASC가 디폴트
* SELECT * FROM users ORDER BY age, last_name; -> 나이오름차순, 같은 나이중 이름은 오름차순.