---
layout: post
date:  2020-01-16 00:00:00 -0300
title:  "Core: Fazendo o CRUD"
slug: "sql-alchemy-essencial"
tag: books
category:  "SQL Alchemy Essencial"
comments: true
---

![Capa do Livro](../assets/img/sql-alchemy-essencial.jpg)

O trabalho de definir as tabelas do Banco de Dados, é um dos mais importantes, mas a realização do CRUD (*Create, Read, Update, Delete*, isto é, Criação, Leitura, Atualização e Deleção) de dados é a interação do desenvolvedor com o Banco. Por isso, cada uma dessas etapas serão analisadas dentro do SQL Alchemy Core.

### Inserindo Dados

#### Usando o Método `insert()`

Há algumas formas de inserir dados através do SQL Alchemy, a primeira delas é através do método `insert()`:

```python
ins = cookies.insert().values(
    cookie_name="chocolate chip",
    cookie_recipe_url="http://some.aweso.me/cookie/recipe.html",
    cookie_sku="CC01",
    quantity="12",
    unit_cost="0.50"
)

print(str(ins))
```

Note que o retorno da função `print` é uma string com um comando SQL, gerado pelo SQL Alchemy, que será executado:

```sql
INSERT INTO cookies (
    cookie_name,
    cookie_recipe_url,
    cookie_sku, quantity,
    unit_cost)
VALUES (
    :cookie_name,
    :cookie_recipe_url,
    :cookie_sku,
    :quantity,
    :unit_cost
)
```

Observe que os valores a serem inseridos, foram apresentados como um placeholder `:column_name`. Isso garante que todos os valores que correspondam ao nome da coluna sejam corretamente recebidos pelo SQL Alchemy, evitando com isso ataques do tipo SQL Injection.

Para ter acesso aos parâmetros passados, o SQL Alchemy utiliza do SQLCompiler, que pode ser acessado através do método `compile()`. No caso o exemplo acima, utilizando o código:

```python
print(str(ins.compile().params))
```

Teríamos como resposta o seguinte resultado:

```python
{
    'cookie_name': 'chocolate chip',
    'cookie_recipe_url': 'http://some.aweso.me/cookie/recipe.html',
    'cookie_sku': 'CC01',
    'quantity': '12',
    'unit_cost': '0.50'
}
```

Para, de fato, executar o comando de inserção, é preciso chamar o método `execute()` da nossa conexão, deste modo:

```python
result = connection.execute(ins)
```

Também é possível receber como resposta a chave-primária do objeto criado, dessa forma:

```python
print(result.inserted_primary_key)
```

A ideia de receber a chave-primária do objeto criado, pode ser útil para confirmar o sucesso da execução sem que, para isso, seja necessário realizar uma consulta ao banco.

#### Usando a Função `insert()`

Outra forma de inserir dados, no Banco de Dados é através da função `insert()`, a qual precisa ser previamente importada:

```python
from sqlalchemy import insert

ins = insert(cookies).values(
    cookie_name="chocolate chip",
    cookie_recipe_url="http://some.aweso.me/cookie/recipe.html",
    cookie_sku="CC01",
    quantity="12",
    unit_cost="0.50"
)
```

Neste formato, a tabela se torna um parâmetro a ser passado para a função, seguida dos valores a serem inseridos.

#### Passando Valores no Método de Execução

Outro modo de realizar a inserção de dados, é através da chamada direta do método `execute()`, onde o primeiro parâmetro é a tabela e método a ser utilizado, seguida dos valores a serem passados:

```python
ins = cookies.insert()
result = connection.execute(
    ins,
    cookie_name='dark chocolate chip',
    cookie_recipe_url='http://some.aweso.me/cookie/recipe_dark.html',
    cookie_sku='CC02',
    quantity='1',
    unit_cost='0.75'
)

result.inserted_primary_key
```

#### Fazendo Múltiplas Inserções

Também é possível realizar múltiplas inserções através do método anterior, bastando, para isso, passar uma lista com os dicionários de itens a serem inseridos:

```python
inventory_list = [
    {
        'cookie_name': 'peanut butter',
        'cookie_recipe_url':
        'http://some.aweso.me/cookie/peanut.html',
        'cookie_sku': 'PB01',
        'quantity': '24',
        'unit_cost': '0.25'
    },
    {
        'cookie_name': 'oatmeal raisin',
        'cookie_recipe_url':'http://some.okay.me/cookie/raisin.html',
        'cookie_sku': 'EWW01',
        'quantity': '100',
        'unit_cost': '1.00'
    }
]

result = connection.execute(ins, inventory_list)
```

### Consultando Dados

#### Usando a Função `select()`

Para consultar dados no banco, é necessário utilizar a função `select`, que é análoga ao `SQL SELECT`

```python
from sqlalchemy.sql import select

s = select([cookies])
rp = connection.execute(s) # rp significa ResultProxy
results = rp.fetchall()
print(results)
```

