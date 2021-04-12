---
layout:     post
title:      "Firebase - BigQuery 留存查询"
subtitle:   ""
date:       2019-02-01
author:     "Will"
header-img: "img/google/firebase-bigquery-01.jpg"
catalog: true
tags:
    - Google
    - Firebase
    - BigQuery
    
---

> “walk beside you ”

## 前言

　　如果将梦想作为信仰，不放弃地追求下去，一定会梦想成真的。
                                ------  岸本齐史

---

## 正文

　　基于Firebase-BigQuery 的 统计留存语句

### SQL

　　```
    WITH
    data_info AS(
    SELECT
        event_name,
        user_pseudo_id,
        traffic_source.source,
        app_info.version,
        geo.country,
        device.operating_system,
        date.key date_type,
        date.value date_value
    FROM
        `table-id.events_*`
    CROSS JOIN
        UNNEST( [ STRUCT("-12:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-12:00") AS value), STRUCT("-11:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-11:00") AS value), STRUCT("-10:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-10:00") AS value), STRUCT("-09:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-09:00") AS value), STRUCT("-08:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-08:00") AS value), STRUCT("-07:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-07:00") AS value), STRUCT("-06:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-06:00") AS value), STRUCT("-05:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-05:00") AS value), STRUCT("-04:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-04:00") AS value), STRUCT("-03:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-03:00") AS value), STRUCT("-02:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-02:00") AS value), STRUCT("-01:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"-01:00") AS value), STRUCT("+00:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+00:00") AS value), STRUCT("+01:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+01:00") AS value), STRUCT("+02:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+02:00") AS value), STRUCT("+03:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+03:00") AS value), STRUCT("+04:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+04:00") AS value), STRUCT("+05:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+05:00") AS value), STRUCT("+06:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+06:00") AS value), STRUCT("+07:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+07:00") AS value), STRUCT("+08:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+08:00") AS value), STRUCT("+09:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+09:00") AS value), STRUCT("+10:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+10:00") AS value), STRUCT("+11:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+11:00") AS value), STRUCT("+12:00" AS key,
            FORMAT_TIMESTAMP("%E4Y%m%d",TIMESTAMP_MICROS(event_timestamp),"+12:00") AS value) ] ) AS date
    WHERE
        _TABLE_SUFFIX >= FORMAT_DATE("%E4Y%m%d", DATE_SUB(CURRENT_DATE(),INTERVAL 60 DAY))
        AND _TABLE_SUFFIX <= FORMAT_DATE("%E4Y%m%d", DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY))
        AND event_name IN ('user_engagement',
        'first_open') ),
    first_open AS(
    SELECT
        user_pseudo_id,
        source,
        version,
        country,
        operating_system,
        date_type,
        date_value first_even_date
    FROM
        data_info
    WHERE
        event_name = 'first_open' ),
    first_open_date AS(
    SELECT
        user_pseudo_id,
        source,
        version,
        country,
        operating_system,
        date_type,
        first_even_date,
        date
    FROM
        first_open
    CROSS JOIN
        UNNEST(GENERATE_ARRAY(0,DATE_DIFF(DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY),PARSE_DATE("%E4Y%m%d",
                first_even_date),DAY))) AS date ),
    first_open_count AS(
    SELECT
        date_type,
        first_even_date,
        source,
        version,
        country,
        operating_system,
        date,
        COUNT(user_pseudo_id) count_first_open
    FROM
        first_open_date
    GROUP BY
        1,
        2,
        3,
        4,
        5,
        6,
        7 ),
    user_engagement AS (
    SELECT
        DISTINCT user_pseudo_id,
        date_value,
        date_type
    FROM
        data_info ),
    user_engagement_diff AS(
    SELECT
        user_pseudo_id,
        date_type,
        source,
        version,
        country,
        operating_system,
        DATE_DIFF(PARSE_DATE("%E4Y%m%d",
            date_value),PARSE_DATE("%E4Y%m%d",
            first_even_date),DAY) date,
        first_even_date
    FROM
        first_open
    INNER JOIN
        user_engagement
    USING
        (user_pseudo_id,
        date_type) )
    SELECT
    date,
    first_even_date,
    date_type,
    source,
    version,
    country,
    operating_system,
    count_first_open,
    COUNT(user_pseudo_id) data
    FROM
    first_open_count
    LEFT JOIN
    user_engagement_diff
    USING
    (first_even_date,
        date_type,
        source,
        version,
        country,
        operating_system,
        date)
    GROUP BY
    1,
    2,
    3,
    4,
    5,
    6,
    7,
    8
    ```
