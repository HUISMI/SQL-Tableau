# SQL 문제풀이

## 우리 플랫폼에 정착한 판매자1

![우리 플랫폼에 정착한 판매자1](../img/week2/우리%20플랫폼에%20정착한%20판매자1.png)

### 코드

```SQL
SELECT seller_id, COUNT(DISTINCT order_id) AS orders
  FROM olist_order_items_dataset
  GROUP BY seller_id
  HAVING COUNT(DISTINCT order_id) >= 100;
```

- 여기서 마지막 `COUNT(DISTINCT order_id) >= 100;`을 `orders >= 100;`으로 별칭으로 사용해도 된다.
  <br>
  <br>
- **왜?** : SQL에서 별칭(alias)는 SELECT 절에서 정의되지만, 실제로는 HAVING 절에서도 사용할 수 있기 떄문이다!
  <br>
  <br>
- 여기서 중요한 점은, order_id를 DISTINCT로 했다는 점이다. 하나의 주문에 여러 상품이 있을 수 있으므로, 단순히 `COUNT(order_id)`를 하면 동일한 주문이 중복으로 세어질 가능성이 있다.

## 몇 분이서 오셨어요?

![몇 분이서 오셨어요?](../img/week2/몇%20분이서%20오셨어요.png)

### 코드

```SQL
SELECT *
  FROM tips
  WHERE size%2 == 1;
```

## 최고의 근무일을 찾아라

![최고의 근무일을 찾아라](../img/week2/최고의%20근무일을%20찾아라.png)

### 코드

```SQL
SELECT day, ROUND(daily_tip, 2) AS tip_daily
  FROM (
    SELECT day, SUM(tip) AS daily_tip
      FROM tips
      GROUP BY day
  )
  ORDER BY daily_tip DESC
  LIMIT 1;
```

- 서브쿼리의 중요성
  - 서브쿼리에서 미리 `GROUP BY`를 하고, 서브쿼리에서 정의한 컬럼명을 본쿼리에서 사용.
  - <**주의**> 서브쿼리에서 이미 집계함수를 사용했으므로 본 쿼리에서 집계함수를 사용하지 않도록 하기
    <br>
    <br>
- 가장 큰 값을 출력하려면 어렵게 생각하지 말고, 그냥 `DESC`하고 `LIMIT 1` 걸기!
