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

![몇 분이서 오셨어요?](../img/week2/몇%20분이서%20오셨어요?.png)

### 코드

```SQL
SELECT *
  FROM tips
  WHERE size%2 == 1;
```
