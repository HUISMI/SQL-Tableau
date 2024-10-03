# SQL 문제풀이

## ROOT 아이템 구하기

![ROOT 아이템 구하기](../img/week4/ROOT아이템.png)

### 코드

```SQL
SELECT INFO.ITEM_ID, INFO.ITEM_NAME
    FROM ITEM_INFO AS INFO
    JOIN ITEM_TREE AS TREE ON INFO.ITEM_ID = TREE.ITEM_ID
    WHERE TREE.PARENT_ITEM_ID IS NULL
    ORDER BY INFO.ITEM_ID ASC;
```

- 저번에 풀었던 최종 아이템보다 쉽다.
- 그냥 `TREE.PARENT_ITEM_ID`가 NULL인 값을 찾으면 된다.

## 노선별 평균 역 사이 거리 조회하기

![노선별 평균 역 사이 거리 조회하기](../img/week4/노선별거리.png)

### 코드

```SQL
SELECT
    ROUTE,
    CONCAT(ROUND(SUM(D_BETWEEN_DIST),1), 'km') AS TOTAL_DISTANCE,
    CONCAT(ROUND(AVG(D_BETWEEN_DIST),2), 'km') AS AVERAGE_DISTANCE
    FROM SUBWAY_DISTANCE
    GROUP BY ROUTE
    ORDER BY ROUND(SUM(D_BETWEEN_DIST),1) DESC;
```

- 우선 처음에 위의 order절을 `ORDER BY TOTAL_DISTANCE DESC` 로 두었었는데 코드는 완벽하다고 생각했는데 코드는 돌아가지만 제출하기를 눌렀다하면 실패했다.
- 알고보니 `ORDER BY TOTAL_DISTANCE DESC` 이런 식으로 할 경우 거리를 정렬할 때 숫자가 아닌 문자열 기준으로 정렬한다는 것이다. 즉, 10.0km와 9.0km가 있으면 10과 9를 기준으로 정렬하는게 아니라 1과 9를 기준으로 정렬하는 엄청난 오류가 발생한다.

=> 따라서, 숫자 데이터와 관련된 정렬은 해당 데이터값 수식을 그대로 입력하는 것이 오류를 예방하는 방법이다

## 헤비 유저가 소유한 장소

![헤비 유저가 소유한 장소](../img/week4/장소.png)

### 코드

```SQL
WITH HEAVY_USER AS (
    SELECT HOST_ID
    FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(ID) >= 2
)

SELECT ID, NAME, PLACES.HOST_ID
    FROM PLACES
    JOIN HEAVY_USER ON PLACES.HOST_ID = HEAVY_USER.HOST_ID
    ORDER BY ID;
```

- 어떻게 할지 고민하다가, 그냥 테이블 따로 만들어서 `HOST_ID`를 기준으로 조인시켜서 전체 출력하면 되는거 아닌가?에 대한 생각으로 진행했다.
- 아주 성공적!!
- (그냥 바로 `GROUP BY`적용하면 전체 레코드 출력이 안됨.)
