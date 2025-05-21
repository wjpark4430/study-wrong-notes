# 📌 트랜잭션과 DDL(ORACLE vs SQL SERVER) 오답노트

| 항목                | 설명                                                                                 |
|-------------------|------------------------------------------------------------------------------------|
| 트랜잭션(ROLLBACK)   | DML(UPDATE 등)은 ROLLBACK으로 취소 가능, DDL(CREATE 등)은 DBMS마다 다름                      |
| ORACLE의 DDL 처리     | DDL 실행 시 **자동 커밋(AUTO COMMIT)** 발생 → ROLLBACK 불가                             |
| SQL SERVER의 DDL 처리 | DDL도 트랜잭션에 포함되어 ROLLBACK 가능 (명시적 트랜잭션 내에서)                         |
| 주의할 점             | DDL/트랜잭션 처리 방식이 DBMS마다 다르므로, ROLLBACK 동작을 반드시 구분해야 함                |

---

## 문제

![34-30번 문제](../images/34-30.png)

---

## ✅ 정답: **4번**

### ✅ 1번 설명
> SQL SERVER의 경우 ROLLBACK이 된 후 UPDATE와 CREATE 구문 모두 취소된다

- SQL SERVER는 트랜잭션 내에서 DML/DDL 모두 ROLLBACK 가능  
- **설명 맞음**

---

### ✅ 2번 설명
> SQL SERVER의 경우 ROLLBACK이 된 후 SQLD_34_21_TEMP는 만들어지지 않는다

- CREATE TABLE도 트랜잭션에 포함되어 ROLLBACK 시 생성 안 됨  
- ✅ **설명 맞음**

---

### ❌ 3번 설명(정답)  
> ORACLE의 경우 ROLLBACK이 된 후 UPDATE와 CREATE 구문 모두 취소된다

- ORACLE은 DDL(CREATE TABLE) 실행 시 **자동 커밋** 발생  
- ROLLBACK 해도 DDL 이전까지의 DML만 취소되고,  
  **CREATE TABLE은 취소되지 않음**  
- ❌ **설명 틀림** 

---

### ✅ 4번 설명
> ORACLE의 경우 UPDATE는 취소되지 않는다

- ORACLE에서 UPDATE는 ROLLBACK 시 **반드시 취소됨**
- 하지만 ORACLE은 DDL(CREATE TABLE) 실행 시 **자동 커밋** 발생
- ✅ **설명 맞음**

---

## 복습 포인트

| 구분      | ORACLE                                      | SQL SERVER                        |
|---------|---------------------------------------------|----------------------------------|
| DML      | ROLLBACK 가능                                 | ROLLBACK 가능                     |
| DDL      | **자동 커밋 발생 → ROLLBACK 불가**               | 트랜잭션 내에서 ROLLBACK 가능         |
| 혼합 사용 | DDL 이후 ROLLBACK 시 DML만 취소, DDL은 남아있음     | DML/DDL 모두 ROLLBACK 시 취소         |

---

## 느낀 점

* **DBMS별 트랜잭션 처리 방식(특히 DDL의 ROLLBACK 처리)**을 반드시 구분해서 기억
* ORACLE은 DDL 실행 시 자동 커밋이 발생하므로, DML만 롤백됨