Neste caso, ao passar apenas a tabela em que ocorrerá a consulta, todas as colunas são consultadas, semelhantes ao `SELECT * FROM cookies`

O resultado será uma lista com tuplas dos itens encontrados:

```python
[
    (1, u'chocolate chip', u'http://some.aweso.me/cookie/recipe.html', u'CC01', 12, Decimal('0.50')),
    (2, u'dark chocolate chip', u'http://some.aweso.me/cookie/recipe_dark.html', u'CC02', 1, Decimal('0.75')),
    (3, u'peanut butter', u'http://some.aweso.me/cookie/peanut.html', u'PB01', 24, Decimal('0.25')),
    (4, u'oatmeal raisin', u'http://some.okay.me/cookie/raisin.html', u'EWW01', 100, Decimal('1.00'))
]
```

#### Usando o Método `select()`

Enquanto a função `select`, espera uma lista contendo tabelas, o método `select` espera uma lista de colunas a serem consultadas, sendo que caso nenhuma coluna seja especificada, o retorno será todas as colunas da tabela:

```python
from sqlalchemy.sql import select
s = cookies.select()
rp = connection.execute(s)
results = rp.fetchall()
```

Para limitar as colunas a serem retornadas na consulta, basta, informá-las em uma lista na função `select`:

```python
s = select([cookies.c.cookie_name, cookies.c.quantity])
rp = connection.execute(s)
print(rp.keys()) # >>> [u'cookie_name', u'quantity']
result = rp.first() # >>> (u'chocolate chip', 12)
```

### Falando Sobre o ResultProxy

O Proxy de Resultado, ou ResultProxy, é um iterador da consulta realizada, e por isso pode ser acessado de diversas maneiras, no caso abaixo, todos possuem o mesmo efeito e resultado:

```python
first_row = results[0]
first_row[1] # >>> u'chocolate chip'
first_row.cookie_name # >>> u'chocolate chip'
first_row[cookies.c.cookie_name] # >>> u'chocolate chip'
```

Importante lembrar que o ResultProxy é um iterador e, portanto, pode ser utilizado em um laço `for`:

```python
rp = connection.execute(s)
for record in rp:
    print(record.cookie_name)
```

Dessa forma, observa-se que o ResultProxy pode ser tanto ser iterado quanto chamado através de métodos como:

- `fetchall()` retorna todos os valores e fecha a conexão
- `first()` retorna o primeiro valor, se houver um, e fecha a conexão
- `fechone()` retorna uma linha, e deixa o cursor aberto para outras chamadas
- `scalar()` retorna um único valor se a consulta retornar um único registro com uma coluna

Se você quiser consultar as colunas retornadas, utilize o método `keys()` para obter uma lista com seus nomes

**Dicas para uma boa produção de código**

Quando escrevendo um código em produção, siga estas recomendações:

- Use o método `first` para receber um único registro ao invés dos métodos `fetchone` e `scalar`, pois torna tudo mais claro
- Use a versão iterável do `ResultProxy` ao invés dos métodos `fetchall` e `fetchone`. Isso torna o uso da memória mais eficiente e tende a operar em um registro de cada vez
- Evite utilizar o método `fecthone`, pois ele deixa a conexão aberta se você não for cuidadoso
- Use o método `scalar` moderadamente, ele levanta erros se a consulta retornar mais do que uma linha com uma coluna

### Ordenação

Assim como na linguagem SQL é possível fazer a ordenação dos resultados retornados, tanto de forma ascendente, quando descendente:

**Ordenação ascendente**

```python
s = select([cookies.c.cookie_name, cookies.c.quantity])
s = s.order_by(cookies.c.quantity)

# A chamada acima é equivalente ao padrão:
# s = select([...]).order_by(...)
```

**Ordenação descendente**

Para fazer a consulta de forma descendente, é necessário importar a função `desc`:

```python
from sqlalchemy import desc

s = select([cookies.c.cookie_name, cookies.c.quantity])
s = s.order_by(desc(cookies.c.quantity))
```

### Limites

Enquanto métodos como `first()` e `fecthone()` retornam um único registro, o método `limit()` permite que seja definido a quantidade de registro que deverão ser retornados na consulta

```python
s = select([cookies.c.cookie_name, cookies.c.quantity])
s = s.order_by(cookies.c.quantity)
s = s.limit(2) # Limita a dois registros
```

### Funções do SQL e Etiquetas

Há dois métodos da linguagem SQL que são amplamente utilizados, o `SUM()` e o `COUNT()`. Eles também podem ser utilizados no SQL Alchemy, devendo, antes, serem importados através do `func`:

**Exemplo de SUM**

```python
from sqlalchemy.sql import func
s = select([func.sum(cookies.c.quantity)])
rp = connection.execute(s)
# Como é esperado um único item como resposta, usa-se o método "scalar()"
print(rp.scalar()) # >>> 137
```

