---
{
    "title": "MINUTES_ADD",
    "language": "zh-CN"
}
---

## minutes_add
## 描述
## 语法

`DATETIME MINUTES_ADD(DATETIME date, INT minutes)`

从日期时间或日期加上指定分钟数

参数 date 可以是 DATETIME 或者 DATE 类型，返回类型为 DATETIME。

## 举例

```
mysql> select minutes_add("2020-02-02", 1);
+---------------------------------------+
| minutes_add('2020-02-02 00:00:00', 1) |
+---------------------------------------+
| 2020-02-02 00:01:00                   |
+---------------------------------------+
```

### keywords

    MINUTES_ADD
