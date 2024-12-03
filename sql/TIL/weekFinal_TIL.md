# SQL Advanced

## 1. PIVOT과 UNPIVOT

### 내용 요약

[유튜브 링크](https://www.youtube.com/watch?v=FINRIH6Bmq0)

### 문제 풀이

```SQL
SELECT
```

\* 필요한 경우 아래 소스 참고하기 <br>
[CASE WHEN으로 PIVOT TABLE 만들기](https://techblog-history-younghunjo1.tistory.com/159)

## 2. 성능 최적화 기법

### 내용 요약

[SQL 쿼리 성능 최적화를 위한 튜닝 팁 6가지](https://community.heartcount.io/ko/query-optimization-tips/)

### 문제

여러분은 `customer_orders`라는 테이블을 관리하는 데이터베이스 관리자로 일하고 있습니다. 이 테이블에는 고객의 주문 정보가 저장되어 있으며, 각 고객이 주문한 제품과 수량, 가격 정보가 포함되어 있습니다. 또한, 고객들이 특정 제품을 재구매한 비율을 계산하려고 합니다.

#### 테이블 구조:

1. **customers** 테이블
   - `customer_id` (고객 ID, PRIMARY KEY)
   - `name` (고객 이름)
2. **orders** 테이블
   - `order_id` (주문 ID, PRIMARY KEY)
   - `customer_id` (고객 ID, FOREIGN KEY)
   - `order_date` (주문 날짜)
3. **order_details** 테이블
   - `order_id` (주문 ID, FOREIGN KEY)
   - `product_id` (제품 ID)
   - `quantity` (수량)
   - `unit_price` (단가)

---

#### 요구 사항:

1. **`avg_order_value`**: 고객별 평균 주문 금액을 계산하여 `customers` 테이블에 업데이트하세요.
   - `avg_order_value`는 각 고객이 한 번의 주문에서 지출한 평균 금액입니다.
2. **`total_spent`**: 고객별 총 지출 금액을 계산하여 `customers` 테이블에 업데이트하세요.
   - `total_spent`는 고객이 지금까지 지출한 총 금액입니다.
3. **`num_orders`**: 고객이 총 몇 번의 주문을 했는지 계산하여 `customers` 테이블에 업데이트하세요.
   - `num_orders`는 고객이 주문한 총 개수입니다.
4. **`repurchase_rate`**: 고객의 재구매 비율을 계산하여 `customers` 테이블에 업데이트하세요.
   - `repurchase_rate`는 각 고객이 2번 이상 주문한 제품 비율을 의미합니다. (즉, 재구매한 제품이 전체 구매 제품 중 몇 퍼센트를 차지하는지)

#### 예시:

- 고객 A는 3번 주문을 했고, 그 중 2개의 제품을 재구매했습니다.
  - 평균 주문 금액: 100,000원
  - 총 지출 금액: 300,000원
  - 주문 횟수: 3번
  - 재구매 비율: 66.67%

### 정답

```SQL
SELECT
```
