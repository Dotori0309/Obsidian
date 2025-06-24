What is RDBMS(Relational Database Management System)
SQL(Structured Query Language)
### DML(Data Manipulation Language) - 데이터 조작
SELECT - 데이터 조회
INSERT - 데이터 삽입
UPDATE - 데이터 수정
DELETE - 데이터 삭제

### DDL(Data Definition Language) - 데이터 정의
CREATE 테이블, 데이터베이스 생성
ALTER 테이블 구조 변경
DROP 테이블, 데이터베이스 삭제
TRUNCATE 테이블 내 데이터 전체 삭제 (구조 유지)

Database Modeling

ERD(Entity Relational Diagram)

## SQL 학습 정리

---

## **1. 데이터베이스 기본**

### **DDL (데이터 정의어)**

- **테이블 생성**
    
    sql
    
    복사편집
    
    `create table emp (     empno int primary key,     ename varchar(10) unique,     job varchar(20) not null,     mgr int,     hiredate date default now(),     sal int check (sal > 0),     comm int,     deptno int );`
    
- **제약조건**
    
    |제약조건|설명|
    |---|---|
    |`primary key`|기본키, 중복 X, NULL X|
    |`unique`|유일값 허용, NULL 가능|
    |`check`|특정 조건 만족해야 함|
    |`foreign key`|참조키, 참조 무결성 보장|
    
- **외래키 설정**
    
    sql
    
    복사편집
    
    `alter table emp add constraint foreign key (deptno) references dept(deptno) on update cascade;`
    

---

## **2. 테이블/데이터 관리**

### **테이블 목록 조회**

sql

복사편집

`show tables; desc emp;`

### **데이터 삽입 예시**

sql

복사편집

`insert into emp values(7369, 'SMITH', 'CLERK', 7902, str_to_date('17-12-1980', '%d-%m-%Y'), 800, NULL, 20);`

### **데이터 수정/삭제**

- 삭제 시 외래키 참조 주의
    
- 삭제 순서 : `emp_car` → `emp` → `dept`
    

---

## **3. 데이터 조회(SELECT)**

### **기본 조회**

sql

복사편집

`select * from emp;`

### **조건 검색**

sql

복사편집

`select * from emp where sal between 1000 and 1500; select * from emp where deptno in (20, 30); select * from emp where ename like '_A%';`

### **서브쿼리 활용**

sql

복사편집

`select * from emp where sal > (select sal from emp where ename = 'ADAMS');`

### **정렬 & 제한**

sql

복사편집

`select ename, sal from emp order by sal desc limit 3;`

---

## **4. 집계/함수 활용**

### **문자열 함수**

sql

복사편집

`select concat(ename, '-', job, '-', sal) from emp; select length('이말숙'); -- 바이트 기준 select char_length('이말숙'); -- 글자 수 기준`

### **날짜 함수**

sql

복사편집

`select curdate(), curtime(), now(); select date_format(hiredate, '%Y년 %m월 %e일') from emp;`

### **CASE 조건 처리**

sql

복사편집

`select ename, sal, case     when sal < 1000 then '하위'     when sal < 2000 then '중위'     when sal < 5000 then '상위'     else '없음' end as grade from emp;`

---

## **5. 실습 응용 예시**

### **연봉 계산**

sql

복사편집

`select ename, sal, (sal * 12) as annu_sal, (sal * 12 * 0.967) as annu_sal_tax from emp;`

### **부서/포인트 문제**

- 포인트 부여 규칙
    
    |조건|포인트|
    |---|---|
    |sal ≤ 1000|300|
    |sal ≤ 2000|200|
    |sal ≤ 5000|100|
    |그 외|0|
    

sql

복사편집

`select ename, sal, case     when sal <= 1000 then 300     when sal <= 2000 then 200     when sal <= 5000 then 100     else 0 end as point from emp;`

---

## **6. 추가 실습: shopdb**

### **회원 테이블**

sql

복사편집

`create table cust (     id varchar(20) primary key,     pwd varchar(20),     name varchar(30) );`

### **이메일 분리 예시**

select 
    substring('jmlee@tonesol.com', 1, instr('jmlee@tonesol.com', '@') - 1) as id,
    substring('jmlee@tonesol.com', instr('jmlee@tonesol.com', '@') + 1, instr('jmlee@tonesol.com', '.') - instr('jmlee@tonesol.com', '@') - 1) as domain;
