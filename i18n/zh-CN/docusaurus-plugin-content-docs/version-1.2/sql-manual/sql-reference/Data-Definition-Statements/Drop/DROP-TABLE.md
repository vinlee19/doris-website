---
{
    "title": "DROP-TABLE",
    "language": "zh-CN"
}
---

## DROP-TABLE

### Name

DROP TABLE

## 描述

该语句用于删除 table 。
语法：

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name [FORCE];
```


说明：

- 执行 DROP TABLE 一段时间内，可以通过 RECOVER 语句恢复被删除的表。详见 [RECOVER](../../../../sql-manual/sql-reference/Database-Administration-Statements/RECOVER.md) 语句
- 如果执行 DROP TABLE FORCE，则系统不会检查该表是否存在未完成的事务，表将直接被删除并且不能被恢复，一般不建议执行此操作

## 举例

1. 删除一个 table
   
    ```sql
    DROP TABLE my_table;
    ```
    
2. 如果存在，删除指定 database 的 table
   
    ```sql
    DROP TABLE IF EXISTS example_db.my_table;
    ```
    

### Keywords

    DROP, TABLE

### Best Practice

