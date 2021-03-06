---
title: PostgreSQL 满足条件时插入数据
date: 2017-11-23T22:26:00+08:00
draft: false
summary: ""
tags: ["PostgreSQL"]
---

例如：当表中不存在某记录时，才插入这条记录。

```sql
INSERT INTO 表名(列名1, 列名2)
SELECT
    '值1', '值2'
WHERE NOT EXISTS (
    SELECT * FROM 表名 WHERE 列名1 = '值1', 列名2 = '值2'
);
```

## 参考

1. [PostgreSQL: Documentation: 10: INSERT](https://www.postgresql.org/docs/current/static/sql-insert.html)

    > This example inserts some rows into table films from a table tmp_films with the same column layout as films:
    > 
    >     INSERT INTO films SELECT * FROM tmp_films WHERE date_prod < '2004-05-07';

2. [database - Conditional INSERT INTO statement in postgres - Stack Overflow](https://stackoverflow.com/questions/15710162/conditional-insert-into-statement-in-postgres)