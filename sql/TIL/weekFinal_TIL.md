# SQL Advanced

## 1. PIVOT과 UNPIVOT

### 내용 요약

[유튜브 링크](https://www.youtube.com/watch?v=FINRIH6Bmq0)

**정의와 목적**  
PIVOT과 UNPIVOT은 2024년 SQL 커리큘럼에 새롭게 추가된 기능으로, 데이터 구조를 변환하여 Excel 및 SQL에서 데이터를 보다 효과적으로 분석하고 시각화할 수 있도록 돕는 중요한 작업

---

**PIVOT 연산**

- PIVOT 함수는 행(row)을 열(column)로 회전시키는 작업으로, 데이터를 요약하는 데 유용함
  예를 들어, 월별 매출 데이터가 있을 경우, PIVOT을 사용하여 각 월을 열로 변환하고 해당 매출 값을 표시할 수 있음
- 통계 지표를 처리할 때 데이터가 더 시각적으로 접근하기 쉽게 정렬되므로 특히 유용함

---

**UNPIVOT 연산**

- UNPIVOT은 PIVOT의 역과정으로, 열(column)을 다시 행(row)으로 변환함
  이를 통해 데이터를 정규화하거나 추가 분석을 위해 데이터를 준비할 수 있음
- 특히 집계된 데이터를 세부적인 항목으로 분해해야 할 때 유연한 데이터 조작을 가능하게 함

---

#### 데이터 구조와 집계

**데이터 시각화**

- 데이터 구조는 분석 가능성에 크게 영향을 미치는데, 전통적인 데이터 구조는 집계 함수의 활용에 적합하지 않을 수 있음
- PIVOT과 UNPIVOT은 데이터의 가시성과 활용성을 높이는 형식을 만드는데 도움을 줌.
  예를 들어, 직원 정보가 여러 달에 걸쳐 표시된 전형적인 데이터 테이블을 PIVOT을 사용해 시간에 따른 추세를 강조하는 형식으로 변환할 수 있음.

---

**활용 사례**

- **PIVOT**: 월별, 부서별과 같이 데이터를 카테고리로 요약해야 하는 보고서 작성에 자주 사용됨
- **UNPIVOT**: 요약된 데이터의 기초가 되는 세부 데이터를 분석해야 할 때 유용함

### 문제 풀이

```SQL
SELECT
    MAX(CASE WHEN Occupation = 'Doctor' THEN Name END) AS Doctor,
    MAX(CASE WHEN Occupation = 'Professor' THEN Name END) AS Professor,
    MAX(CASE WHEN Occupation = 'Singer' THEN Name END) AS Singer,
    MAX(CASE WHEN Occupation = 'Actor' THEN Name END) AS Actor
FROM
(
    SELECT
        ROW_NUMBER() OVER (PARTITION BY Occupation ORDER BY Name) AS RowNumber,
        Occupation,
        Name
    FROM OCCUPATIONS
) AS tmpTable
GROUP BY RowNumber
ORDER BY RowNumber;
```

- `SELECT` 문에서 CASE WHEN 구분하는 것이 관건이었다.

\* 필요한 경우 아래 소스 참고하기 <br>
[CASE WHEN으로 PIVOT TABLE 만들기](https://techblog-history-younghunjo1.tistory.com/159)

## 2. 성능 최적화 기법

### 내용 요약

[SQL 쿼리 성능 최적화를 위한 튜닝 팁 6가지](https://community.heartcount.io/ko/query-optimization-tips/)

1. 좌변을 연산하지 않을 것. 쿼리에서 컬럼의 좌변에 함수를 적용하면 인덱스 활용이 어려워진다. 예를 들어, WHERE YEAR(date) = 2021 대신 WHERE date >= '2021-01-01' AND date <= '2021-12-31'와 같이 작성하여 인덱스를 효과적으로 활용할 수 있다.

2. OR 대신 UNION을 사용할 것. OR 연산자는 인덱스 사용을 방해할 수 있으므로, 동일한 결과를 얻기 위해 UNION 또는 UNION ALL을 사용하는 것이 좋다. 예를 들어, WHERE department = 'Marketing' OR department = 'IT' 대신 두 개의 쿼리를 UNION으로 결합하여 작성한다.

3. 필요한 Row, Column만 가져올 것 필요한 데이터만 선택적으로 조회하여 불필요한 데이터 전송을 줄이고 성능을 향상시킨다. 예를 들어, SELECT \* 대신 필요한 컬럼만 지정하여 조회한다.

4. 분석 함수를 최대한 활용할 것. 데이터를 그룹화하거나 집계할 때 분석 함수를 활용하면 성능을 높일 수 있다. 예를 들어, GROUP BY와 함께 집계 함수를 사용하여 원하는 결과를 얻을 수 있다.

5. 와일드카드(%)는 끝에 작성하는 것이 더 좋다. LIKE 연산자에서 와일드카드를 문자열 끝에만 사용하면 인덱스 활용이 가능하지만, 문자열 시작이나 중간에 사용하면 인덱스 사용이 어려워지기 때문이다. 예를 들어, LIKE 'abc%'는 인덱스를 활용할 수 있지만, LIKE '%abc'는 그렇지 않다.

6. 계산값을 미리 저장해두고, 나중에 사용할 것 자주 사용되는 계산 결과를 미리 계산하여 저장해두면 쿼리 성능을 향상시킬 수 있다. 예를 들어, 계산된 컬럼을 추가하여 나중에 조회 시 활용할 수 있음.

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
-- 1. avg_order_value 업데이트
UPDATE customers c
SET avg_order_value = (
    SELECT AVG(order_total)
    FROM (
        SELECT o.order_id, SUM(od.quantity * od.unit_price) AS order_total
        FROM orders o
        JOIN order_details od ON o.order_id = od.order_id
        WHERE o.customer_id = c.customer_id
        GROUP BY o.order_id
    ) sub
);

-- 2. total_spent 업데이트
UPDATE customers c
SET total_spent = (
    SELECT SUM(od.quantity * od.unit_price)
    FROM orders o
    JOIN order_details od ON o.order_id = od.order_id
    WHERE o.customer_id = c.customer_id
);

-- 3. num_orders 업데이트
UPDATE customers c
SET num_orders = (
    SELECT COUNT(*)
    FROM orders o
    WHERE o.customer_id = c.customer_id
);

-- 4. repurchase_rate 업데이트
UPDATE customers c
SET repurchase_rate = (
    SELECT
        CASE
            WHEN total_products = 0 THEN 0
            ELSE ROUND((repurchased_products * 100.0) / total_products, 2)
        END
    FROM (
        SELECT COUNT(DISTINCT od.product_id) AS total_products,
               COUNT(DISTINCT CASE
                   WHEN COUNT(DISTINCT o.order_id) >= 2 THEN od.product_id
                   ELSE NULL
               END) AS repurchased_products
        FROM orders o
        JOIN order_details od ON o.order_id = od.order_id
        WHERE o.customer_id = c.customer_id
        GROUP BY od.product_id
    ) AS product_stats
);

```

### 추가질문

1. 정답 쿼리에서 `repurchase_rate`를 구할 때 사용한 `HAVING COUNT(order_details.product_id) > 1`의 의미는 무엇인가요?

- 특정 고객이 특정 제품을 두 번 이상 주문했는지 확인하는 조건이다.

2. 이 문제에서 사용될 수 있는 성능을 최적화하기 위한 방법은 무엇일까요?

- 각 서브쿼리를 별도의 임시 테이블로 저장하거나 공통 테이블 표현식(CTE)를 사용하여 중복 계산을 방지하는 등의 방법이 있다.
