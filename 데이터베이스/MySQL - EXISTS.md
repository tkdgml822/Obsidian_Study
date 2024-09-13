![title|center|400](./images/mysql_logo.png)
# CREATE TABLE

이글은 EXISTS와 IF EXISTS에 대한 들어가기 앞서`student_details` 테이블을 만듭시다.
```sql
CREATE TABLE student_details(
  stu_id int,
  stu_firstName varchar(255) DEFAULT NULL,
  stu_lastName varchar(255) DEFAULT NULL,
  primary key(stu_id)
);

INSERT INTO student_details(stu_id,stu_firstName,stu_lastName) 
 VALUES(1,"Preet","Sanghavi"),
 (2,"Rich","John"),
 (3,"Veron","Brow"),
 (4,"Geo","Jos"),
 (5,"Hash","Shah"),
 (6,"Sachin","Parker"),
 (7,"David","Miller");
```
위의 퀴리는 학생의 이름과 성이 포함된 행과 함께 테이블을 생성합니다. 다음은 만들어진 테이블이 잘만들어 확인합시다.
```sql
SELECT * FROM student_details;
```
다음과 같이 출력이 되는 모습을 볼 수 있습니다.
```text
stu_id	stu_firstName	stu_lastName
1	      Preet	        Sanghavi
2	      Rich	        John
3	      Veron	        Brow
4	      Geo	        Jos
5	      Hash	        Shah
6	      Sachin	    Parker
7	      David	        Miller
```

# MySQL에서 **EXISTS** 사용법
EXISTS는 일반적으로 `DELETE`, `SELETCT`, `INSERT` 또는 `UPDATE`를 사용하는데 사용합니다.
```sql
SELECT column_name  
FROM table_name  
WHERE EXISTS (  
    SELECT column_name   
    FROM table_name   
    WHERE condition  
); 
```
여기서 `condition`은 특정 열에서 행을 선택할때 필터링 역할을 합니다. 다음을 보십다.
만약에 `stu_id`=4 `stu_firstName`열에 학생이 있는지 확인하고 싶으면 다음과 같은 코드를 사용할 수 있습니다.
```sql
SELECT EXISTS(SELECT * from student_details WHERE stu_id=4) as RESULT;
```
그러면 다음과 같은 값이 나옵니다.
```text
+--------+ 
| RESULT | 
+--------+ 
|      1 | 
+--------+
```

왜 이런 값이 나올까요? `EXISTS` 명령문은 서브쿼리를 사용하는 명령문입니다. 반환하는 결과값이 있는지 조사합니다. 만약 값이 있다면 참(True), 없다면 거짓(False)을 반환합니다.
그리고 일반적으로 SELECT절까지 가지 않기에 **IN에 비해 속도나 성능면에서 더 좋습니다.**

반대로 조건에 맞지 않은 ROW만 추출하고 싶으면 **NOT EXISTS**을 하면 됩니다.

퀴리 순서는 다음과 같습니다. 메인 쿼리 -> EXISTS 퀴리

# MySQL에서 **IF EXISTS** 사용법
MySQL 명령문 중 조건문인 `IF`문을 사용하면 해당 조건의 기반으로 출력을 변경할 수 있습니다.
```sql
SELECT IF( EXISTS(
             SELECT column_name
             FROM table_name
             WHERE condition), 1, 0)
```

여기에서 만약 `EXISTS`문의 값이 있다면 `True`가 됩니다. 그리고 `IF`문이 실행이 됩니다. 값이 있다고 해봅시다. 그러면 `1`이 출력이 됩니다. 그렇지 않으면 0이 출력이 됩니다.

다음은 테이블에 `stu_id` = 4가 있는 학생이 있는 경우에 `Yes, exists`를 반환하는 퀴리를 작성 하겠습니다. 없는 경우에는 `No, does not exist`을 반환을 하겠습니다. 

```sql
SELECT IF( EXISTS(
             SELECT stu_firstName
             FROM student_details
             WHERE stu_id = 4), 'Yes, exists', 'No, does not exist') as RESULT;
```
그럼 다음과 같은 값이 반환 됩니다. 

```text
+-------------+ 
| RESULT      | 
+-------------+ 
| Yes, exists |
+-------------+
```

그럼 다음은 `stu_id` = 11가 있는 학생을 찾아보겠습니다.
```sql
SELECT IF ( EXUSTS(
			SELECT stu)firstName
			FROM student_details
			WHERE stu_id = 11), 'Yes, exists', 'No, doed not exist') as RESULT
```

그럼 다음과 같이 출력이 됩니다.
```text
+--------------------+ 
| RESULT             | 
+--------------------+ 
| No, does not exist |
+--------------------+
```

# **IF EXISTS CASCADE**
전에 블로그 글을 보다가 `IF EXISTS CASCADE`라는 것도 봤다. 주로 테이블을 삭제할때 같이 사용했다.
다음 명령어를 보자.
```sql
DROP TABLE member IF EXISTS CASCADE;
```
이 명령어가 같는 의미를 보자.

- `DROP TABLE member`: member라는 이름의 테이블을 삭제
- `IF EXISTS` : member 테이블이 존재할 경우에만 삭제합니다. 테이블이 없으면 오류를 발생시키지 않습니다.
- `CASCADE`: 만약 member이라는 테이블이 존재할 경우는 그 테이블과 관련된 모든 객체를 삭제하라는 의미 입니다. 왜래 키 제약 조건 등도 같이 삭제됩니다. 
