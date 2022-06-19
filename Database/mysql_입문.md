# Mysql 입문 & 저장 프로시저

- Mysql 로컬 설치
  - Https://dev.mysql.com/downloads/mysql/

- mysql 접속
  - `$ mysql -u root -p`

- DB 보기
  - `$ SHOW DATABASES;`
  - ![스크린샷 2022-06-19 오후 8.44.06](mysql_입문.assets/스크린샷 2022-06-19 오후 8.44.06.png)

- 실습용 테이블 신규 생성

  - 테이블명: sassy_user
  - 컬럼: id, name, campus, class, gi

- 신규 데이터 베이스 생성

  - `$ CREATE database ssafy;`

- DB 선택

  - `$ USE ssafy;`

- 테이블 생성

  - ```mysql
    CREATE TABLE ssafy_user (
    id INT PRIMARY KEY AUTO_INCREMENT);
    ```

- 컬럼 추가

  - ```mysql
    ALTER TABLE ssafy_user ADD name VARCHAR(50) NOT NULL;
    ALTER TABLE ssafy_user ADD campus VARCHAR(50) NOT NULL;
    ALTER TABLE ssafy_user ADD class INT NOT NULL;
    ALTER TABLE ssafy_user ADD gi INT NOT NULL;
    ```

  - 한 번에

  - ```mysql
    mysql> CREATE TABLE ssafy_user (
        -> id INT PRIMARY KEY AUTO_INCREMENT,
        -> name VARCHAR(20) NOT NULL,
        -> campus VARCHAR(20) NOT NULL,
        -> class INT NOT NULL,
        -> gi INT NOT NULL);
    ```

- 데이터 삽입 해보기

  - `mysql> INSERT INTO ssafy_user VALUES(null, '윤형준', '광주', 2, 7);`

- 테이블 확인

  - `$ select * from ssafy;`

![스크린샷 2022-06-19 오후 9.57.32](mysql_입문.assets/스크린샷 2022-06-19 오후 9.57.32.png)

- :white_check_mark: 구분 문자(;) 변경하기
  - 명령문이 완성되지 않은 상태에서 실행 되면 매우 곤란!!
  - 저장 프로시저에서 END를 입력하고 나서 CREATE PROCEDURE 명령이 실행되도록 환경을 변경해야 함
  - 구분 문자를 쌍반점이 아닌 다른 문자로. 일반적으로 `//` 사용
  - 구분 문자를 //로 변경할 때에는 **DELIMITER** 명령을 사용
  - `DELIMETER //`
  - 이렇게 되면 END 뒤에 `//`로 종료하면 됨
  - 저장 프로시저 만든 이후에는 다시 쌍반점으로 돌려놓기

​	![스크린샷 2022-06-19 오후 9.30.15](mysql_입문.assets/스크린샷 2022-06-19 오후 9.30.15.png)

- ### 요구사항의 기능을 만족하는 저장 프로시저 작성

  - [공식문서](https://dev.mysql.com/doc/refman/8.0/en/create-procedure.html)

- ```mysql
  mysql> CREATE PROCEDURE `proc_user_insert` (
      -> name VARCHAR(20)
      -> , campus VARCHAR(20)
      -> , class INT
      -> , gi INT)
      -> BEGIN
      -> DECLARE id INT;
      -> SELECT COUNT(*) + 1
      ->   INTO id
      ->   FROM ssafy_user;
      -> INSERT INTO ssafy_user(id, name, campus, class, gi) VALUES(id, name, campus, class, gi);
      -> END//
  Query OK, 0 rows affected (0.00 sec)
  
  ```

- 이후 delimiter ;를 입력해서 다시 바꿔주고

- ![스크린샷 2022-06-19 오후 10.03.09](mysql_입문.assets/스크린샷 2022-06-19 오후 10.03.09.png)

- 위와 같이 데이터가 추가 됨을 알 수 있다.

- 