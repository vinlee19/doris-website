---
{
    "title": "GET_JSON_INT",
    "language": "en"
}
---

## get_json_int
### Description
#### Syntax

`INT get_json_int(VARCHAR json_str, VARCHAR json_path)`


Parse and retrieve the integer content of the specified path in the JSON string.
Where json_path must start with the $symbol and use. as the path splitter. If the path contains..., double quotation marks can be used to surround it.
Use [] to denote array subscripts, starting at 0.
The content of path cannot contain ",[and].
If the json_string format is incorrect, or the json_path format is incorrect, or matches cannot be found, NULL is returned.

In addition, it is recommended to use the jsonb type and jsonb_extract_XXX function performs the same function.

Exception handling is as follows:
- if the field specified by json_path does not exist, return NULL
- if datatype of the field specified by json_path is not the same with type of json_extract_t, return t if it can be cast to t else NULL

### example

1. Get the value of key as "k1"

```
mysql> SELECT get_json_int('{"k1":1, "k2":"2"}', "$.k1");
+--------------------------------------------+
| get_json_int('{"k1":1, "k2":"2"}', '$.k1') |
+--------------------------------------------+
|                                          1 |
+--------------------------------------------+
```

2. Get the second element of the array whose key is "my. key"

```
mysql> SELECT get_json_int('{"k1":"v1", "my.key":[1, 2, 3]}', '$."my.key"[1]');
+------------------------------------------------------------------+
| get_json_int('{"k1":"v1", "my.key":[1, 2, 3]}', '$."my.key"[1]') |
+------------------------------------------------------------------+
|                                                                2 |
+------------------------------------------------------------------+
```

3. Get the first element in an array whose secondary path is k1. key - > K2
```
mysql> SELECT get_json_int('{"k1.key":{"k2":[1, 2]}}', '$."k1.key".k2[0]');
+--------------------------------------------------------------+
| get_json_int('{"k1.key":{"k2":[1, 2]}}', '$."k1.key".k2[0]') |
+--------------------------------------------------------------+
|                                                            1 |
+--------------------------------------------------------------+
```
### keywords
GET_JSON_INT,GET,JSON,INT
