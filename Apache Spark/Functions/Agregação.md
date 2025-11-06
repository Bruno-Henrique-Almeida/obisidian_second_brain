Funções de agregação
- `approx_count_distinct`
- `avg`
- `collect_list`
- `collect_set`
- `count`
- `countDistinct`
- `first`
- `last`
- `max`
- `mean`
- `min`
- `sum`
- `sumDistinct`
### 1. approx_count_distinct(col, rsd=0.05)

Retorna uma estimativa aproximada da contagem de valores distintos em uma coluna, usando o algoritmo HyperLogLog. Útil para grandes volumes de dados, pois consome menos memória.

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

spark = SparkSession.builder.appName("SparkFunctions").getOrCreate()

data = [(1,), (2,), (2,), (3,), (3,), (3,)]
df = spark.createDataFrame(data, ["values"])

df.select(approx_count_distinct("values")).show()
```

**Saída:**

```
+-----------------------------+
|approx_count_distinct(values)|
+-----------------------------+
|                            3|
+-----------------------------+
```

---

### 2. `avg(col)`

Calcula a média dos valores de uma coluna.

```python
from pyspark.sql.functions import avg

df.select(avg("values")).show()
```

**Saída:**

```
+-----------+
|avg(values)|
+-----------+
|      2.333|
+-----------+
```

---

### 3. `collect_list(col)`

Retorna uma lista contendo todos os valores da coluna, incluindo duplicatas.

```python
from pyspark.sql.functions import collect_list

df.groupBy().agg(collect_list("values")).show()
```

**Saída:**

```
+-----------------------+
|collect_list(values)   |
+-----------------------+
|    [1, 2, 2, 3, 3, 3] |
+-----------------------+
```

---

### 4. `collect_set(col)`

Retorna uma lista de valores distintos da coluna.

```python
from pyspark.sql.functions import collect_set

df.groupBy().agg(collect_set("values")).show()
```

**Saída:**

```
+-------------------+
|collect_set(values)|
+-------------------+
|          [1, 2, 3]|
+-------------------+
```

---

### 5. `count(col)`

Conta o número total de elementos em uma coluna, incluindo duplicatas.

```python
from pyspark.sql.functions import count

df.select(count("values")).show()
```

**Saída:**

```
+-------------+
|count(values)|
+-------------+
|            6|
+-------------+
```

---

### 6. `count_distinct(col)`

Conta o número de valores distintos.

```python
from pyspark.sql.functions import countDistinct

df.select(count_distinct("values")).show()
```

**Saída:**

```
+--------------------+
|count(values)       |
+--------------------+
|                   3|
+--------------------+
```

---

### 7. `first(col, ignorenulls=False)`

Retorna o primeiro valor encontrado na coluna.

```python
from pyspark.sql.functions import first

df.select(first("values")).show()
```

**Saída:**

```
+-------------+
|first(values)|
+-------------+
|            1|
+-------------+
```

---

### 8. `last(col, ignorenulls=False)`

Retorna o último valor encontrado na coluna.

```python
from pyspark.sql.functions import last

df.select(last("values")).show()
```

**Saída:**

```
+------------+
|last(values)|
+------------+
|          3 |
+-------=----+
```

---

### 9. `max(col)`

Retorna o valor máximo da coluna.

```python
from pyspark.sql.functions import max

df.select(max("values")).show()
```

**Saída:**

```
+-----------+
|max(values)|
+-----------+
|          3|
+-----------+
```

---

### 10. `mean(col)`

Outra forma de calcular a média, equivalente a `avg(col)`.

```python
from pyspark.sql.functions import mean

df.select(mean("values")).show()
```

**Saída:**

```
+------------+
|mean(values)|
+------------+
|       2.333|
+------------+
```

---

### 11. `min(col)`

Retorna o menor valor da coluna.

```python
from pyspark.sql.functions import min

df.select(min("values")).show()
```

**Saída:**

```
+-----------+
|min(values)|
+-----------+
|          1|
+-----------+
```

---

### 12. `sum(col)`

Retorna a soma de todos os valores da coluna.

```python
from pyspark.sql.functions import sum

df.select(sum("values")).show()
```

**Saída:**

```
+-----------+
|sum(values)|
+-----------+
|         14|
+-----------+
```

---

### 13. `sumDistinct(col)`

Retorna a soma apenas dos valores distintos na coluna.

```python
from pyspark.sql.functions import sumDistinct

df.select(sum_distinct("values")).show()
```

**Saída:**

```
+-------------------+
|sumDistinct(values)|
+-------------------+
|                  6|
+-------------------+
```

(Apenas `1 + 2 + 3`, sem contar as repetições).
