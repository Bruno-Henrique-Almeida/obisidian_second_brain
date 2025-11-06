## Cache & Persistência no Apache Spark

### Introdução

O Apache Spark é uma ferramenta poderosa para o processamento distribuído de dados, mas seu desempenho pode ser significativamente melhorado com o uso eficiente de **cache** e **persistência**. Esses mecanismos ajudam a reduzir a necessidade de recomputar dados e evitam leituras repetitivas, otimizando a execução de pipelines de dados.

Neste eBook, exploraremos as diferenças entre **cache** e **persistência**, os diferentes níveis de armazenamento disponíveis e boas práticas para utilizá-los corretamente.

---

## 1. Diferença entre Cache e Persistência

### Cache

O **cache** é usado para armazenar os dados temporariamente na memória para evitar recomputações dispendiosas. No Spark, podemos utilizar o método `.cache()`, que armazena o DataFrame ou RDD no **Memory_Only**.

Exemplo:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("CacheExample").getOrCreate()

data = spark.read.csv("data.csv", header=True, inferSchema=True)
data.cache()  # Armazena os dados em memória

# A primeira ação aciona a leitura do arquivo
print(data.count())
```

### Persistência

A **persistência** permite armazenar os dados em diferentes níveis de armazenamento, como **memória, disco ou ambos**. Para isso, utilizamos o `.persist(storageLevel)`, onde o `storageLevel` define como os dados serão mantidos.

Exemplo:

```python
from pyspark import StorageLevel

data.persist(StorageLevel.MEMORY_AND_DISK)
```

Se a memória for insuficiente, os dados persistidos podem ser armazenados no disco, evitando erros de falta de memória.

---

## 2. Níveis de Armazenamento no Spark

O Spark oferece diferentes níveis de armazenamento para cache e persistência:

|Storage Level|Descrição|
|---|---|
|MEMORY_ONLY|Armazena os dados apenas na memória RAM.|
|MEMORY_AND_DISK|Mantém os dados na memória, mas escreve no disco se necessário.|
|MEMORY_ONLY_SER|Igual ao MEMORY_ONLY, mas armazena os dados serializados, economizando memória.|
|MEMORY_AND_DISK_SER|Igual ao MEMORY_AND_DISK, mas armazena os dados serializados.|
|DISK_ONLY|Armazena os dados apenas no disco.|

---

## 3. Quando Utilizar Cache e Persistência

- **Use `.cache()`** quando os dados cabem na memória e serão reutilizados várias vezes.
- **Use `.persist(StorageLevel.MEMORY_AND_DISK)`** quando há risco de falta de memória.
- **Evite excesso de cache/persistência**, pois pode ocupar muita memória e reduzir o desempenho.
- **Remova dados não mais necessários** com `.unpersist()` para liberar recursos:

```python
data.unpersist()
```

---

## Conclusão

O uso estratégico de **cache** e **persistência** no Apache Spark melhora significativamente a performance das aplicações, reduzindo recomputações e otimizando o uso de recursos.

Ao entender os diferentes níveis de armazenamento e quando usá-los, você pode construir pipelines de dados mais eficientes e escaláveis!