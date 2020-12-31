# sample.content_category ve sample.content_category_20201222_00_59 Tablolarının Karşılaştırılması ve Güncellenmesi

```SQL

MERGE zeki_oren.content_category tgt
USING zeki_oren.content_category_20201222_00_59 src
ON tgt.id = src.id
WHEN MATCHED THEN UPDATE SET tgt.cdc_date = src.cdc_date, tgt.category = src.category
WHEN NOT MATCHED BY TARGET THEN INSERT(category, id, is_deleted, cdc_date)
                                VALUES (src.category, src.id, src.is_deleted, src.cdc_date)
WHEN NOT MATCHED BY SOURCE THEN UPDATE SET tgt.is_deleted = true;



WITH LeftData AS
(
SELECT farm_fingerprint(to_json_string(a)) AS h
from `dsmbootcamp.zeki_oren.content_category_target` AS a
),

RightData AS 
(
SELECT farm_fingerprint(to_json_string(b)) AS h
FROM `dsmbootcamp.zeki_oren.content_category` AS b 
)

SELECT * 
FROM LeftData l 
FULL OUTER JOIN RightData r ON l.h = r.h 
WHERE l.h IS NULL OR r.h IS NULL;

```
