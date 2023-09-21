
Relational Algebra is a procedural query language, which takes relation as input and generates relation as output. Relational algebra mainly provides a theoretical foundation for relational databases and SQL.

## Unary Operators (on single relation)

### Projection (π)

Projection (π) retains only desired columns (**vertical**).

Projection corresponds to the `SELECT` list in SQL.

Suppose we want column A and column B from Relation R:
```shell
     R 
  (A  B  C)    
  ----------
   1  2  4
   2  2  3
   3  2  3
   4  3  4
```

`π B,C(R)` will show following columns:
```shell
B  C
-----
2  4
2  3
3  4
```

### Selection (σ)

Selection (σ) selects a subset of rows(**horizontal**)

Selection corresponds to the `WHERE` clause in SQL.

For example, `σ c>3(R)` will select tuples with `c` more than 3.

> Selection operator only selects the required tuples but **doesn't display them**. For display, the [[Database/Relational Algebra#Projection (π)|projection (π)]] operator is used.

### Renaming (ρ)

Renaming renames attributes and relations. `ρ(a/b)R` will rename the attribute `b` of the relation by `a`.

## Binary Operators (on pairs of relations)

### Union (U)

Tuples in `r1` or in `r2`. 
Both relations must have the **same set of attributes**.

### Set-difference (-)

Tuples in `r1`, but not in `r2`.
Both relations must have the **same set of attributes**.

### Cross-product (x)

Allows us to combine two relations.
`r1 x s1`: each row of `r1` paired with each row of `s1`.

```shell
   A                                  B
    (Name   Age  Sex )                (Id   Course)  
    ------------------                -------------
    Ram    14   M                      1     DS
    Sona   15   F                      2     DBMS
    kim    20   M

     A X B
  Name   Age   Sex   Id   Course
---------------------------------
  Ram    14    M      1    DS
  Ram    14    M      2    DBMS
  Sona   15    F      1    DS
  Sona   15    F      2    DBMS
  Kim    20    M      1    DS
  Kim    20    M      2    DBMS
```

## Compound Operators

common "macros" for the above

### Intersection  (∩)

Tuples in `r1` and in `r2`.

### Joins (⋈)

Combine relations that satisfy predicates.

## Aggregation Oeraotr (γ)

Corresponds to the `GROUP BY` clause in SQL.
