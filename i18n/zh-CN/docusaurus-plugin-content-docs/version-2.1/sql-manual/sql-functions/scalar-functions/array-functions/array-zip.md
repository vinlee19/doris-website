---
{
    "title": "ARRAY_ZIP",
    "language": "zh-CN"
}
---

## 描述

将所有数组合并成一个单一的数组。结果数组包含源数组中按参数列表顺序分组的相应元素。

## 语法

```sql
ARRAY_ZIP(<array>[, <array> ])
```

## 参数

| 参数 | 说明   |
|--|------|
| `<array>` | 输入数组 |

## 返回值

返回来自源数组的元素分组成结构体的数组。结构体中的数据类型与输入数组的类型相同，并按照传递数组的顺序排列。

## 举例

```sql
SELECT ARRAY_ZIP(['a', 'b', 'c'], [1, 2, 3]);
```

```text
+--------------------------------------------------------+
| array_zip(['a', 'b', 'c'], [1, 2, 3])                  |
+--------------------------------------------------------+
| [{"1":"a", "2":1}, {"1":"b", "2":2}, {"1":"c", "2":3}] |
+--------------------------------------------------------+
```
