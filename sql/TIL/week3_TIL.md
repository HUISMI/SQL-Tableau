# SQL 문제풀이

## 조건에 맞는 사용자와 총 거래금액 조회하기

![조건에 맞는 사용자와 총 거래금액 조회하기](../img/week3/조건에%20맞는%20사용자와%20총%20거래금액%20조회하기.png)

### 코드

```SQL
SELECT USER_.USER_ID, USER_.NICKNAME, SUM(BOARD.PRICE) AS TOTAL_SALES
    FROM USED_GOODS_USER AS USER_
    JOIN USED_GOODS_BOARD AS BOARD ON USER_.USER_ID = BOARD.WRITER_ID
    WHERE STATUS = 'DONE'
    GROUP BY USER_.USER_ID
    HAVING TOTAL_SALES >= 700000
    ORDER BY TOTAL_SALES ASC;
```

- `JOIN`할 때 다른 테이블의 별명 설정 후 굳이 `FROM`절에 적지 않고 `SELECT`절에 써도 되는구나!
- `JOIN` 정확히 어떻게 사용하는지 파악 완료

## 업그레이드 할 수 없는 아이템 구하기

![업그레이드 할 수 없는 아이템 구하기](../img/week3/업그레이드%20불가%20아이템.png)

### 코드

```SQL
SELECT DISTINCT INFO.ITEM_ID, INFO.ITEM_NAME, INFO.RARITY
    FROM ITEM_INFO AS INFO
    LEFT JOIN ITEM_TREE AS TREE ON INFO.ITEM_ID = TREE.PARENT_ITEM_ID
    WHERE TREE.PARENT_ITEM_ID IS NULL
    ORDER BY INFO.ITEM_ID DESC;
```

-> 우선, window 함수를 사용하지 않고 풀었다.

- 내가 JOIN에 대한 개념이 햇갈려 JOIN시 행이 두 개가 생길 수 있음을 잠깐 까먹었다..🥲(실화니 나)<br>
  그래서 많이 헤매다가 개념이 갑자기 머리에 돌아왔고 풀었다.
- 여기서 가장 중요한 점은 아마 `TREE.PARENT_ITEM_ID IS NULL`인 곳일 것이다.<br>
  왜냐하면, `PARENT_ITEM_ID`로 연결되지 않는 행이야말로 최종 업그레이드 되지 않는 아이템일 것이기 때문이다.

## 조건에 맞는 개발자 찾기

![조건에 맞는 개발자 찾기](../img/week3/조건에%20맞는%20개발자%20찾기.png)

### 코드

```SQL
SELECT DISTINCT D.ID, D.EMAIL, D.FIRST_NAME, D.LAST_NAME
    FROM DEVELOPERS AS D
    JOIN SKILLCODES AS S ON S.NAME = 'C#' OR S.NAME = 'Python'
    WHERE D.SKILL_CODE & S.CODE > 0
    ORDER BY D.ID ASC;
```

- 2진수 비트 비교!

  - 2진수 비트 연산자 &의 사용!
  - 하나라도 겹치면 1 이상의 값을 내놓기에

- `JOIN`
  - 우선 `ON` 절을 통해 기준키 없이 카티션 곱으로 결합되는 행을 축소하는 제약을 우선 걸었다.
  - 그리고 `WHERE` 절을 통해 2진수 비트값이 1이상인 값을 내놓는 행들만 또 골라냈다.
  - 마지막으로 `DISTINCT`를 통해 `JOIN`을 통해 발생할 수 있는 중복된 레코드를 제거했다!
