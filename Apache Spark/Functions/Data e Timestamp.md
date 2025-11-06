Funções Data e Timestamp
- `current_date`
- `current_timestamp`
- `date_add`
- `date_format`
- `date_sub`
- `datediff`
- `dayofmonth`
- `dayofweek`
- `dayofyear`
- `from_unixtime`
- `hour`
- `last_day`
- `minute`
- `month`
- `next_day`
- `second`
- `to_date`
- `to_timestamp`
- `trunc`
- `unix_timestamp`
- `weekofyear`
- `year`

### 1. current_date()
Retorna a data atual no formato `YYYY-MM-DD`, sem a parte de hora.

```python
df.select(
    'date_only',
    F.current_date()
).show()
```

**Saída**

```
+----------+--------------+
| date_only|current_date()|
+----------+--------------+
|2025-03-13|    2025-03-13|
|2023-05-22|    2025-03-13|
|2021-08-09|    2025-03-13|
|2022-12-31|    2025-03-13|
+----------+--------------+
```
### 2. current_timestamp()
Retorna a data e hora atuais no formato `YYYY-MM-DD HH:MM:SS`.

```python
df.select(
    'date_only',
    F.current_timestamp()
).show(truncate=False)
```

**Saída**

```
+----------+--------------------------+
|date_only |current_timestamp()       |
+----------+--------------------------+
|2025-03-13|2025-03-13 22:06:43.425756|
|2023-05-22|2025-03-13 22:06:43.425756|
|2021-08-09|2025-03-13 22:06:43.425756|
|2022-12-31|2025-03-13 22:06:43.425756|
+----------+--------------------------+
```
### 3.date_add
Adiciona um número específico de dias a uma data fornecida e retorna a nova data.

```python
df.select(
    'date_only',
    F.date_add('date_only', 1)
).show()
```

**Saída**

```
+----------+----------------------+
| date_only|date_add(date_only, 1)|
+----------+----------------------+
|2025-03-13|            2025-03-14|
|2023-05-22|            2023-05-23|
|2021-08-09|            2021-08-10|
|2022-12-31|            2023-01-01|
+----------+----------------------+
```
### 4. date_format
Formata uma data ou timestamp de acordo com o formato especificado.

```python
df.select(
    'datetime',
    F.date_format('datetime', 'a'),
    F.date_format('datetime', 'E')
).show()
```

**Saída**

```
+-------------------+------------------------+------------------------+
|           datetime|date_format(datetime, a)|date_format(datetime, E)|
+-------------------+------------------------+------------------------+
|2025-03-13 12:30:00|                      PM|                     Thu|
|2023-05-22 09:15:00|                      AM|                     Mon|
|2021-08-09 17:45:00|                      PM|                     Mon|
|2022-12-31 00:00:00|                      AM|                     Sat|
+-------------------+------------------------+------------------------+
```
### 5. date_sub
Subtrai um número específico de dias de uma data fornecida e retorna a nova data.

```python
df.select(
    'datetime',
    F.date_sub('datetime', 5)
).show()
```

**Saída**

```
+-------------------+---------------------+
|           datetime|date_sub(datetime, 5)|
+-------------------+---------------------+
|2025-03-13 12:30:00|           2025-03-08|
|2023-05-22 09:15:00|           2023-05-17|
|2021-08-09 17:45:00|           2021-08-04|
|2022-12-31 00:00:00|           2022-12-26|
+-------------------+---------------------+
```
### 6. date_diff
Retorna o número de dias entre duas datas fornecidas.

```python
df.withColumn(
    'days_diff',
    F.datediff('datetime', F.expr("DATE('2025-03-23')"))
).show()
```

**Saída**

```
+---+----------+-------------------+---------+
| id| date_only|           datetime|days_diff|
+---+----------+-------------------+---------+
|  1|2025-03-13|2025-03-13 12:30:00|      -10|
|  2|2023-05-22|2023-05-22 09:15:00|     -671|
|  3|2021-08-09|2021-08-09 17:45:00|    -1322|
|  4|2022-12-31|2022-12-31 00:00:00|     -813|
+---+----------+-------------------+---------+
```
### 7. dayofmonth
Retorna o dia do mês de uma data ou timestamp.

```python
df.select(
    'date_only',
    F.dayofmonth('date_only')
).show()
```

**Saída**

```
+----------+---------------------+
| date_only|dayofmonth(date_only)|
+----------+---------------------+
|2025-03-13|                   13|
|2023-05-22|                   22|
|2021-08-09|                    9|
|2022-12-31|                   31|
+----------+---------------------+
```
### 8. dayofweek
Retorna o dia da semana de uma data ou timestamp (com 1 representando domingo, 2 segunda-feira, etc).

```python
df.select(
    'date_only',
    F.dayofweek('date_only')
).show()
```

**Saída**

```
+----------+--------------------+
| date_only|dayofweek(date_only)|
+----------+--------------------+
|2025-03-13|                   5|
|2023-05-22|                   2|
|2021-08-09|                   2|
|2022-12-31|                   7|
+----------+--------------------+
```
### 9. dayofyear
Retorna o dia do ano de uma data ou timestamp (de 1 a 365 ou 366).

```python
df.select(
    'date_only',
    F.dayofyear('date_only')
).show()
```

**Saída**

```
+----------+--------------------+
| date_only|dayofyear(date_only)|
+----------+--------------------+
|2025-03-13|                  72|
|2023-05-22|                 142|
|2021-08-09|                 221|
|2022-12-31|                 365|
+----------+--------------------+
```
### 10. from_unixtime
Converte um valor de timestamp Unix (número de segundos desde 1970-01-01) para uma data ou timestamp legível.

```python

```

**Saída**

```

```
### 11. hour
Retorna a hora (parte de hora) de um timestamp.

```python

```

**Saída**

```

```
### 12. last_day
Retorna o último dia mês para uma data fornecida.

```python

```

**Saída**

```

```
### 13. minute
Retorna o minuto (parte de minuto) de um timestamp.

```python

```

**Saída**

```

```
### 14. month
Retorna o mês (de 1 a 12) de uma data ou timestamp.

```python

```

**Saída**

```

```
### 15. next_day
Retorna a data do próximo dia da semana especificado a partir de uma data fornecida.

```python

```

**Saída**

```

```
### 16. second
Retorna o segundo (parte do segundo) de um timestamp.

```python

```

**Saída**

```

```
### 17. to_date
Converte uma string no formato de data para um tipo de dado de data.

```python

```

**Saída**

```

```
### 18. to_timestamp
Converte uma string ou número de segundos Unix para um tipo de dado timestamp.

```python

```

**Saída**

```

```
### 19. trunc
Trunca uma data ou timestamp até a unidade de tempo especificada (como ano, mês, dia, etc.).

```python

```

**Saída**

```

```

### 20. unix_timestamp
Retorna o número de segundos desde a época Unix (1970-01-01) para uma data ou timestamp fornecido.

```python

```

**Saída**

```

```
### 21. weekofyear
Retorna o número da semana do ano para uma data ou timestamp.

```python

```

**Saída**

```

```
### 22. year
Retorna o ano (parte de ano) de uma data ou timestamp.

```python

```

**Saída**

```

```