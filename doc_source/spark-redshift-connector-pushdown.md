# Performance improvements with pushdown<a name="spark-redshift-connector-pushdown"></a>

 The Spark connector automatically applies predicate and query pushdown to optimize for performance\. This support means that if you’re using a supported function in your query, the Spark connector will turn the function into a SQL query and run the query in Amazon Redshift\. This optimization results in less data being retrieved, so Apache Spark can process less data and have better performance\. By default, pushdown is automatically activated\. To deactivate it, set `autopushdown` to false\. 

```
import sqlContext.implicits._val 
 sample= sqlContext.read
    .format("io.github.spark_redshift_community.spark.redshift")
    .option("url",jdbcURL )
    .option("tempdir", tempS3Dir)
    .option("dbtable", "event")
    .option("autopushdown", "false")
    .load()
```

 The following functions are supported with pushdown\. If you’re using a function that’s not in this list, the Spark connector will perform the function in Spark instead of Amazon Redshift, resulting in unoptimized performance\. For a complete list of functions in Spark, see [Built\-in Functions](https://spark.apache.org/docs/3.3.0/api/sql/index.html)\. 
+ Aggregation functions
  + avg
  + count
  + max
  + min
  + sum
+ Boolean operators
  + in
  + isnull
  + isnotnull
  + contains
  + endswith
  + startswith
+ Logical operators
  + and
  + or
  + not \(or \!\)
+ Mathematical functions
  + \+
  + \-
  + \*
  + /
  + \- \(unary\)
  + abs
  + acos
  + asin
  + atan
  + ceil
  + cos
  + exp
  + floor
  + greatest
  + least
  + log10
  + pi
  + pow
  + round
  + sin
  + sqrt
  + tan
+ Miscellaneous functions
  + cast
  + coalesce
  + decimal
  + if
  + in
+ Relational operators
  + \!=
  + =
  + >
  + >=
  + <
  + <=
+ String functions
  + ascii
  + lpad
  + rpad
  + translate
  + upper
  + lower
  + length
  + trim
  + ltrim
  + rtrim
  + like
  + substring
  + concat
+ Time and date functions
  + add\_months
  + date
  + date\_add
  + date\_sub
  + date\_trunc
  + timestamp
  + trunc
+ Mathematical operations
  + CheckOverflow
  + PromotePrecision
+ Relational operations
  + Aliases \(for example, AS\)
  + CaseWhen
  + Distinct
  + InSet
  + Joins
  + Limits
  + Unions, union all
  + ScalarSubquery
  + Sorts \(ascending and descending\)
  + UnscaledValue