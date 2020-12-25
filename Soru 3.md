# Günün Her Dakikası İçin Aktif Kullanıcı Sayısının Hesaplanması

```SQL

CREATE OR REPLACE TABLE zeki_oren.active_user_count_per_minute AS
SELECT TIMESTAMP_TRUNC(view_ts,MINUTE) AS minute,
       hll_count.init(deviceid, 24) AS device_count,
FROM zeki_oren.pageview
GROUP BY minute
ORDER BY minute;
  
  
SELECT minute,
       SUM(count_apprx) OVER (ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) AS active_user_count
FROM ( SELECT minute, hll_count.EXTRACT(device_count) AS count_apprx
       FROM zeki_oren.active_user_count_per_minute )
ORDER BY minute;

```
