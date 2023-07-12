# Programmers SQL KIT

프로그래머스 sql 고득점 kit 문제풀이용 레포지토리입니다. 



![스크린샷 2023-05-07 오후 4 40 29](https://user-images.githubusercontent.com/121741140/236664521-013667c4-f489-416f-86a2-0c0e1ef1d7ed.png)
- 2023년 5월 7일 시작

## SELECT

### 3월에 태어난 여성 회원 목록 출력하기

```
SELECT
    member_id,
    member_name,
    gender,
    to_char(DATE_OF_BIRTH, 'YYYY-MM-DD')as date_of_birth
from
    member_profile
WHERE
    TO_CHAR(DATE_OF_BIRTH, 'MM') = '03' and
    tlno is not null AND GENDER = 'W'
order by
    member_id asc
 ```

### 모든 레코드 조회하기

```
SELECT *
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
### 12세 이하인 여자 환자 목록 출력하기

```
SELECT PT_NAME, PT_NO, GEND_CD, AGE , NVL(TLNO, 'NONE')
FROM PATIENT
WHERE AGE <= 12 AND GEND_CD = 'W'
ORDER BY AGE DESC, PT_NAME;
```

### 과일로 만든 아이스크림 고르기

```
SELECT F.FLAVOR 
FROM FIRST_HALF F JOIN ICECREAM_INFO I ON F.FLAVOR = I.FLAVOR
WHERE I.INGREDIENT_TYPE = 'fruit_based' AND
F.TOTAL_ORDER > 3000
ORDER BY F.TOTAL_ORDER DESC;
```

### 재구매가 일어난 상품과 회원 리스트 구하기
```
SELECT USER_ID, PRODUCT_ID
FROM (
    SELECT USER_ID, PRODUCT_ID, COUNT(*) C
      FROM ONLINE_SALE
      GROUP BY USER_ID , PRODUCT_ID) OS
WHERE C > 1
ORDER BY USER_ID, PRODUCT_ID DESC;
```

### 서울에 위치한 식당 목록 출력하기

```
SELECT RI.REST_ID,
       RI.REST_NAME,
       RI.FOOD_TYPE,
       RI.FAVORITES,
       RI.ADDRESS,
       ROUND(AVG(REVIEW_SCORE),2) AS AVG_REVIEW
FROM REST_INFO RI JOIN REST_REVIEW RR ON RI.REST_ID = RR.REST_ID
WHERE RI.ADDRESS LIKE '서울%'
GROUP BY RI.REST_ID, RI.REST_NAME, RI.FOOD_TYPE, RI.FAVORITES, RI.ADDRESS
ORDER BY AVG_REVIEW DESC, RI.FAVORITES DESC; 
```

### 조건에 부합하는 중고거래 댓글 조회하기

```
SELECT UGB.TITLE,
        UGB.BOARD_ID,
        UGR.REPLY_ID,
        UGR.WRITER_ID,
        UGR.CONTENTS,
        TO_CHAR(UGR.CREATED_DATE,'YYYY-MM-DD') AS C_DATE
FROM USED_GOODS_BOARD UGB JOIN USED_GOODS_REPLY UGR ON UGB.BOARD_ID = UGR.BOARD_ID
WHERE TO_CHAR(UGB.CREATED_DATE, 'YYYY-MM') = '2022-10'
ORDER BY C_DATE, UGB.TITLE; // 일반 조인은 메인이 되는 왼쪽 테이블 기준으로 조인된다
```

### 강원도에 위치한 생산공장 목록 출력하기

```
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
FROM FOOD_FACTORY
WHERE ADDRESS LIKE '강원도%'
```

### 인기있는 아이스크림

```
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC , SHIPMENT_ID
```

### 흉부외과 또는 일반외과 의사 목록 출력하기

```
SELECT DR_NAME, DR_ID, MCDP_CD, TO_CHAR(HIRE_YMD, 'YYYY-MM-DD') D
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS') 
ORDER BY D DESC, DR_NAME
```

### 조건에 맞는 도서 리스트 출력하기

- MYSQL
```
SELECT BOOK_ID, DATE_FORMAT(PUBLISHED_DATE,'%Y-%m-%d')
FROM BOOK
WHERE YEAR(PUBLISHED_DATE) = 2021 AND CATEGORY = '인문'
ORDER BY PUBLISHED_DATE
```

- ORACLE

```
SELECT BOOK_ID, TO_CHAR(PUBLISHED_DATE,'YYYY-MM-DD')
FROM BOOK
WHERE TO_CHAR(PUBLISHED_DATE,'YYYY') = '2021' AND CATEGORY = '인문'
ORDER BY PUBLISHED_DATE
```

### 역순 정렬하기
```
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC
```

### 아픈 동물 찾기

```
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'SICK'
```

### 어린 동물 찾기

```
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION <> 'AGED'
ORDER BY ANIMAL_ID
```

### 동물의 아이디와 이름
```
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

### 여러 기준으로 정렬하기
```
SELECT ANIMAL_ID,NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME, DATETIME DESC
```

### 상위 n개 레코드
```
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
```

### 조건에 맞는 회원수 구하기

```
SELECT COUNT(*)
FROM USER_INFO
WHERE YEAR(JOINED) = 2021 AND AGE BETWEEN 20 AND 29
```

### 오프라인/온라인 판매 데이터 통합하기

```
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, 
        PRODUCT_ID , 
        USER_ID, 
        SALES_AMOUNT
FROM ONLINE_SALE
WHERE YEAR(SALES_DATE) = 2022 AND MONTH(SALES_DATE) = 3

UNION ALL

SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, 
        PRODUCT_ID ,
        NULL AS USER_ID,
        SALES_AMOUNT
FROM OFFLINE_SALE
WHERE YEAR(SALES_DATE) = 2022 AND MONTH(SALES_DATE) = 3

ORDER BY SALES_DATE, PRODUCT_ID, USER_ID
```

## Join


### 조건에 맞는 도서와 저자 리스트 출력하기

```
SELECT B.BOOK_ID, A.AUTHOR_NAME, TO_CHAR(B.PUBLISHED_DATE, 'YYYY-MM-DD') PD
FROM BOOK B JOIN AUTHOR A ON B.AUTHOR_ID = A.AUTHOR_ID
WHERE CATEGORY = '경제'
ORDER BY PD;
```

### 상품 별 오프라인 매출 구하기

```
SELECT P.PRODUCT_CODE, SUM(P.PRICE * OS.SALES_AMOUNT) AS SALES
FROM PRODUCT P JOIN OFFLINE_SALE OS ON P.PRODUCT_ID = OS.PRODUCT_ID
GROUP BY P.PRODUCT_CODE
ORDER BY SALES DESC, PRODUCT_CODE
```

### 있었는데요 없었습니다

```
SELECT AI.ANIMAL_ID, AI.NAME 
FROM ANIMAL_INS AI JOIN ANIMAL_OUTS AO ON AI.ANIMAL_ID = AO.ANIMAL_ID
WHERE AI.DATETIME > AO.DATETIME
ORDER BY AI.DATETIME 
```

### 없어진 기록 찾기

- 내 답안

```
SELECT ANIMAL_ID, NAME
FROM ANIMAL_OUTS A
WHERE NOT EXISTS (SELECT ANIMAL_ID FROM ANIMAL_INS B WHERE A.ANIMAL_ID = B.ANIMAL_ID)
ORDER BY ANIMAL_ID
```
- 다른 답안
```
SELECT B.ANIMAL_ID, B.NAME
FROM ANIMAL_INS A RIGHT JOIN ANIMAL_OUTS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.ANIMAL_ID IS NULL
ORDER BY B.ANIMAL_ID
```

### 오랜 기간 보호한 동물(1) ### 

- 내 답안
```
SELECT NAME, DATETIME
FROM (
    SELECT ROWNUM, AI.NAME, AI.DATETIME
    FROM ANIMAL_INS AI LEFT OUTER JOIN ANIMAL_OUTS AO
                        ON AI.ANIMAL_ID = AO.ANIMAL_ID
    WHERE AO.DATETIME IS NULL
    ORDER BY AI.DATETIME
)
WHERE ROWNUM < 4
```

### 5월 식품들의 총매출 조회하기 ###
```
SELECT FP.PRODUCT_ID, FP.PRODUCT_NAME,
        SUM(FO.AMOUNT * FP.PRICE) TOTAL_SALES
FROM FOOD_PRODUCT FP JOIN FOOD_ORDER FO
                            ON FP.PRODUCT_ID = FO.PRODUCT_ID
WHERE TO_CHAR(PRODUCE_DATE,'YYYY-MM') = '2022-05'
GROUP BY FP.PRODUCT_ID, FP.PRODUCT_NAME
ORDER BY TOTAL_SALES DESC, FP.PRODUCT_ID
```

### 주문량이 많은 아이스크림들 조회하기

-MYSQL
```
SELECT F.FLAVOR
FROM FIRST_HALF F JOIN JULY J ON F.FLAVOR = J.FLAVOR
GROUP BY F.FLAVOR
ORDER BY SUM(F.TOTAL_ORDER) + SUM(J.TOTAL_ORDER) DESC
LIMIT 3;
```
-Oracle
```
SELECT C.FLAVOR FROM(
SELECT S.*, ROWNUM
FROM (
        SELECT J.FLAVOR, SUM(NVL(F.TOTAL_ORDER,0) + J.TOTAL_ORDER) SM
        FROM FIRST_HALF F 
        RIGHT OUTER JOIN JULY J ON F.SHIPMENT_ID = J.SHIPMENT_ID 
                                                AND F.FLAVOR = J.FLAVOR
        GROUP BY J.FLAVOR
) S
    ORDER BY S.SM DESC
) C
WHERE ROWNUM <= 3
```

### 평균 일일 대여 요금 구하기

```
SELECT ROUND(AVG(DAILY_FEE)) AS AVERAGE_FEE
FROM CAR_RENTAL_COMPANY_CAR
WHERE CAR_TYPE = 'SUV'
```

### 보호소에서 중성화한 동물

```
SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME
FROM ANIMAL_OUTS A JOIN ANIMAL_INS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.SEX_UPON_OUTCOME <> B.SEX_UPON_INTAKE 
ORDER BY A.ANIMAL_ID
```

### 그룹별 조건에 맞는 식당 목록 출력하기

```
SELECT A.MEMBER_NAME, B.REVIEW_TEXT, DATE_FORMAT(REVIEW_DATE, '%Y-%m-%d') AS REVIEW_DATE
FROM MEMBER_PROFILE AS A JOIN REST_REVIEW AS B
                            ON A.MEMBER_ID = B.MEMBER_ID
WHERE A.MEMBER_ID = (
                SELECT MEMBER_ID
                FROM REST_REVIEW
                GROUP BY MEMBER_ID
                ORDER BY COUNT(REVIEW_ID) DESC 
                LIMIT 1
)
ORDER BY REVIEW_DATE, REVIEW_TEXT
```

### 상품을 구매한 회원 비율 구하기

```
# SET @2021_DATE := (
#     SELECT COUNT(*)
#     FROM USER_INFO
#     WHERE YEAR(JOINED) = 2021
# );

# SELECT YEAR(SALES_DATE) AS YEAR,
#         MONTH(SALES_DATE) AS MONTH,
#         COUNT(DISTINCT B.USER_ID) AS PURCHASED_USERS,
#         ROUND(COUNT(DISTINCT B.USER_ID) / @2021_DATE , 1) AS PURCHASED_RATIO
# FROM USER_INFO A JOIN ONLINE_SALE B ON A.USER_ID = B.USER_ID
# WHERE YEAR(JOINED) = 2021
# GROUP BY YEAR, MONTH
# ORDER BY YEAR, MONTH

# 다른 방법

SELECT YEAR(SALES_DATE) AS YEAR,
        MONTH(SALES_DATE) AS MONTH,
        COUNT(DISTINCT B.USER_ID) AS PURCHASED_USERS,
        ROUND(COUNT(DISTINCT B.USER_ID) / 
                    (
                    SELECT COUNT(*)
                    FROM USER_INFO
                    WHERE YEAR(JOINED) = 2021), 1)
FROM USER_INFO A JOIN ONLINE_SALE B ON A.USER_ID = B.USER_ID
WHERE YEAR(JOINED) = 2021
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH
```


## Group by


### 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기

```
SELECT CAR_ID, 
       max(
           case when '2022-10-16' 
           between TO_CHAR(START_DATE, 'YYYY-MM-DD') and TO_CHAR(END_DATE, 'YYYY-MM-DD' )
           then '대여중' 
           else '대여 가능' end) as AVAILABILITY
FROM  CAR_RENTAL_COMPANY_RENTAL_HISTORY 
group by car_id
ORDER BY car_id desc // 다시 
```

### 저자 별 카테고리 별 매출액 집계하기

- ORACLE
```
SELECT B.AUTHOR_ID, A.AUTHOR_NAME, B.CATEGORY, SUM(B.PRICE * BS.SALES) TOTAL_SALES
FROM BOOK B JOIN (SELECT * 
                 FROM BOOK_SALES BS
                  WHERE TO_CHAR(BS.SALES_DATE, 'YYYY-MM') = '2022-01'
                 ) BS ON B.BOOK_ID = BS.BOOK_ID JOIN AUTHOR A 
                                                ON A.AUTHOR_ID = B.AUTHOR_ID
GROUP BY B.AUTHOR_ID, A.AUTHOR_NAME, B.CATEGORY
ORDER BY B.AUTHOR_ID, B.CATEGORY DESC
```
- MYSQL

```
SELECT B.AUTHOR_ID, A.AUTHOR_NAME, B.CATEGORY, SUM(B.PRICE * BS.SALES) TOTAL_SALES
FROM BOOK B JOIN (SELECT * 
                 FROM BOOK_SALES BS
                  WHERE YEAR(BS.SALES_DATE) = 2022 AND MONTH(BS.SALES_DATE) = 01
                 ) BS ON B.BOOK_ID = BS.BOOK_ID JOIN AUTHOR A 
                                                ON A.AUTHOR_ID = B.AUTHOR_ID
GROUP BY B.AUTHOR_ID, A.AUTHOR_NAME, B.CATEGORY
ORDER BY B.AUTHOR_ID, B.CATEGORY DESC
```

### 성분으로 구분한 아이스크림 총 주문량

```
SELECT B.INGREDIENT_TYPE, SUM(A.TOTAL_ORDER) AS TOTAL_ORDER
FROM FIRST_HALF A JOIN ICECREAM_INFO B ON A.FLAVOR = B.FLAVOR
GROUP BY B.INGREDIENT_TYPE
ORDER BY TOTAL_ORDER
```

### 진료과별 총 예약 횟수 출력하기
```
SELECT MCDP_CD 진료과코드, COUNT(*) 5월예약건수
FROM APPOINTMENT
WHERE YEAR(APNT_YMD) = 2022 AND MONTH(APNT_YMD) = 05
GROUP BY 진료과코드
order by 5월예약건수, 진료과코드
```

### 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기

- MYSQL
```
SELECT CAR_TYPE, COUNT(*) CARS
FROM CAR_RENTAL_COMPANY_CAR 
WHERE OPTIONS LIKE '%통풍시트%'
      OR  OPTIONS LIKE '%열선시트%'
      OR  OPTIONS LIKE '%가죽시트%'
GROUP BY CAR_TYPE
ORDER BY CAR_TYPE
```

### 즐겨찾기가 가장 많은 식당 정보 출력하기

```
SELECT R2.FOOD_TYPE, R2.REST_ID, R2.REST_NAME, R2.FAVORITES
FROM (
    SELECT MAX(FAVORITES) FAVORITES, FOOD_TYPE
    FROM REST_INFO
    GROUP BY FOOD_TYPE
) R1 JOIN REST_INFO R2 ON R1.FOOD_TYPE = R2.FOOD_TYPE AND R1.FAVORITES = R2.FAVORITES
GROUP BY R2.FOOD_TYPE
ORDER BY R2.FOOD_TYPE DESC
```

### 조건에 맞는 사용자와 총 거래금액 조회하기

```
SELECT CB.USER_ID, CB.NICKNAME, CB.TOTAL_SALES
FROM (
    SELECT A.USER_ID, SUM(B.PRICE) AS TOTAL_SALES, A.NICKNAME, B.STATUS
    FROM USED_GOODS_USER A JOIN USED_GOODS_BOARD B ON A.USER_ID = B.WRITER_ID
    WHERE B.STATUS = 'DONE'
    GROUP BY A.USER_ID
) AS CB
WHERE CB.TOTAL_SALES >= 700000
ORDER BY CB.TOTAL_SALES
```

### 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기

- MYSQL
```
SELECT MONTH(START_DATE) MONTH, CAR_ID, COUNT(*) RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE START_DATE BETWEEN '2022-08-01' AND '2022-10-31'
AND CAR_ID IN (
    SELECT CAR_ID
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    WHERE START_DATE BETWEEN '2022-08-01' AND '2022-10-31'
    GROUP BY CAR_ID
    HAVING COUNT(CAR_ID) >= 5)
GROUP BY MONTH, CAR_ID
HAVING RECORDS > 0
ORDER BY MONTH, CAR_ID DESC
```

-ORACLE
```
SELECT EXTRACT(MONTH FROM C.START_DATE) AS MONTH, CAR_ID, COUNT(*) RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY C
WHERE START_DATE BETWEEN TO_DATE('2022-08-01','YYYY-MM-DD') AND TO_DATE('2022-10-31', 'YYYY-MM-DD')
AND CAR_ID IN (
    SELECT CAR_ID
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    WHERE START_DATE BETWEEN TO_DATE('2022-08-01','YYYY-MM-DD') AND TO_DATE('2022-10-31', 'YYYY-MM-DD')
    GROUP BY CAR_ID
    HAVING COUNT(*) >= 5)
GROUP BY EXTRACT(MONTH FROM C.START_DATE), CAR_ID
HAVING COUNT(*) > 0
ORDER BY EXTRACT(MONTH FROM C.START_DATE), CAR_ID DESC
```

### 고양이와 개는 몇 마리 있을까

```
SELECT ANIMAL_TYPE, COUNT(*)
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'CAT'
UNION ALL
SELECT ANIMAL_TYPE, COUNT(*)
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'DOG'
```

### 동명 동물 수 찾기
```
SELECT NAME, COUNT(NAME)
FROM ANIMAL_INS
WHERE NAME <> ''
GROUP BY NAME
HAVING COUNT(NAME) >= 2
ORDER BY NAME
```

### 입양 시각 구하기(2)

-MYSQL

```
SET @HOUR = -1;

SELECT
    (@HOUR := @HOUR + 1) AS HOUR, 
    (SELECT COUNT(*) FROM ANIMAL_OUTS WHERE @HOUR = HOUR(DATETIME)) AS COUNT
FROM ANIMAL_OUTS
WHERE @HOUR < 23

```

### 카테고리 별 도서 판매량 집계하기

- MYSQL

```
SELECT B.CATEGORY, SUM(SALES) AS TOTAL_SALES
FROM BOOK_SALES A JOIN BOOK B ON A.BOOK_ID = B.BOOK_ID
WHERE YEAR(SALES_DATE) = 2022 AND MONTH(SALES_DATE) = 1
GROUP BY B.CATEGORY
ORDER BY CATEGOR
```

- ORACLE

```
SELECT A.CATEGORY, SUM(SALES)
FROM BOOK A JOIN BOOK_SALES B ON A.BOOK_ID = B.BOOK_ID
WHERE TO_CHAR(B.SALES_DATE, 'YYYY-MM') = '2022-01'
GROUP BY A.CATEGORY
ORDER BY A.CATEGORY
```

### 가격대 별 상품 개수 구하기

- MYSQL
```
SELECT 
    CASE
    WHEN PRICE >= 10000 AND PRICE < 20000 THEN 10000
    WHEN PRICE >= 20000 AND PRICE < 30000 THEN 20000
    WHEN PRICE >= 30000 AND PRICE < 40000 THEN 30000
    WHEN PRICE >= 40000 AND PRICE < 50000 THEN 40000
    WHEN PRICE >= 50000 AND PRICE < 60000 THEN 50000
    WHEN PRICE >= 60000 AND PRICE < 70000 THEN 60000
    WHEN PRICE >= 70000 AND PRICE < 80000 THEN 70000
    WHEN PRICE >= 80000 AND PRICE < 90000 THEN 80000
    WHEN PRICE >= 90000 AND PRICE < 100000 THEN 90000
    ELSE 0
    END AS PRICE_GROUP, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY 1
ORDER BY 1
```

-MYSQL

```
SELECT TRUNC(PRICE, -4) AS PRICE_GROUP, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY TRUNC(PRICE, -4)
ORDER BY 1
```

### 년, 월, 성별 별 상품 구매 회원 수 구하기

-MYSQL
```
SELECT YEAR(B.SALES_DATE) AS YEAR , MONTH(B.SALES_DATE) AS MONTH
                                , A.GENDER, COUNT(DISTINCT A.USER_ID) AS USERS
FROM USER_INFO A JOIN ONLINE_SALE B ON A.USER_ID = B.USER_ID
WHERE GENDER IS NOT NULL
GROUP BY YEAR, MONTH, GENDER
ORDER BY YEAR, MONTH, GENDER
```
-ORACLE
```
SELECT EXTRACT(YEAR FROM B.SALES_DATE) AS YEAR,
        EXTRACT(MONTH FROM B.SALES_DATE) AS MONTH,
        A.GENDER, 
        COUNT(DISTINCT A.USER_ID) AS USERS
FROM USER_INFO A JOIN ONLINE_SALE B ON A.USER_ID = B.USER_ID
WHERE GENDER IS NOT NULL
GROUP BY EXTRACT(YEAR FROM B.SALES_DATE),
        EXTRACT(MONTH FROM B.SALES_DATE),
        A.GENDER
ORDER BY YEAR, MONTH, GENDER
```

### 식품분류별 가장 비싼 식품의 정보 조회하기
```
SELECT CATEGORY, PRICE MAX_PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE PRICE IN
    (
    SELECT MAX(PRICE) MAX_PRICE
    FROM FOOD_PRODUCT
    GROUP BY CATEGORY
    )
AND CATEGORY IN ('과자', '국', '김치', '식용유')
ORDER BY 2 DESC
```

## SUM,MAX,MIN

### 가격이 제일 비싼 식품의 정보 출력하기

```
SELECT PRODUCT_ID, PRODUCT_NAME, PRODUCT_CD, CATEGORY, PRICE
FROM FOOD_PRODUCT
WHERE PRICE = (SELECT MAX(PRICE) FROM FOOD_PRODUCT)
```

### 최솟값 구하기

-ORACLE
```
SELECT DATETIME AS 시간
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MIN(DATETIME) FROM ANIMAL_INS)
```

### 동물 수 구하기

```
SELECT COUNT(*)
FROM ANIMAL_INS
```

### 중복 제거하기
```
SELECT COUNT(DISTINCT NAME)
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
```

### 입양 시각 구하기(1)

-MYSQL

```
SELECT HOUR(DATETIME), COUNT(*)
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) BETWEEN 9 AND 19
GROUP BY HOUR(DATETIME)
ORDER BY HOUR(DATETIME)
```

- ORACLE

```
SELECT TO_NUMBER(TO_CHAR(DATETIME, 'HH24')) , COUNT(*)
FROM ANIMAL_OUTS
WHERE TO_NUMBER(TO_CHAR(DATETIME, 'HH24')) BETWEEN 09 AND 19
GROUP BY TO_NUMBER(TO_CHAR(DATETIME, 'HH24'))
ORDER BY TO_NUMBER(TO_CHAR(DATETIME, 'HH24'))
```

## IS NULL

### NULL 처리하기

```
SELECT ANIMAL_TYPE,
        CASE
            WHEN NAME IS NULL THEN 'No name'
            ELSE NAME
        END AS NAME,
        SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

### 가장 비싼 상품 구하기

```
SELECT MAX(PRICE)
FROM PRODUCT
```

### 최댓값 구하기

```
SELECT MAX(DATETIME) AS 시간
FROM ANIMAL_INS
```

### 경기도에 위치한 식품창고 목록 출력하기

-MYSQL

```
SELECT WAREHOUSE_ID,
        WAREHOUSE_NAME,
        ADDRESS, 
        IFNULL(FREEZER_YN, 'N') AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '%경기도%'
```

-ORACLE

```
SELECT WAREHOUSE_ID,
        WAREHOUSE_NAME,
        ADDRESS, 
        NVL(FREEZER_YN, 'N') AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '%경기도%'
```
