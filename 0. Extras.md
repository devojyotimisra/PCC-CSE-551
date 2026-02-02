# Few Commands to keep in mind

```sql
SELECT * FROM TAB WHERE TNAME LIKE '%20231056';
SELECT * FROM DEPENDENT20231056;

DESC EMPLOYEE20231056;

DROP TABLE EMPLOYEE20231056 CASCADE CONSTRAINTS;
DROP TABLE DEPARTMENT20231056 CASCADE CONSTRAINTS;
DROP TABLE PROJECT20231056 CASCADE CONSTRAINTS;
DROP TABLE WORKS_ON20231056 CASCADE CONSTRAINTS;
DROP TABLE DEPT_LOCATIONS20231056 CASCADE CONSTRAINTS;
DROP TABLE DEPENDENT20231056 CASCADE CONSTRAINTS;

ALTER TABLE EMPLOYEE20231056 DROP CONSTRAINT DNO20231056;
DESC EMPLOYEE20231056;

DEFINE_EDITOR=nano;
SET SERVEROUTPUT ON;
```

# To clear `SQL PLUS` screen

```shell
CLEAR SCR
```

# Connect to UIT Oracle Server

**STEP 1: Open Secure Shell Client**
Open 'Secure Shell Client' on Windows.

**STEP 2: Quick Connect**
Navigate to `File` > `Quick Connect`.

**STEP 3: Configure Connection**
Fill 'Host Name' with `192.168.153.16` and 'User Name' with `uit`.

**STEP 4: Establish Connection**
Click 'Connect' and enter password `uit`.

**STEP 5: SSH into Oracle Server**
```shell
ssh oracle@192.168.153.16
```
(Password: `oracle`)

**STEP 6: Run SQL*Plus**
```shell
sqlplus
```

**STEP 7: Login**
Enter credentials: `scott` / `tiger`.

# Connect to local Oracle Server

1. `USERNAME: system`
2. `PASSWORD: admin`

# Creation of a New User

**STEP 1: Login to the Oracle server**
(Use `system` / `admin` as mentioned above)

**STEP 2: Check current container**
```sql
SHOW CON_NAME;
-- Output: CDB$ROOT (Root container)
```

**STEP 3: Switch to PDB (XEPDB1)**
```sql
ALTER SESSION SET CONTAINER = XEPDB1;

SHOW CON_NAME;
-- Output: XEPDB1
```

**STEP 4: Create user**
```sql
CREATE USER myuser IDENTIFIED BY devojyoti;
```

**STEP 5: Grant required privileges**
```sql
GRANT CONNECT, RESOURCE TO myuser;
-- Optional: GRANT CREATE TABLE TO myuser;
```

**STEP 6: Connect as the new user**
```shell
sqlplus myuser/devojyoti@localhost:1521/XEPDB1
```

**STEP 7: Verify connected user**
```sql
SHOW USER;
-- Output: USER is "MYUSER"
```
