# How to analyze size of table, index in Oracle

看 table 大小:
<code language="sql">
SELECT * FROM all_tables WHERE table_name LIKE 'AB%' (記得檢查last_analyzed_date)
</code>

重新分析 table 大小:
<code language="sql">
ANALYZE table TABLE_NAME estimate statisitics
</code>

看 index 相關資訊:(這裡不會有 block 資訊)
<code language="sql">
SELECT * FROM all_indexes WHERE table_name LIKE'AB%'
</code>

重新分析 index:
<code language="sql">
ANALYZE index INDEX_NAME validate structure;
</code>

看 index 大小: (這個一次只能看最近一次 analyze 過的 index)
<code language="sql">
SELECT * FROM index_stats =&gt;
</code> 

<!--more-->

1. Index要rebuild後才準, delete/update record 通常不會free index block
2. Block 的定義要請 DBA 查

