---
{
    "title": "LakeSoul Catalog",
    "language": "zh-CN"
}
---

:::tip 备注
这是一个实验功能。
:::

## 使用须知

1. 目前支持 LakeSoul 作为 Source 的读操作，支持 LakeSoul 的主键表、无主键表，支持 MOR 读取。
2. LakeSoul 的数据存储在 HDFS 上时，需要将 core-site.xml，hdfs-site.xml 和 hive-site.xml  放到 FE 和 BE 的 conf 目录下。优先读取 conf 目录下的 hadoop 配置文件，再读取环境变量 `HADOOP_CONF_DIR` 的相关配置文件。

## Doris 中创建 Catalog

LakeSoul Catalog 创建时需要指定 LakeSoul 元数据的连接信息。关于 LakeSoul 的环境搭建，请参考 [LakeSoul 文档](https://lakesoul-io.github.io/zh-Hans/docs/Getting%20Started/setup-local-env)。

### 创建 Catalog 示例

```sql
create catalog lakesoul properties (
'type'='lakesoul',
'lakesoul.pg.username'='lakesoul_test',
'lakesoul.pg.password'='lakesoul_test',
'lakesoul.pg.url'='jdbc:postgresql://127.0.0.1:5432/lakesoul_test?stringtype=unspecified'
);
```

## 支持的类型
| Doris Data Type           | Comment        |
| ------------------------- | -------------- |
| Boolean                   | 支持           |
| TinyInt                   | 支持           |
| SmallInt                  | 支持           |
| Int                       | 支持           |
| Float                     | 支持           |
| BigInt                    | 支持           |
| Double                    | 支持           |
| VarChar                   | 支持           |
| Char                      | 支持           |
| String                    | 支持           |
| Decimal(precision, scale) | 支持           |
| DateTime                  | 支持           |
| Date                      | 支持           |
| Array                     | 支持 Array 嵌套   |
| Map                       | 支持 Map 嵌套     |
| Struct                    | 支持 Struct 嵌套  |
