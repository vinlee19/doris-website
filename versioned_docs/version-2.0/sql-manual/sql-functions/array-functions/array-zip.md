---
{
    "title": "ARRAY_ZIP",
    "language": "en"
}
---

## array_zip

array_zip

### description

Combines all all arrays into a single array. The resulting array contains the corresponding elements of the source arrays grouped into structs in the listed order of arguments.

#### Syntax

`Array<Struct<T1, T2,...>> array_zip(Array<T1>, Array<T2>, ...)`

#### Returned value

Array with elements from the source arrays grouped into tuples. Data types in the tuple are the same as types of the input arrays and in the same order as arrays are passed.

### example

```
mysql> select array_zip(['a', 'b', 'c'], [1, 2, 3]);
+-------------------------------------------------+
| array_zip(ARRAY('a', 'b', 'c'), ARRAY(1, 2, 3)) |
+-------------------------------------------------+
| [{'a', 1}, {'b', 2}, {'c', 3}]                  |
+-------------------------------------------------+
1 row in set (0.01 sec)
```

### keywords

ARRAY,ZIP,ARRAY_ZIP