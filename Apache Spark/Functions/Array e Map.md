Funções Array e Map
- `array`
- `array_contains`
- `array_distinct`
- `array_except`
- `array_intersect`
- `array_join`
- `array_max`
- `array_min`
- `array_position`
- `array_remove`
- `array_repeat`
- `array_sort`
- `array_union`
- `arrays_overlap`
- `arrays_zip`
- `explode`
- `explode_outer`
- `flatten`
- `map`
- `map_concat`
- `map_entries`
- `map_filter`
- `map_from_arrays`
- `map_from_entries`
- `map_keys`
- `map_values`
- `transform`
- `zip_with`
## Funções Array
### 1. array(array)
Cria um array a apartir de valores fornecidos.

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

spark = SparkSession.builder.getOrCreate()

data = [(1, ['apple', 'banana', 'cherry', 'banana']), (2, ['apple', 'orange', 'banana', 'orange'])]

df = spark.createDataFrame(data, ["id", "fruits"])

df.select(F.array('fruits')).show(truncate=False)
```

**Saída**

```
+---------------------------------+
|array(fruits)                    |
+---------------------------------+
|[[apple, banana, cherry, banana]]|
|[[apple, orange, banana, orange]]|
+---------------------------------+
```
### 2. array_contains(array, value)
Verifica se um valor está presente em um array.

```python
df.select(F.array_contains('fruits', 'orange')).show()
```

**Saída**

```
+------------------------------+
|array_contains(fruits, orange)|
+------------------------------+
|                         false|
|                          true|
+------------------------------+
```
### 3. array_distinct(array)
Remove os valores duplicados de um array.

```python
df.select(F.array_distinct('fruits')).show(truncate=False)
```

**Saída**

```
+-----------------------+
|array_distinct(fruits) |
+-----------------------+
|[apple, banana, cherry]|
|[apple, orange, banana]|
+-----------------------+
```

### 4. array_except(array, array)
Retorna os elementos de um array que não estão presentes no outro.

```python
df.select(F.array_except('fruits', F.array(F.lit('banana')))).show(truncate=False)
```

**Saída**

```
+-----------------------------------+
|array_except(fruits, array(banana))|
+-----------------------------------+
|[apple, cherry]                    |
|[apple, orange]                    |
+-----------------------------------+
```

### 5. array_intesect(array, array)
Retorna os elementos comuns entre dois arrays.
```python
df.select(F.array_intersect('fruits', F.array(*[F.lit(x) for x in ['banana', 'cherry']]))).show()
```

**Saída**

```
+----------------------------------------------+
|array_intersect(fruits, array(banana, cherry))|
+----------------------------------------------+
|[banana, cherry]                              |
|[banana]                                      |
+----------------------------------------------+
```
### 6. array_join(array, delimiter)
Junta os elementos de um array em uma string, usando um delimitador.

```python
df.select(F.array_join('fruits',', ')).show()
```

**Saída**

```
+-----------------------------+
|array_join(fruits, , )       |
+-----------------------------+
|apple, banana, cherry, banana|
|apple, orange, banana, orange|
+-----------------------------+
```
### 7. array_max(col)
Retorna o maior valor de um array.

```python
df.select(F.array_max('fruits')).show()
```

**Saída**

```
+-----------------+
|array_max(fruits)|
+-----------------+
|           cherry|
|           orange|
+-----------------+
```
### 8. array_min(col)
Retorna o menor valor de um array.

```python
df.select(F.array_min('fruits')).show()
```

**Saída**

```
+-----------------+
|array_min(fruits)|
+-----------------+
|            apple|
|            apple|
+-----------------+
```
### 9. array_position(array, value)
Retorna a posição de um elemento no array.

```python
df.select(F.array_position('fruits', 'banana')).show()
```

**Saída**

```
+------------------------------+
|array_position(fruits, banana)|
+------------------------------+
|                             2|
|                             3|
+------------------------------+
```
### 10. array_remove
Remove todas as ocorrências de um valor do array.

```python
df.select(F.array_remove('fruits', 'banana')).show(truncate=False)
```

**Saída**

```
+----------------------------+
|array_remove(fruits, banana)|
+----------------------------+
|[apple, cherry]             |
|[apple, orange, orange]     |
+----------------------------+
```
### 10. array_repeat
Repete os elementos de um array um número específico de vezes.

```python
df.select(F.array_repeat('fruits', 2)).show(truncate=False)
```

**Saída**

```
+------------------------------------------------------------------+
|array_repeat(fruits, 2)                                           |
+------------------------------------------------------------------+
|[[apple, banana, cherry, banana], [apple, banana, cherry, banana]]|
|[[apple, orange, banana, orange], [apple, orange, banana, orange]]|
+------------------------------------------------------------------+
```
### 11. array_sort
Ordena os elementos de um array.

```python
df.select(F.array_sort('fruits')).show()
```

**Saída**

```
+-------------------------------+
|array_sort('fruits')           |
+-------------------------------+
|[apple, banana, banana, cherry]|
|[apple, banana, orange, orange]|
+-------------------------------+
```
### 12. array_union
Retorna a união de dois arrays, eliminando duplicadas.

```python
df.select(F.array_union('fruits', F.array(*[F.lit(x) for x in ['strawberry', 'cherry']]))).show(truncate=False)
```

**Saída**

```
+----------------------------------------------+
|array_union(fruits, array(strawberry, cherry))|
+----------------------------------------------+
|[apple, banana, cherry, strawberry]           |
|[apple, orange, banana, strawberry, cherry]   |
```
### 13. arrays_overlap
Retorna se há sobreposição entre dois arrays.

```python
df.select(F.arrays_overlap('fruits', F.array(*[F.lit(x) for x in ['strawberry', 'cherry']]))).show()
```

**Saída**

```
+-------------------------------------------------+
|arrays_overlap(fruits, array(strawberry, cherry))|
+-------------------------------------------------+
|                                             true|
|                                            false|
+-------------------------------------------------+
```

### 14. arrays_zip
Junta dois arrays em um único arrays de structs.

```python
df.select(F.arrays_zip('fruits', F.array(*[F.lit(x) for x in ['strawberry', 'cherry']]))).show(truncate=False)
```

**Saída**

```
+-----------------------------------------------------------------------+
|arrays_zip(fruits, array(strawberry, cherry))                          |
+-----------------------------------------------------------------------+
|[{apple, strawberry}, {banana, cherry}, {cherry, NULL}, {banana, NULL}]|
|[{apple, strawberry}, {orange, cherry}, {banana, NULL}, {orange, NULL}]|
+-----------------------------------------------------------------------+
```

### 15. explode
Expande um array em várias linhas.

```python
df.select(F.explode('fruits')).show()
```

**Saída**

```
+------+
|   col|
+------+
| apple|
|banana|
|cherry|
|banana|
| apple|
|orange|
|banana|
|orange|
+------+
```

### 16. explode_outer
Expande um array em várias linhas, mas mantém valores nulos.

```python
df.select(F.explode_outer('fruits')).show()
```

**Saída**

```
+------+
|   col|
+------+
| apple|
|banana|
|cherry|
|banana|
| apple|
|orange|
|banana|
|orange|
+------+
```

### 17. flatten
Achata um array de arrays em um único array.

```python
df.select(F.flatten(F.array('fruits'))).show(truncate=False)
```

**Saída**

```
+-------------------------------+
|flatten(array(fruits))         |
+-------------------------------+
|[apple, banana, cherry, banana]|
|[apple, orange, banana, orange]|
+-------------------------------+
```

## Funções Map



### 1. map_concat
Concatena dois mapas.

```python
```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

spark = SparkSession.builder.getOrCreate()

map_data = [(1, {"key1": "value1", "key2": "value2"}), (2, {"key3": "value3", "key4": "value4"})]

map_df = spark.createDataFrame(map_data, ["id", "map_example"])

map_df.select(F.map_concat('map_example', F.expr("map('key5', 'value5')"))).show()
```

