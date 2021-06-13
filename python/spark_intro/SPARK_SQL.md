- load csv file
  ```python
  >>> df = spark.read.option('header', True).csv('/data/python/spark_intro/sample.csv')
  >>> df
  >>> df.show()
  ```

- create a table from the dataframe and query it
  ```python
  >>> df.createTempView('sample')
  >>> spark.sql("SELECT * FROM sample").show()
  >>> spark.sql("SELECT COUNT(1) FROM sample WHERE width > 2 LIMIT 2").show()
  ```
  - what does the `spark.sql` method return?

- create a table from the access log dataset and get top 10 clients by bytes
  ```python
  >>> access_log = spark.read.option('delimiter', ' ').csv('/data/uncommitted/access.log', header=False).withColumnRenamed('_c0', 'client_ip').withColumnRenamed('_c7', 'bytes')
  >>> access_log.createOrReplaceTempView('access_log')
  >>> spark.sql('''
SELECT
    client_ip,
    SUM(bytes) total_bytes
FROM
    access_log
GROUP BY
    client_ip
ORDER BY
    total_bytes DESC
LIMIT 10;''').show()
  ```