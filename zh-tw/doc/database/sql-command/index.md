# 抓某個區間的第一筆資料


```sql
SELECT * FROM
(SELECT DISTINCT rank() over(PARTITION BY a.username ORDER BY a.update_dt desc) rn, a.*
   FROM table_name a
  WHERE a.update_dt >  p_cur_timestamp
    AND a.update_dt <= p_cur_timestamp + C_PERIOD
 ) 
WHERE rn = 1
```

