---
{
    "title": "JSON_PARSE",
    "language": "zh-CN"
}
---

## json_parse
## 描述
## 语法

```sql
JSON json_parse(VARCHAR json_str)
JSON json_parse_error_to_null(VARCHAR json_str)
JSON json_parse_error_to_value(VARCHAR json_str, VARCHAR default_json_str)
```

将原始JSON字符串解析成JSON二进制格式。为了满足不同的异常数据处理需求，提供不同的json_parse系列函数，具体行为如下：
- json_str为NULL时，都返回NULL
- json_str为非法JSON字符串时
  - json_parse报错
  - json_parse_error_to_null返回NULL，
  - json_parse_error_to_value返回参数default_json_str指定的默认值

## 举例

1. 正常JSON字符串解析

```
mysql> SELECT json_parse('{"k1":"v31","k2":300}');
+--------------------------------------+
| json_parse('{"k1":"v31","k2":300}') |
+--------------------------------------+
| {"k1":"v31","k2":300}                |
+--------------------------------------+
1 row in set (0.01 sec)
```

2. 非法JSON字符串解析

```
mysql> SELECT json_parse('invalid json');
ERROR 1105 (HY000): errCode = 2, detailMessage = json parse error: Invalid document: document must be an object or an array for value: invalid json

mysql> SELECT json_parse_error_to_null('invalid json');
+-------------------------------------------+
| json_parse_error_to_null('invalid json') |
+-------------------------------------------+
| NULL                                      |
+-------------------------------------------+
1 row in set (0.01 sec)

mysql> SELECT json_parse_error_to_value('invalid json', '{}');
+--------------------------------------------------+
| json_parse_error_to_value('invalid json', '{}') |
+--------------------------------------------------+
| {}                                               |
+--------------------------------------------------+
1 row in set (0.00 sec)
```


### keywords
JSONB, JSON, json_parse, json_parse_error_to_null, json_parse_error_to_value