**Atenção:** Observe que é feita a importação do `func` não do `sum` diretamente para evitar conflitos com a função `sum` do Python

**Exemplo de COUNT**

```python
s = select([func.count(cookies.c.cookie_name)])
rp = connection.execute(s)
record = rp.first()
print(record.keys()) # >>> [u'count_1']
print(record.count_1) # >>> 4
```

Observe que o nome da coluna é gerado automaticamente através do padrão `<func_name>_<position>`

**Exemplo de COUNT com Etiqueta**

Apesar do padrão gerado automaticamente pelo SQL Alchemy, na maioria das vezes, ter a opção de definir o nome da coluna a ser retornada facilita tanto a leitura quanto a execução das demais rotinas. Para isso, basta criar uma etiqueta, `label`, passando como parâmetro nome pelo qual a coluna deverá ser retornada:

```python
s = select([func.count(cookies.c.cookie_name).label('inventory_count')])
rp = connection.execute(s)
record = rp.first()
print(record.keys()) # >>> [u'inventory_count']
print(record.inventory_count) # >>> 4
```

### Filtros

Os filtros são feito através do método `where()`, assim como no SQL. Um `where` tipico possui uma coluna, um operador, e um valor ou coluna. Além disso, é possível encadenar múltiplos `where()` e eles se comportarão como um `AND` em SQL.

```python
s = select([cookies]).where(cookies.c.cookie_name == 'chocolate chip')
rp = connection.execute(s)
record = rp.first()
print(record.items())
```

O comando acima, retorna todos os registro que possuam o nome exatamente igual à "chocolate chip", como pode-se observar no retorno abaixo:

```python
[
    (u'cookie_id', 1),
    (u'cookie_name', u'chocolate chip'),
    (u'cookie_recipe_url', u'http://some.aweso.me/cookie/recipe.html'),
    (u'cookie_sku', u'CC01'),
    (u'quantity', 12),
    (u'unit_cost', Decimal('0.50'))
]
```

Veja abaixo a lista alguns dos possíveis operadores e métodos do `where`:

| Operador | Examplo |
| --- | --- |
| == | statement.where(users.c.name == 'ed')|
| != | statement.where(users.c.name == 'ed')|
| like(string) | statement.where(users.c.name.like('%ed%'))|
| ilike(string) | statement.where(users.c.name.ilike('%ed%'))|
| in_([list]) | statement.where(users.c.name.in_(['ed', 'wnedy', 'jack'])|
| notin\_ | statement.where(users.c.name.notin_(['ed', 'wnedy', 'jack'])|
| is_(None) | statement.where(users.c.name.is(None))|
| match | statement.where(users.c.name.match('wendy'))|
| contains(string) | statement.where(users.c.name.match('end')) |
| endswith(string) | statement.where(users.c.name.match('wen')) |
| startswith(string) | statement.where(users.c.name.match('dy')) |

Um uso comum de operadores é para computar valores de múltiplas colunas. Nesse caso, uma opção útil é a função `cast()`, a qual permite a conversão de valores. No caso do exemplo abaixo, é feita a conversão da multiplicação por um valor numérico de dois dígitos decimais.

```python
from sqlalchemy import cast
s = select([
    cookies.c.cookie_name,
    cast(
        (cookies.c.quantity * cookies.c.unit_cost),
        Numeric(12,2).label('inv_cost')
    )])

for row in connection.execute(s):
    print('{} - {}'.format(row.cookie_name, row.inv_cost))
```

Além desses operadores, há os operadores de conjunção, os quais permitem sejam feitas combinações nos filtros

| Operador | Examplo |
| --- | --- |
| and\_ | statement.where(and_(users.c.name == 'ed', users.c.fullname == 'Ed Jones'))|
| or\_ | statement.where(or_(users.c.name == 'ed', users.c.name == 'wendy'))|
| not\_ | statement.whete(not_(users.c.name != 'ed')) |

### Atualizando Dados

Atualizar dados em SQL Alchemy é muito similar aos demais métodos, de forma que tanto é possível utilizar a função quanto o método `update()`.

```python
from sqlalchemy import update
u = update(cookies).where(cookies.c.cookie_name == 'chocolate chip')
u = u.values(quantity=(cookies.c.quantity + 120))
result = connection.execute(u)
print(result.rowcount)
```

Neste exemplo, a quantia de cookies foi atualizada através da adição de 120 unidades. **Atenção:** Lembre-se que sem o método `where` a atualização será feita em todos os registros da tabela.

### Deletando Dados

Semelhantes aos demais casos do CRUD, é possível fazer a deleção de dados através da função ou do método `delete`.

```python
from sqlalchemy import delete
d = delete(cookies).where(cookies.c.cookie_name == 'dark chocolate chip')
result = connection.execute(d)
print(result.rowcount)
```
