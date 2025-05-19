# 📌 윈도우 함수 + 부서별 연봉 조건 오답노트

| 항목        | 설명                                                                 |
| --------- | ------------------------------------------------------------------ |
| 핵심 개념   | `COUNT(*) OVER (PARTITION BY ... ORDER BY ... RANGE/ROWS ...)` 사용    |
| 조건 설명   |  1. 부서별로(`PARTITION BY department_id`)<br>2. 연봉 기준 오름차순(`ORDER BY salary`)<br>3. 현재 연봉 기준 **100 ~ 200 사이의 연봉자 수** 구하기 |
| 핵심 함수 구성요소 | `COUNT(*)`, `OVER`, `PARTITION`, `ORDER BY`, `RANGE` 또는 `ROWS` |

---

## ✅ 정답: **2번**

---

### ✅ 2번 설명

> `RANGE BETWEEN 100 PRECEDING AND 200 FOLLOWING`
> ⇒ **연봉을 기준으로 숫자 범위**를 의미함.

* `RANGE`는 값 자체의 범위 (ex: salary ± 범위)
* `ROWS`는 순번 기준 (현재 행 기준 앞뒤로 몇 개)

**요구사항이 "연봉이 100\~200 사이"이므로**,
`RANGE`를 사용하는 것이 적절합니다.

또한 `PARTITION BY department_id`를 통해
**부서별로 연봉자 수를 계산**하는 조건도 만족

---

### 📌 `RANGE` 예시: 연봉 기준으로 100 \~ 200 사이의 급여자 수 세기

#### 💡 테이블 예시 (`employees`)

| employee\_id | department\_id | salary |
| ------------ | -------------- | ------ |
| 1            | 10             | 100    |
| 2            | 10             | 120    |
| 3            | 10             | 150    |
| 4            | 10             | 170    |
| 5            | 10             | 200    |
| 6            | 10             | 250    |

---

각 사원의 급여를 기준으로 **±50** 범위에 속하는 사원 수를 구하고 싶을 때:

```sql
SELECT
  employee_id,
  salary,
  COUNT(*) OVER (
    PARTITION BY department_id
    ORDER BY salary
    RANGE BETWEEN 50 PRECEDING AND 50 FOLLOWING
  ) AS salary_range_count
FROM employees;
```

---

#### 🔍 예를 들어보면...

* `salary = 150`인 사원 기준으로 **100 \~ 200**의 급여를 가진 사원을 세면
  → `salary 100`, `120`, `150`, `170`, `200` → **5명**
* `salary = 100`이면
  → `salary 100`, `120`, `150` → **3명**
* `salary = 250`이면
  → `salary 200`, `250` → **2명**


---

### ❌ 1번 설명

* `ROWS BETWEEN 100 PRECEDING AND 200 FOLLOWING`
  → **값 기준이 아닌 행 개수 기준**
  → 문제의 "연봉 범위"와는 맞지 않음

---

### ❌ 3번 설명

* `PARTITION BY`만 존재, `ORDER BY`와 `RANGE/ROWS` 없음
  → **지정된 연봉 범위를 고려하지 않음**

---

### ❌ 4번 설명

* `PARTITION BY department_id`가 없음
  → **부서별 계산이 아님**

---

## 추가 개념 정리

| 키워드         | 설명                               |
| ----------- | -------------------------------- |
| `RANGE`     | 값 자체의 범위를 기준으로 윈도우 프레임 생성        |
| `ROWS`      | 현재 행 기준 몇 행 앞/뒤로 볼지, 물리적 행 개수 기준 |
| `PARTITION` | 그룹별 연산을 수행할 때 사용 (ex. 부서별 집계)    |
| `ORDER BY`  | 윈도우 함수에서 프레임 지정 시 정렬 기준 필수       |

---

## 느낀 점

* `RANGE`와 `ROWS`의 차이를 명확히 이해하고 있어야됨
* **값 기준의 범위 계산에는 `RANGE`**, **행 수 기준 계산에는 `ROWS`** 사용

