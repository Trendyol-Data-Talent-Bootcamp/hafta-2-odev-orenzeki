# 1980’den İtibaren Spor Grubu Bazında En Çok Madalya Alan 1'inci, 3'üncü ve 5'inci Ülkeleri Veren Sorgu

```SQL

WITH soru1 AS
(
WITH inner_query AS
(
SELECT Sport, 
       Country, 
       COUNT(*) AS Medal_Count
FROM `dsmbootcamp.zeki_oren.summer_medals`
WHERE Year >= 1980 AND Country IS NOT NULL
GROUP BY Sport, Country
)
SELECT Sport, 
       Country, 
       Medal_Count, 
       row_number() OVER(PARTITION BY Sport ORDER BY MEDAL_COUNT DESC) AS Row_Num
FROM inner_query
ORDER BY Sport, Medal_Count DESC
)
SELECT DISTINCT Sport, 
      (SELECT Country FROM soru1 s1 WHERE s.Sport = s1.Sport AND s1.Row_Num = 1) AS First,
      (SELECT Country FROM soru1 s2 WHERE s.Sport = s2.Sport AND s2.Row_Num = 3) AS Second,
      (SELECT Country FROM soru1 s3 WHERE s.Sport = s3.Sport AND s3.Row_Num = 5) AS Third
FROM soru1 s
ORDER BY Sport;

```
