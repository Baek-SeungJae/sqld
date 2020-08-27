# SQLD

# 4. **SQL** 활용

## 1. 표준조인

### 1. SQL에서의 연산

> - 집합연산
>
> | UNION        | UNION                               | 합집합                                  |
> | ------------ | ----------------------------------- | --------------------------------------- |
> | INTERSECTION | INTERSECT                           | 교집합                                  |
> | DIFFERENCE   | MINUS (오라클)  EXCEPT (SQL Server) | 차집합                                  |
> | PRODUCT      | CROSS  JOIN                         | 곱집합 (생길 수  있는 모든 데이터 조합) |
>
> - 관계연산
>
> | SELECT  | WHERE절   | 조건에 맞는 행 조회                                          |
> | ------- | --------- | ------------------------------------------------------------ |
> | PROJECT | SELECT절  | 조건에 맞는 칼럼 조회                                        |
> | JOIN    | 여러 JOIN |                                                              |
> | DIVIDE  | 없음      | 공통요소를 추출하고 분모 릴레이션의 속성을 삭제한 후 중복된  행 제거 |

### 2. ANSI/ISO SQL의 조인 형태

> - INNER JOIN, NATURAL JOIN, CROSS JOIN, OUTER JOIN

### 3. NATURAL JOIN

> - 같은 이름을 가진 칼럼 전체에 대한 등가 조인
> - USING 조건절이나 ON 조건절 사용 불가
> - 같은 데이터 유형 칼럼만 조인 가능
> - 앨리어스나 테이블명 사용 불가

### 4. INNER JOIN

> - 행에 동일한 값이 있는 칼럼 조인
>
> - JOIN의 디폴트 옵션
>
> - USING 조건절이나 ON 조건절 필수
>
> - CROSS JOIN이나 OUTER JOIN과 동시 사용 불가
>
> - 두 테이블에 동일 이름 칼럼이 있을 경우 SELECT절에 앨리어스 필수
>
> - SQL>> SELECT 칼럼s FROM 테이블1 A, 테이블2 B WHERE A.칼럼=B.칼럼;
>
>   SQL>> SELECT 칼럼s FROM 테이블1 A INNER JOIN 테이블2 B ON A.칼럼=B.칼럼; (ANSI/ISO 표준)
>
> - USING 조건절
>
>   - 같은 이름을 가진 칼럼 중 등가 조인 대상 칼럼 선택
>   - SQL Server에서는 지원하지 않음
>   - 조건절에 앨리어스나 테이블명 불가
>
> - ON 조건절
>
>   - 다른 이름을 가진 칼럼 간 조인 가능 (앨리어스나 테이블명 필수)
>   - 괄호는 의무사항 아님
>
>   - SQL>> SELECT 칼럼s FROM 테이블1 A JOIN 테이블2 B ON (A.칼럼=B.칼럼);

### 5. CROSS JOIN

> - 가능한 모든 조합으로 조인
> - SQL>> SELECT 칼럼 FROM 테이블1, 테이블2; (조인 조건이 없을 때 발생 ↔ NATURAL JOIN은 명시해야 됨)

### 6. OUTER JOIN

> - 조인 조건에서 행에 동일한 값이 없는 칼럼 조인
>
> - USING 조건절이나 ON 조건절 필수
>
> - LEFT OUTER JOIN
>
>   - 좌측 테이블 데이터 조회 후 우측 테이블 조인 대상 데이터 조회
>
>   - SQL>> SELECT 칼럼s FROM 테이블1 A, 테이블2 B A.칼럼=B.칼럼(+);
>
>     SQL>> SELECT 칼럼s FROM 테이블1 A
>
>     ​			LEFT OUTER JOIN 테이블2 B
>
>     ​			ON (A.칼럼=B.칼럼);
>
> - RIGHT OUTER JOIN
>
>   - *오른쪽 결과가 더 긺*
>
> - FULL OUTER JOIN
>
>   - LEFT와 RIGHT OUTER JOIN 포함
>
>   - SQL>> SELECT 칼럼s FROM 테이블1 A          결과:
>
>     ​			FULL OUTER JOIN 테이블2 B
>
>     ​			ON (A.칼럼=B.칼럼);

## 2. 집합 연산자

### 1. 집합 연산자

> - 조인 없이 여러 테이블의 관련 데이터를 조회하는 연산자

### 2. UNION

> - 합집합, 칼럼 수와 데이터 타입이 모두 동일한 테이블 간 연산만 가능
> - SQL>> SELECT 칼럼명 FROM 테이블명 A WHERE 조건절 UNION SELECT 테이블명 WHERE 조건절;
> - UNION ALL
>   - 중복된 행도 전부 출력하는 합집합, 정렬 안함 (↔ UNION은 정렬을 유발함), 집합 연산자에 속함
>   - SQL>> SELECT 칼럼명 FROM 테이블명 A WHERE 조건절 UNION ALL SELECT 테이블명 WHERE 조건절;

### 3. INTERSECT

> - 교집합
> - SQL>> SELECT 칼럼명 FROM 테이블명 A WHERE 조건절MS INTERSECT SELECT 테이블명 WHERE 조건절;

### 4. MINUS, EXCEPT

> - 차집합
> - SQL>> SELECT 칼럼명 FROM 테이블명 A WHERE 조건절 MINUS SELECT 테이블명 WHERE 조건절;

## 3. 계층형 질의와 셀프 조인

### 1. 계층형 질의(Hierarchical Query)

> - 계층형 데이터를 조회하기 위해 사용함, Oracle에서 지원함
> - 계층형 데이터
>   - 엔터티를 순환관계 데이터 모델로 설계할 때 발생함
> - CONNECT BY
>   - 트리 형태의 구조로 쿼리 수행 (루트 노드부터 하위 노드의 쿼리를 실행함) *상사 이름과 사람 이름을 조인하여 상사 밑에 넣기*
> - START WITH
>   - 시작 조건 지정
> - CONNECT BY PRIOR
>   - 조인 조건 지정
>   - LEVEL : 검색 항목의 깊이, 최상위 계층의 레벨은 1
>   - CONNECT_BY_ROOT : 최상위 계층 값 표시
>   - CONNECT_BY_ISLEAF : 최하위 계층 값 표시
>   - SYS_CONNECT_BY_PATH : 계층 구조의 전개 경로 표시
> - CONNECT BY절의 루프 알고리즘 키워드
>   - NOCYCLE : 순환구조의 발생지점까지만 전개
>   - CONNECT_BY_ISCYCLE : 순환구조 발생지점 표시 (부모 노드와 자식 노드가 같을 때 1 아니면 0 출력)
> - LPAD
>   - 계층형 조회 결과를 명확히 하기 위해 사용 (LEVEL 값을 이용하여 결과 데이터 정렬)

### 2. SQL Server 계층형 질의

> - CTE(Common Table Expression)로 재귀 호출

### 3. 셀프 조인

> - 한 테이블 내에서 두 칼럼이 연관 관계가 있는 경우, 앨리어스 필수

