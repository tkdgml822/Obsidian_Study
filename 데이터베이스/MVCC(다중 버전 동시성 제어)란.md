---
aliases:
  - MVCC(다중 버전 동시성 제어)란?
---
### 동시성 제어
동시성 제어는 DBMS 쓴다고 가정했을 때 다수의 사용자가 동시에 다중 트랜잭션의 상호간섭 작용에서 Database를 보호하는 것을 의미한다.
DBMS는 동시성을 제어할 수 있도록 **Lock 기능**과 **SET TRANSACTION 기능**을 제공한다. 동시성을 제어하는 방법에는 **낙관적 동시성 제어**와 **비관적 동시성 제어**가 있다.

**낙관적 동시성 제어(Optimistic Concurrency Control)**
- 사용자들이 같은 데이터를 동시에 수정하지 않을 것이라고 가정
- 데이터를 읽는 시점에 Lock을 걸지 않는 대신 수정 시점에 값이 변경됐는지를 반드시 검사

**비관적 동시성 제어(Pessimistic Concurrency Control)**
- 사용자들이 같은 데이터를 동시에 수정할 것이라고 가정
- 데이터를 읽는 시점에 Lock을 걸고, 트랜잭션이 완료될 때까지 이를 유지
- SELECT 시점에 Lock을 거는 비관적 동시성 제어는 시스템의 동시성을 심각하게 떨어뜨릴 수 있어서 wait 또는 nowait 옵션과 함께 사용되어야 한다.

#### 공유락(Shared Lock)과 배타락(Exclusive Lock)
비관적 동시성을 제어를 위한 대표적인 방법은 2개다.

- 공유락(Shared Lock): 읽기 잠금
- 배타락(Exclusive Lock): 쓰기 잠금

**동일한 레코드에 대해 각각 공유락과 베타락을 가져간 경우에 동작**
- 1번 트랜잭션이 공유락(읽기 잠금)을 가져간 경우
	- 2번 트랜잭션이 데이터를 읽는 경우는 데이터가 일관되므로, 2번 트랜잭션이 또 다른 공유락을 가져가면서 동시에 처리함
	- 2번 트랜잭션이 데이터를 쓰는 경우는 1번 트랜잭션과 데이터가 달라질 수 있므로 1번 트랜잭션 종료까지 기다려야 함
- 1번 트랜잭션이 배타락(쓰기 잠금)을 가져간 경우
    - 2번 트랜잭션이 데이터를 읽는 경우, 1번 트랜잭션이 데이터를 변경할 수 있으므로 기다림
    - 2번 트랜잭션이 데이터를 쓰는 경우에도, 1번 트랜잭션이 데이터를 변경할 수 있으므로 기다림

락을 푸는 방식은 커밋과 롤백말고 없다. 

**Locking 메커니즘의 문제점**
- 읽기 작업과 쓰기 작업이 서로 방해를 일으키기 때문에 동시성 문제가 발생
- 데이터 일관성에 문제가 생기는 경우도 있어서 Lock을 더 오래 유지하거나 테이블 레벨의 Lock을 사용해야 하고, 동시성 저하가 발생

### MVCC(Multi-Version Concurrency Control, 다중 버전 동시)
MVCC는 동시 접근을 허용하는 데이터베이스에서 <font color="#d99694">동시성을 제어하기 위해 사용하는 방법</font>
원본의 데이터와 변경중인 데이터를 동시에 유지하는 방식, 원본 데이터에 대한 Snapshot을 백업하여 보관한다.
만약 두 가지 버전의 데이터가 존재하는 상황에서 새로운 사용자가 데이터에 접근하면 데이터베이스의 Snapshot을 읽는다. 그러다가<font color="#d99694"> 변경이 취소되면 Snapshot 바탕으로 데이터를 복구하고, 만약 변경이 완료되면 최종적으로 디스크에 반영하는 방식으로 동작</font>한다.
결국 기존의 데이터를 덮어 씌우는것이 아닌 <font color="#d99694">기존의 데이터를 바탕으로 이전 버전의 데이터와 비교해서 변경된 내용을 기록한다.</font>
- 일반적으로 RDBMS보다 매우 빠르게 작동
- 사용하지 않는 데이터가 계속 쌓이게 되므로 데이터를 정리하는 시스템이 필요
- 데이터 버전이 충돌하면 애플리케이션 영역에서 이러한 문제를 해결해야 함

잠금을 필요로 하지 않기 때문에 RDBMS보다 빠르다. 데이터를 읽기 시작할 때, 다른 사람이 그 데이터를 삭제하거나 수정하더라도 영향을 받지 않고 데이터를 사용할 수 있다. 대신 데이터가 계속 쌓이게 되므로 데이터를 정리해주는 시스템이 필요하다.

**MySQL에서의 MVCC(Multi-Version Concurrency Control)**
MySQL의 InnoDB에서는 Undo Log를 활용해  MVCC 기능을 구현한다. 이해를 위해 실제 쿼리문 예시를 가지고 살펴보도록 하자.

먼저 `member`테이블을 만들자.
```sql
CREATE TABLE member ( 
	id INT NOT NULL,
	name VARCHAR(20) NOT NULL,
	area VARCHAR(100) NOT NULL,
	PRIMARY KEY(m_id),
	INDEX idx_area(area) 
)
```
`member`이라는 테이블을 생성해보도록 하자.

그런 다음 다음과 같은 데이터를 주입해보자.
```sql
INSERT INTO member(id, name, area) VALUES (1, "MangKyu", "서울");
```

그러면 데이터는 다음과 같은 상태가 된다. 메모리와 디스크에 해당 데이터가 동일하게 저장된다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6txYY%2FbtrRZN1BXuj%2F8iRkKk4RkjnxhKG8pwrUbk%2Fimg.png)

`UPDATE`로 데이터를 바꾸자
```sql
UPDATE member SET area = "경기" WHERE id = 1;
```

`UPDATE`가 실행되면 다음과 같은 결과가 나온다. COMMIT 실행 여부와 무관하게 버퍼 풀은 새로운 값으로 갱신된다. 그리고 `Undo 로그`에는 변경전의 값이 복사된다. 그리고 디스크에 있는 데이터는 시점에 따라 다를 수 있어서 ?로 표시되어있다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcg8wmO%2FbtrSGWDIdL5%2FA8tBSFwPK9G6bPNxJeFtYK%2Fimg.png)
그러면 `SELECT`를 하면 어떤 값이 나올까?
```sql
SELECT * FROM member WHERE id = 1;
```

그 결과는 격리수준에 따라서 다르다. READ_UNCOMMITTED라면 버퍼 풀의 데이터를 읽어서 반환해준다. 커밋 여부와 무관하게 변경된 데이터를 읽어 반환해준다.
READ_COMMITED 이나 그 이상의 격리 수준(REPEATABLE_READ, SERIALIZABLE)이면 이전의 Undo 로그 영역의 데이터를 반환하게 된다.
여기서 Undo Log 영역의 데이터는 커밋 혹은 롤백을 호출하여 InnoDB 버퍼풀도 이전의 데이터로 복구되고, 더 이상 언두 영역을 필요로 하는 트랜잭션이 더는 없을 때 비로소 삭제된다.
