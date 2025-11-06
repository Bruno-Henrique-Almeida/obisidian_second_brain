## Machine Learning com Apache Spark

### Introdução

O Apache Spark fornece uma poderosa biblioteca de aprendizado de máquina chamada **MLlib**, que permite construir e treinar modelos de Machine Learning em larga escala. Utilizando processamento distribuído, Spark MLlib facilita a criação de pipelines eficientes para tarefas de **regressão, classificação, clustering e recomendação**.

Neste eBook, exploraremos os conceitos fundamentais de **Machine Learning com Spark**, focando nas técnicas de **Regressão** e **Classificação**.

---

## 1. Fundamentos do MLlib

O **MLlib** é a biblioteca de aprendizado de máquina do Spark, que oferece suporte a:

- **Transformações e pré-processamento de dados** (normalização, encoding, vetorização).
- **Construção de modelos de Machine Learning** (Regressão, Classificação, Clustering).
- **Pipelines de ML** para facilitar o fluxo de trabalho.
- **Avaliação de modelos** e ajuste de hiperparâmetros.

Para utilizar o **Spark MLlib**, precisamos criar uma **SparkSession**:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("ML with Spark") \
    .getOrCreate()
```

---

## 2. Regressão com Spark

A **Regressão** é uma técnica usada para prever valores contínuos com base em variáveis de entrada. O MLlib suporta diferentes tipos de regressão, como **Regressão Linear** e **Regressão Logística**.

### Exemplo de Regressão Linear

```python
from pyspark.ml.regression import LinearRegression
from pyspark.ml.feature import VectorAssembler

# Carregar dataset de exemplo
data = spark.read.csv("data.csv", header=True, inferSchema=True)

# Transformar colunas de entrada em um vetor
assembler = VectorAssembler(inputCols=["feature1", "feature2"], outputCol="features")
data = assembler.transform(data)

# Definir modelo de regressão
lr = LinearRegression(featuresCol="features", labelCol="target")

# Treinar modelo
model = lr.fit(data)

# Fazer previsões
predictions = model.transform(data)
predictions.select("target", "prediction").show()
```

### Aplicações da Regressão

- **Previsão de preços de imóveis**.
- **Estimativa de demanda de produtos**.
- **Análise de séries temporais**.

---

## 3. Classificação com Spark

A **Classificação** é usada para prever categorias discretas, como "fraude ou não fraude" ou "spam ou não spam". O Spark MLlib suporta algoritmos como **Árvores de Decisão, Random Forest, SVM e Redes Neurais**.

### Exemplo de Classificação com Random Forest

```python
from pyspark.ml.classification import RandomForestClassifier

# Criar modelo de classificação
rf = RandomForestClassifier(featuresCol="features", labelCol="label", numTrees=10)

# Treinar modelo
model = rf.fit(data)

# Fazer previsões
predictions = model.transform(data)
predictions.select("label", "prediction").show()
```

### Aplicações da Classificação

- **Detecção de fraudes em transações bancárias**.
- **Filtragem de spam em e-mails**.
- **Diagnóstico de doenças a partir de exames médicos**.

---

## 4. Avaliação e Ajuste de Modelos

Para avaliar a performance dos modelos, o Spark MLlib fornece métricas como:

- **Erro Quadrático Médio (RMSE)** para regressão.
- **Acurácia, Precisão e Recall** para classificação.

Exemplo de cálculo de acurácia:

```python
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

evaluator = MulticlassClassificationEvaluator(labelCol="label", predictionCol="prediction", metricName="accuracy")
accuracy = evaluator.evaluate(predictions)
print(f"Acurácia: {accuracy}")
```

Para otimizar modelos, podemos utilizar **tuning de hiperparâmetros** com `CrossValidator` e `ParamGridBuilder`.

---

## Conclusão

O Apache Spark MLlib oferece uma abordagem escalável para treinamento de modelos de Machine Learning, permitindo processar grandes volumes de dados com eficiência. Técnicas de **Regressão** e **Classificação** são amplamente utilizadas em diversas aplicações do mundo real.

Neste eBook, cobrimos os conceitos básicos e exemplos práticos dessas técnicas. Você pode explorar mais algoritmos, integrar Spark com Deep Learning e otimizar seus modelos para produção!
