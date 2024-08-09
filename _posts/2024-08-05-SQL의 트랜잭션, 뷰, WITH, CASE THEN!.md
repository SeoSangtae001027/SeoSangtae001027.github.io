---
title: "SQL의 트랜잭션, 뷰, WITH, CASE THEN!"
date: 2024-08-05 10:00:00 +09:00 
# last_modified_at:
categories: [패스트캠퍼스, 데이터분석]
tags: [SQL, 트랜잭션, VIEW, WITH, CASE THEN]
---

## 트랜잭션
트랜잭션(Transaction)은 데이터베이스에서 일어나는 하나의 작업 단위로, 여러 SQL 작업을 묶어서 처리할 수 있어요. 트랜잭션의 주요 목적은 데이터베이스의 일관성을 유지하는 거에요.

### 커밋 (COMMIT)
커밋은 트랜잭션의 모든 변경 사항을 영구적으로 데이터베이스에 저장하는 명령이에요. 즉, 커밋을 실행하면 트랜잭션 내의 모든 작업이 확정되어 다른 사용자가 볼 수 있게 돼요.

```sql
BEGIN TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE account_id = 1;
UPDATE account SET balance = balance + 100 WHERE account_id = 2;
COMMIT;
```
위의 예시에서는 두 개의 계좌를 업데이트하는 트랜잭션을 커밋하여 모든 변경 사항이 데이터베이스에 반영돼요.

### 롤백 (ROLLBACK)
롤백은 트랜잭션 내의 모든 변경 사항을 취소하고, 트랜잭션 시작 전의 상태로 되돌리는 명령이에요. 롤백은 트랜잭션이 실패하거나 오류가 발생했을 때 사용돼요.

```sql
BEGIN TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE account_id = 1;
UPDATE account SET balance = balance + 100 WHERE account_id = 2;
ROLLBACK;
```
위의 예시에서는 트랜잭션이 롤백되므로 계좌의 잔액 변경이 취소돼요.

- 사용 이유  
트랜잭션의 커밋과 롤백은 데이터베이스의 일관성과 무결성을 보장하기 위해 필요해요. 트랜잭션 도중 문제가 발생하면 롤백을 통해 원자성을 유지하고, 모든 작업이 성공적으로 완료되었을 때 커밋을 통해 데이터베이스에 확정적인 변경을 적용할 수 있어요.

## 뷰 (VIEW)
뷰(View)는 하나 이상의 테이블에서 데이터를 조회할 수 있는 가상 테이블이에요. 뷰는 실제 데이터를 저장하지 않고, 데이터베이스 쿼리를 기반으로 데이터를 동적으로 보여줘요.
뷰는 CREATE VIEW 명령을 사용하여 생성할 수 있어요.

```sql
CREATE VIEW employee_view AS
SELECT employee_id, first_name, last_name
FROM employees
WHERE department = 'Sales';
```
위의 예시에서는 'Sales' 부서의 직원 정보를 조회하는 뷰를 생성해요.

- 사용 이유
보안: 특정 열이나 행만 노출시켜 민감한 정보를 보호해요.     
단순화: 복잡한 쿼리를 간단한 뷰로 추상화하여 사용자가 쉽게 접근할 수 있게 해요.    
재사용성: 동일한 쿼리를 반복할 필요 없이 뷰를 통해 재사용할 수 있어요.    

## WITH (Common Table Expressions, CTE)
CTE(Common Table Expression)는 SQL 쿼리에서 재사용 가능한 임시 결과 집합을 정의하는 방법이에요. WITH 절을 사용하여 복잡한 쿼리를 더 읽기 쉽고 구조적으로 만들 수 있어요.
CTE는 WITH 절로 시작하며, 이후에 정의된 쿼리를 사용할 수 있어요.

```sql
WITH sales_cte AS (
    SELECT employee_id, SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_id, e.first_name, e.last_name, s.total_sales
FROM employees e
JOIN sales_cte s ON e.employee_id = s.employee_id;
```
위의 예시에서는 sales_cte라는 CTE를 정의하고, 이를 사용하여 직원과 총 매출을 결합한 결과를 조회해요.

- 사용 이유
읽기 쉬움: 복잡한 쿼리를 분해하여 가독성을 높여요.    
재사용: CTE를 여러 번 참조할 수 있어 쿼리의 중복을 줄여요.    
재귀 쿼리: 재귀적인 데이터 구조를 처리하는 데 유용해요.     

## CASE THEN
CASE THEN 구문은 SQL에서 조건에 따라 다른 결과를 반환하는 데 사용돼요. 이 구문은 여러 조건을 평가하고, 해당 조건에 맞는 값을 반환해요.
CASE THEN은 SELECT, UPDATE, DELETE 등 여러 SQL 명령어에서 사용할 수 있어요.

```sql
SELECT employee_id,
       CASE 
           WHEN salary < 50000 THEN 'Junior'
           WHEN salary BETWEEN 50000 AND 100000 THEN 'Mid-level'
           ELSE 'Senior'
       END AS position
FROM employees;
```
위의 예시에서는 직원의 급여에 따라 'Junior', 'Mid-level', 'Senior'로 직급을 분류해요.

- 사용 이유
조건부 로직: 쿼리 내에서 조건에 따라 다른 결과를 반환할 수 있어요.
데이터 변환: 데이터의 특정 조건에 따라 다른 형식으로 변환할 수 있어요.
비즈니스 로직 구현: 복잡한 비즈니스 규칙을 쿼리 내에서 직접 구현할 수 있어요.