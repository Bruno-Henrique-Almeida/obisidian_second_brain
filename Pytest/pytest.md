**Unit Test (Teste Unitário)** - Um teste unitário é um tipo de teste automatizado que verifica se uma parte específica do código (uma "unidade") - normalmente uma função ou método está funcionando normalmente. A ideia é isolar o comportamento de cada função para garantir que ela retorne o resultado  esperado independentemente de outros componentes do sistema.
```py
# calculator.py
def add(a, b):
    return a + b
```
```py
from calculator import add

def test_add():
    result = add(2, 3)
    assert result == 5
```

**Pytest** - É uma ferramenta (framework) para rodar testes em Python. Ele é simples, poderoso e muito usado em projetos profissionais.

Com ele, basta:
> Nomear arquivos de teste com prefixo **test_**
> Nomear funções de teste também com  prefixo **test_**
> Usar **assert** para comprar resultados esperados e obtidos.

```py
math_utils.py
def multiply(a, b):
    return a * b
```
```py
test_math_utils.py
from math_utils import multiply

def test_multiply():
    assert multiply(2, 4) == 8
    assert multiply(0, 5) == 0
```
```py
pytest -v
```
```cpp
test_math_utils.py::test_multiply PASSED
```

**Fixtures** - É uma função especial do pytest usada para **preparar o ambiente de teste** - por exemplo, criar uma conexão com o banco, gerar dados de exemplo ou inicializar um objeto. Ela serve **evitar repetição de código** e garantir que cada teste tenha o mesmo contexto.

```py
import pytest

@pytest.fixture
def sample_data():
  return [1, 2, 3, 4, 5]

def test_sum(sample_data):
  assert sum(sample_data) == 15

def test_length(sample_data):
  asset len(sample_data) == 5
```
> A fixture sample_data é executada **automaticamente **antes de cada teste que a usa como argumento.
```py
# Ler um csv e transformar dados
import pytest
import pandas as pd

@pytest.fixture
def sample_df():
  data = {'name': ['Lucca', 'Alice'], 'age': [25, 30]}
  return pd.DataFrame(data)

def test_mean_age(sample_df):
  assert sample_df['age'].mean() == 27.5
```

**Mocks (Simulação)** - É uma técnica usada para** substituir partes do código que interagem com recursos externos** (como bancos de dados, APIs ou sistemas de arquivos), de modo a testar apenas a lógica interna.  Com mocks, você **finge ** que algo foi executado - mas sem realmente fazer chamadas externas.

```py
from unittest.mock import Mock

def get_user_age(api_client, user_id):
    response = api_client.get_user(user_id)
    return response["age"]

def test_get_user_age():
    fake_api = Mock()
    fake_api.get_user.return_value = {"name": "Bruno", "age": 32}

    result = get_user_age(fake_api, 1)

    assert result == 32
```
> Aqui, **fake_api** simula um cliente de API real.
> Não há conexão externa — tudo é controlado dentro do teste.

```py
# Simulando com banco de dados
from unittest.mock import patch
import pandas as pd

def get_table(conn, table_name):
    return pd.read_sql(f"SELECT * FROM {table_name}", conn)

@patch("pandas.read_sql")
def test_get_table(mock_read_sql):
    mock_read_sql.return_value = pd.DataFrame({"id": [1], "name": ["Test"]})
    
    df = get_table("fake_conn", "users")
    assert "name" in df.columns
    assert df.iloc[0]["name"] == "Test"
```
> Aqui, usamos **@patch** para simular o comportamento do **pandas.read_sql**, evitando acesso a um banco real.