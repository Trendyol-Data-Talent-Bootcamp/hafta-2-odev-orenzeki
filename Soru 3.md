# Günün Her Dakikası İçin Aktif Kullanıcı Sayısının Hesaplanması

- Önce bu query çalıştırılmalı. Burada dakika başı cihaz sayısı HyperLogLog kullanılarak hesaplanmaktadır.

```SQL

CREATE OR REPLACE TABLE zeki_oren.active_user_count_per_minute AS
SELECT TIMESTAMP_TRUNC(view_ts,MINUTE) AS minute,
       hll_count.init(deviceid, 24) AS device_count,
FROM zeki_oren.pageview
GROUP BY minute
ORDER BY minute;

```

- Ardından bu query çalıştırılmalı. Burada ise 5 dakikalık periyotta aktif kullanıcı sayılarına ulaşılmaktadır.

```SQL

WITH soru3 AS (
SELECT minute,
       ARRAY_AGG(device_count) OVER (ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) active_user_count
FROM zeki_oren.active_user_count_per_minute)

SELECT minute,(SELECT HLL_COUNT.MERGE(device_count)
               FROM UNNEST(active_user_count) device_count) AS final_count
FROM soru3
ORDER BY minute;

```