**Saída**

```
+------------------------------------------+
|map_concat(map_example, map(key5, value5))|
+------------------------------------------+
|                      {key1 -> value1, ...|
|                      {key3 -> value3, ...|
+------------------------------------------+
```

### 2. map_entries
Retorna os pares chave-valor de um mapa como um array de structs.

```python
map_df.select(F.map_entries('map_example')).show(truncate=False)
```

**Saída**

```
+--------------------------------+
|map_entries(map_example)        |
+--------------------------------+
|[{key1, value1}, {key2, value2}]|
|[{key3, value3}, {key4, value4}]|
+--------------------------------+
```

### 3. map_filter
Filtra os elementos de um mapa com base em uma condição.

```python
map_df.select(
    F.map_filter('map_example', lambda k, v: v != 'value4')
    .alias("F.map_filter('map_example', lambda k, v: v != 'value4')")
).show(truncate=False)
```

**Saída**

```
+-------------------------------------------------------+
|F.map_filter('map_example', lambda k, v: v != 'value4')|
+-------------------------------------------------------+
|{key1 -> value1, key2 -> value2}                       |
|{key3 -> value3}                                       |
+-------------------------------------------------------+
```

### 4. map_from_arrays
Cria um mapa a partir de duas arrays (uma de chaves e outra de valores).

