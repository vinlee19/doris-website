---
{
    "title": "DESCRIBE",
    "language": "zh-CN"
}
---

## DESCRIBE

### Name

DESCRIBE

## 描述

该语句用于展示指定 table 的 schema 信息

语法：

```sql
DESC[RIBE] [db_name.]table_name [ALL];
```

说明：

1. 如果指定 ALL，则显示该 table 的所有 index(rollup) 的 schema

## 举例

1. 显示Base表Schema

    ```sql
    DESC table_name;
    ```

2. 显示表所有 index 的 schema

    ```sql
    DESC db1.table_name ALL;
    ```

### Keywords

    DESCRIBE, DESC

### Best Practice

