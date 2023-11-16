# 数据跳过索引（Data Skipping Index）

## Background

Clickhouse 为列数据库，无法使用 [[B+ Tree]] 创建多级索引。

## Grammar

- index name
- index expression：计算存储在索引中的值集。它可以是列、简单运算符和/或由索引类型确定的函数子集的组合
- TYPE：是否可以跳过读取和评估每个索引块的计算
- GRANULARITY：索引块包含的颗粒数

```sql
ALTER TABLE skip_table ADD INDEX vix my_value TYPE set(100) GRANULARITY 2;
-- index name: vix
-- index expression: my_value
```

通常情况下，跳过索引仅适用于新插入的数据，因此仅添加索引不会影响上述查询。

要索引现有数据：

```sql
ALTER TABLE skip_table MATERIALIZE INDEX vix;
```


## TYPE

### minmax

Stores the minimum and maximum values of the index expression for each block. This type is ideal for columns that tend to be loosely sorted by value. This index type is usually the least expensive to apply during query processing.

In contrast, minmax indexes work particularly well with ranges since determining whether ranges intersect is very fast.

### set

This lightweight index type accepts a single parameter of the  max_size of the value set per block(0 permits an unlimited number of discrete values). This set contains all values in the block (or is empty if the number of values exceeds the max_size). This index type works well with columns with low cardinality within each set of granules (essentially, "clumped together") but higher cardinality overall.

This index's cost, performance, and effectiveness depend on the cardinality within blocks. If each block contains a large number of unique values, either evaluating the query condition against a large index set will be very expensive, or the index will not be applied because the index is empty due to exceeding max_size.

### [[布隆过滤器| Bloom Filter]] Types

Because Bloom filters can more efficiently handle testing for a large number of discrete values, they can be appropriate for conditional expressions that produce more values to test.

- The basic **bloom_filter** which takes a single optional parameter of the allowed "false positive" rate between 0 and 1 (if unspecified, .025 is used).
- The specialized **tokenbf_v1**. It takes three parameters, all related to tuning the bloom filter used: (1) the size of the filter in bytes (larger filters have fewer false positives, at some cost in storage), (2) number of hash functions applied (again, more hash filters reduce false positives), and (3) the seed for the bloom filter hash functions. See the calculator [here](https://hur.st/bloomfilter/) for more detail on how these parameters affect bloom filter functionality. This index works only with String, FixedString, and Map datatypes. The input expression is split into character sequences separated by non-alphanumeric characters. For example, a column value of `This is a candidate for a "full text" search` will contain the tokens `This` `is` `a` `candidate` `for` `full` `text` `search`. It is intended for use in LIKE, EQUALS, IN, hasToken() and similar searches for words and other values within longer strings. For example, one possible use might be searching for a small number of class names or line numbers in a column of free form application log lines.
- The specialized **ngrambf_v1**. This index functions the same as the token index. It takes one additional parameter before the Bloom filter settings, the size of the ngrams to index. An ngram is a character string of length `n` of any characters, so the string `A short string` with an ngram size of 4 would be indexed as:

```
'A sh', ' sho', 'shor', 'hort', 'ort ', 'rt s', 't st', ' str', 'stri', 'trin', 'ring'
```

**This index can also be useful for text searches, particularly languages without word breaks, such as Chinese.**

## Best Practices

In most cases, a useful skip index requires a strong correlation between the primary key and the targeted, non-primary column/expression. 否则，索引无法发挥过滤无用颗粒的作用，仍会读取几乎所有颗粒，与全表遍历无异。

Another good candidate for a skip index is for high cardinality expressions where any one value is relatively sparse in the data. One example might be an observability platform that tracks error codes in API requests. Certain error codes, while rare in the data, might be particularly important for searches. A set skip index on the error_code column would allow bypassing the vast majority of blocks that don't contain errors and therefore significantly improve error-focused queries.

[Getting started with ClickHouse? Here are 13 "Deadly Sins" and how to avoid them](https://clickhouse.com/blog/common-getting-started-issues-with-clickhouse)





