# Query

### 1. Date of Birth and address of John B Smith

```sql
SELECT BDATE, ADDRESS 
FROM EMPLOYEE20231056 
WHERE FNAME='John' AND LNAME='Smith';
```

### 2. Name and address of employees working in Research department

```sql
SELECT E.FNAME, E.LNAME, E.ADDRESS 
FROM EMPLOYEE20231056 E, DEPARTMENT20231056 D 
WHERE E.DNO=D.DNUMBER AND D.DNAME='Research';
```

### 3. Project Number, Department Number, Name, Address, and Date of Birth of all employees

```sql
SELECT P.PNUMBER, D.DNUMBER, E.FNAME, E.LNAME, E.ADDRESS, E.BDATE 
FROM EMPLOYEE20231056 E, DEPARTMENT20231056 D, PROJECT20231056 P 
WHERE E.DNO=D.DNUMBER AND D.DNUMBER=P.DNUM;
```

### 4. Employee's name with their manager's name

```sql
SELECT E.FNAME AS EMPLOYEE_NAME, E.MINIT, E.LNAME, M.FNAME AS MANAGER_NAME, E.MINIT, E.LNAME 
FROM EMPLOYEE20231056 E, EMPLOYEE20231056 M
WHERE E.SUPERSSN = M.SSN;
```

### 5. All employees working in department 5

```sql
SELECT * 
FROM EMPLOYEE20231056 
WHERE DNO=5;
```

### 6. All distinct salaries with name of employees

```sql
SELECT DISTINCT SALARY 
FROM EMPLOYEE20231056;
```

### 7. Project numbers for projects involving employee Smith (as worker or manager)

```sql
SELECT DISTINCT P.PNUMBER 
FROM PROJECT20231056 P, WORKS_ON20231056 W, EMPLOYEE20231056 E, DEPARTMENT20231056 D
WHERE 
(E.LNAME='Smith' AND E.SSN=W.ESSN AND W.PNO=P.PNUMBER)
OR
(E.LNAME='Smith' AND D.MGRSSN=E.SSN AND D.DNUMBER=P.DNUM);
```

### 8. Employees whose address is in Houston, Texas

```sql
SELECT * 
FROM EMPLOYEE20231056 
WHERE ADDRESS LIKE '%Houston, TX%';
```

### 9. Employees born during the 1950s

```sql
SELECT * 
FROM EMPLOYEE20231056 
WHERE BDATE BETWEEN '01-JAN-1950' AND '31-DEC-1959';
```

### 10. Salaries with 10% raise for employees working on 'ProductX' projects

```sql
SELECT E.FNAME, E.LNAME, E.SALARY*1.1 AS NEW_SALARY
FROM EMPLOYEE20231056 E, WORKS_ON20231056 W, PROJECT20231056 P
WHERE E.SSN=W.ESSN AND W.PNO=P.PNUMBER AND P.PNAME='ProductX';
```

### 11. Employees in department 5 with salary between 30000 and 40000

```sql
SELECT * 
FROM EMPLOYEE20231056 
WHERE DNO=5 AND SALARY BETWEEN 30000 AND 40000;
```

### 12. Employees and the projects they are working on, ordered by dept and name

```sql
SELECT D.DNUMBER, E.FNAME, E.MINIT, E.LNAME, P.PNAME
FROM EMPLOYEE20231056 E, WORKS_ON20231056 W, PROJECT20231056 P, DEPARTMENT20231056 D
WHERE E.SSN=W.ESSN AND W.PNO=P.PNUMBER AND E.DNO=D.DNUMBER
ORDER BY D.DNUMBER, E.FNAME, E.MINIT, E.LNAME;
```

### 13. Employees who have dependent with same first name and same sex

```sql
SELECT E.FNAME, E.LNAME
FROM EMPLOYEE20231056 E, DEPENDENT20231056 DEP
WHERE E.SSN=DEP.ESSN AND E.SEX=DEP.SEX AND E.FNAME=DEP.DEPENDENT_NAME;
```

### 14. Employees who work on all projects controlled by dept 5

```sql
SELECT FNAME, LNAME 
FROM EMPLOYEE20231056
WHERE NOT EXISTS (
    (SELECT PNUMBER FROM PROJECT20231056 WHERE DNUM=5)
    MINUS
    (SELECT PNO FROM WORKS_ON20231056 WHERE ESSN=SSN)
);
```

### 15. Employees who have no dependent

```sql
SELECT FNAME, LNAME 
FROM EMPLOYEE20231056 
WHERE SSN NOT IN 
(
	SELECT ESSN FROM DEPENDENT20231056
);
```

### 16. Managers who have at least one dependent

```sql
SELECT FNAME, MINIT, LNAME, SSN
FROM EMPLOYEE20231056
WHERE EXISTS
(
    SELECT *
    FROM DEPENDENT20231056
    WHERE SUPERSSN=ESSN
);
```

### 17. SSNs of employees working on project 1, 2, and 3

```sql
SELECT DISTINCT ESSN 
FROM WORKS_ON20231056
WHERE PNO IN (1,2,3);
```

### 18. Employees who do not have supervisors

```sql
SELECT FNAME, LNAME 
FROM EMPLOYEE20231056 
WHERE SUPERSSN IS NULL;
```

### 19. Sum, max, min, and average salary of all employees

```sql
SELECT SUM(SALARY), MAX(SALARY), MIN(SALARY), AVG(SALARY) 
FROM EMPLOYEE20231056;
```

### 20. Salary stats for Research department

```sql
SELECT SUM(SALARY), MAX(SALARY), MIN(SALARY), AVG(SALARY)
FROM EMPLOYEE20231056 E, DEPARTMENT20231056 D
WHERE E.DNO=D.DNUMBER AND D.DNAME='RESEARCH';
```

### 21. Count distinct salary values in department

```sql
SELECT COUNT(DISTINCT SALARY) 
FROM EMPLOYEE20231056 GROUP BY DNO;
```

### 22. Department number, employee count, and avg salary per department

```sql
SELECT DNO, COUNT(*), AVG(SALARY) 
FROM EMPLOYEE20231056 GROUP BY DNO;
```

### 23. Project number, name, and number of employees working on it

```sql
SELECT P.PNUMBER, P.PNAME, COUNT(W.ESSN) AS NUM_EMPLOYEES
FROM PROJECT20231056 P, WORKS_ON20231056 W
WHERE P.PNUMBER=W.PNO
GROUP BY P.PNUMBER, P.PNAME;
```

### 24. Departments with >5 employees earning >40000

```sql
SELECT DNO, COUNT(*) AS NUM_EMPLOYEES 
FROM EMPLOYEE20231056 E
WHERE SALARY>40000
GROUP BY DNO
HAVING COUNT(*)>5;
```

### 25. Delete record from Employee where LNAME='Brown'

```sql
DELETE FROM EMPLOYEE20231056 
WHERE LNAME='Brown';
```

### 26. Change project location and department number where project number=10

```sql
UPDATE PROJECT20231056 
SET PLOCATION='NEW YORK', DNUM=2 
WHERE PNUMBER=10;
```

### 27. Create a view from Employee, Project, Works_on with selected attributes

```sql
CREATE VIEW EMP_PROJECT_VIEW20231056 AS
SELECT E.FNAME, E.LNAME, E.SSN, P.PNAME, P.PLOCATION, W.HOURS
FROM EMPLOYEE20231056 E, PROJECT20231056 P, WORKS_ON20231056 W
WHERE E.SSN=W.ESSN AND P.PNUMBER=W.PNO;
```