```python
map_df.select(
    F.map_from_arrays(
        F.expr("array('key5', 'key6')"),
        F.expr("array('value5', 'value6')")
    )
).show(truncate=False)
```

**Saída**

```
+---------------------------------------------------------+
|map_from_arrays(array(key5, key6), array(value5, value6))|
+---------------------------------------------------------+
|{key5 -> value5, key6 -> value6}                         |
|{key5 -> value5, key6 -> value6}                         |
+---------------------------------------------------------+
```

### 5. map_from_entries
Cria um mapa a partir de uma array de structs contendo chaves e valores.

```python
map_df.select(
    F.map_from_entries(F.expr("array(('key5', 'value5'), ('key6', 'value6'))"))
).show(truncate=False)
```

**Saída**

```
+-------------------------------------------------------------------------------------------------------+
|map_from_entries(array(named_struct(col1, key5, col2, value5), named_struct(col1, key6, col2, value6)))|   
+-------------------------------------------------------------------------------------------------------+   
|{key5 -> value5, key6 -> value6}                                                                       |   
|{key5 -> value5, key6 -> value6}                                                                       |   
+-------------------------------------------------------------------------------------------------------+
```

### 6. map_keys
Retorna todas as chaves de um mapa.

```python
map_df.select(F.map_keys('map_example')).show()
```

**Saída**

```
+---------------------+
|map_keys(map_example)|
+---------------------+
|         [key1, key2]|
|         [key3, key4]|
+---------------------+
```

### 7. map_values
Retorna todos os valores de um mapa.

```python
map_df.select(F.map_values('map_example')).show()
```

**Saída**

```
+-----------------------+
|map_values(map_example)|
+-----------------------+
|       [value1, value2]|
|       [value3, value4]|
+-----------------------+
```

### 8. transform
Aplica uma transformação a cada valor de um mapa.

```python
df.select(
    'fruits',
    F.transform('fruits', lambda x: F.concat(x, F.lit('_fruit')))
    .alias('transform(col, func)')
).show(truncate=False)
```

**Saída**

```
+-------------------------------+-------------------------------------------------------+
|fruits                         |transform(col, func)                                   |
+-------------------------------+-------------------------------------------------------+
|[apple, banana, cherry, banana]|[apple_fruit, banana_fruit, cherry_fruit, banana_fruit]|
|[apple, orange, banana, orange]|[apple_fruit, orange_fruit, banana_fruit, orange_fruit]|
+-------------------------------+-------------------------------------------------------+
```

### 9. zip_with
Combina dois array em um novo array com base em uma função personalizada.

```python
df.select(
    F.expr('array(1, 2, 3)'),
    F.expr('array(4, 5, 6)'),
    F.zip_with(
        F.expr('array(1, 2, 3)'),
        F.expr('array(4, 5, 6)'),
        lambda x, y: x + y
    ).alias('zip_with(array, array, func)')
).show(truncate=False)
```

**Saída**

```
+--------------+--------------+----------------------------+
|array(1, 2, 3)|array(4, 5, 6)|zip_with(array, array, func)|
+--------------+--------------+----------------------------+
|[1, 2, 3]     |[4, 5, 6]     |[5, 7, 9]                   |
|[1, 2, 3]     |[4, 5, 6]     |[5, 7, 9]                   |
+--------------+--------------+----------------------------+
```
