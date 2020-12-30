# 1980’den İtibaren Herhangi Bir Spor Grubunda Üst Üste En Az 3 Olimpiyatta Madalya Almış Atletleri Veren Sorgu

```SQL

SELECT DISTINCT q1.Athlete
FROM ((`dsmbootcamp.zeki_oren.summer_medals` AS q1
INNER JOIN `dsmbootcamp.zeki_oren.summer_medals` AS q2 ON q1.Athlete = q2.Athlete)
INNER JOIN `dsmbootcamp.zeki_oren.summer_medals` AS q3  ON q2.Athlete = q3.Athlete)
WHERE q1.Year >=1980 AND q1.Year = q2.Year-4 AND q1.Year = q3.Year-8;

```
