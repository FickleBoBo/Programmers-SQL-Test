# [조건에 맞는 사원 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/284527)

## 문제 설명

`HR_DEPARTMENT` 테이블은 회사의 부서 정보를 담은 테이블입니다. `HR_DEPARTMENT` 테이블의 구조는 다음과 같으며 `DEPT_ID`, `DEPT_NAME_KR`, `DEPT_NAME_EN`, `LOCATION`은 각각 부서 ID, 국문 부서명, 영문 부서명, 부서 위치를 의미합니다.

| Column name  | Type    | Nullable |
| ------------ | ------- | -------- |
| DEPT_ID      | VARCHAR | FALSE    |
| DEPT_NAME_KR | VARCHAR | FALSE    |
| DEPT_NAME_EN | VARCHAR | FALSE    |
| LOCATION     | VARCHAR | FALSE    |

`HR_EMPLOYEES` 테이블은 회사의 사원 정보를 담은 테이블입니다. `HR_EMPLOYEES` 테이블의 구조는 다음과 같으며 `EMP_NO`, `EMP_NAME`, `DEPT_ID`, `POSITION`, `EMAIL`, `COMP_TEL`, `HIRE_DATE`, `SAL`은 각각 사번, 성명, 부서 ID, 직책, 이메일, 전화번호, 입사일, 연봉을 의미합니다.

| Column name | Type    | Nullable |
| ----------- | ------- | -------- |
| EMP_NO      | VARCHAR | FALSE    |
| EMP_NAME    | VARCHAR | FALSE    |
| DEPT_ID     | VARCHAR | FALSE    |
| POSITION    | VARCHAR | FALSE    |
| EMAIL       | VARCHAR | FALSE    |
| COMP_TEL    | VARCHAR | FALSE    |
| HIRE_DATE   | DATE    | FALSE    |
| SAL         | NUMBER  | FALSE    |

`HR_GRADE` 테이블은 2022년 사원의 평가 정보를 담은 테이블입니다. `HR_GRADE`의 구조는 다음과 같으며 `EMP_NO`, `YEAR`, `HALF_YEAR`, `SCORE`는 각각 사번, 연도, 반기, 평가 점수를 의미합니다.

| Column name | Type    | Nullable |
| ----------- | ------- | -------- |
| EMP_NO      | VARCHAR | FALSE    |
| YEAR        | NUMBER  | FALSE    |
| HALF_YEAR   | NUMBER  | FALSE    |
| SCORE       | NUMBER  | FALSE    |

## 문제

`HR_DEPARTMENT`, `HR_EMPLOYEES`, `HR_GRADE` 테이블에서 2022년도 한해 평가 점수가 가장 높은 사원 정보를 조회하려 합니다. 2022년도 평가 점수가 가장 높은 사원들의 점수, 사번, 성명, 직책, 이메일을 조회하는 SQL문을 작성해주세요.

2022년도의 평가 점수는 상,하반기 점수의 합을 의미하고, 평가 점수를 나타내는 컬럼의 이름은 `SCORE`로 해주세요.

## 예시

예를 들어 `HR_DEPARTMENT` 테이블이 다음과 같고

| DEPT_ID | DEPT_NAME_KR | DEPT_NAME_EN    | LOCATION     |
| ------- | ------------ | --------------- | ------------ |
| D0001   | 법무팀       | Law Dep         | 그렙타워 4층 |
| D0002   | 인사팀       | Human resources | 그렙타워 4층 |
| D0003   | 총무팀       | General Affairs | 그렙타워 4층 |

`HR_EMPLOYEES` 테이블이 다음과 같고

| EMP_NO  | EMP_NAME | DEPT_ID | POSITION | EMAIL                  | COMP_TEL      | HIRE_DATE  | SAL      |
| ------- | -------- | ------- | -------- | ---------------------- | ------------- | ---------- | -------- |
| 2017002 | 정호식   | D0001   | 팀장     | hosick_jung@grepp.com  | 031-8000-1101 | 2017-03-01 | 65000000 |
| 2018001 | 김민석   | D0001   | 팀원     | minseock_kim@grepp.com | 031-8000-1102 | 2018-03-01 | 60000000 |
| 2019001 | 김솜이   | D0002   | 팀장     | somi_kim@grepp.com     | 031-8000-1106 | 2019-03-01 | 60000000 |
| 2020002 | 김연주   | D0002   | 팀원     | yeonjoo_kim@grepp.com  | 031-8000-1107 | 2020-03-01 | 53000000 |
| 2020005 | 양성태   | D0003   | 팀원     | sungtae_yang@grepp.com | 031-8000-1112 | 2020-03-01 | 53000000 |

`HR_GRADE` 테이블이 다음과 같을 때

| EMP_NO  | YEAR | HALF_YEAR | SCORE |
| ------- | ---- | --------- | ----- |
| 2017002 | 2022 | 1         | 92    |
| 2018001 | 2022 | 1         | 89    |
| 2019001 | 2022 | 1         | 94    |
| 2020002 | 2022 | 1         | 90    |
| 2020005 | 2022 | 1         | 92    |
| 2017002 | 2022 | 2         | 84    |
| 2018001 | 2022 | 2         | 89    |
| 2019001 | 2022 | 2         | 81    |
| 2020002 | 2022 | 2         | 91    |
| 2020005 | 2022 | 2         | 81    |

다음과 같이 평가 점수가 가장 높은 사원 정보를 출력해야 합니다.

| SCORE | EMP_NO | EMP_NAME | POSITION | EMAIL                 |
| ----- | ------ | -------- | -------- | --------------------- |
| 181   | 202002 | 김연주   | 팀원     | yeonjoo_kim@grepp.com |

---

## 문제 정보

| 난이도 | Lv. 2 |
| ------ | ----- |
| 정답률 | 80%   |

---

## 풀이

```SQL
SELECT SUM(HG.SCORE) AS SCORE, HE.EMP_NO, HE.EMP_NAME, HE.POSITION, HE.EMAIL
FROM HR_EMPLOYEES AS HE
JOIN HR_GRADE AS HG
    ON HE.EMP_NO = HG.EMP_NO
GROUP BY HE.EMP_NO
ORDER BY SCORE DESC
LIMIT 1;
```

---

## 결과

| 채점 결과        |
| ---------------- |
| 테스트 1 〉 통과 |
| 테스트 2 〉 통과 |
| 테스트 3 〉 통과 |
| 테스트 4 〉 통과 |
| 테스트 5 〉 통과 |
| 테스트 6 〉 통과 |

---
