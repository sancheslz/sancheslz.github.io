---
layout: post
date:  2020-01-09 00:00:00 -0300
title:  "Core: Criando Tabelas"
slug: "sql-alchemy-essencial core criando tabelas"
tag: books
category:  "SQL Alchemy Essencial"
comments: true
---

![Capa do Livro](../assets/img/sql-alchemy-essencial.jpg)

### Os Tipos de Dados

A primeira coisa à conhecer ao utilizar o SQLAlchemy é a sua representação genérica dos tipos de dados. A Tabela abaixo demonstra algumas dessas representações:

| SQL Alchemy | Python | SQL |
| --- | --- | --- |
| BigInteger | int | BIGINT |
| Boolean | bool | BOOLEAN or SMALLINT
| Date | datetime.date | DATE (SQLite: STRING)
| DateTime | datetime.datetime | DATETIME (SQLite: STRING)
| Enum | str | ENUM ou VARCHAR
| Float | float ou Decimal | FLOAT ou REAL
| Integer | int | INTEGER
| Interval | datetime.timedelta | INTERVAL or DATE from epoch
| LargeBinary | byte | BLOB ou BYTEA
| Numeric | decimal.Decimal | NUMERIC ou DECIMAL
| Unicode | unicode | UNICODE ou VARCHAR
| Text | str | CLOB ou TEXT
| Time | datetime.time | DATETIME

Conhecer os tipos de dados permite que você modele corretamente o Banco de Dados não apenas de sua estrutura interna, mas também com as relações que o mesmo fará com seus sistema Python.

### Metadados

Outro recurso importante do SQLAlchemy são os metadados. Eles permitem o acesso e a rápida manipulação, do Banco de Dados. Pode ser útil pensar nos metadados como um catálogo de objetos do tipo `Table` com informações opcionais sobre a engine e a conexão.

Para criar um objeto do tipo MetaData, é preciso importar e instanciar o mesmo da biblioteca:

```python
from sqlalchemy import MetaData
metadata = MetaData()
```

Este objeto será o responsável por controlar o acesso de todas as tabelas do Banco de Dados. Note que as tabelas podem ser acessadas por um dicionário, `MetaData.tables`.

### Tabelas

Os objetos do tipo `Table` são inicializados no SQL Alchemy Core a partir de um objeto `MetaData` o qual é responsável por invocar o construtor do objeto `Table`, tendo o seguinte padrão:

```python
Table('nome_da_tabela', metadata,
    ... # colunas da tabela e suas definições
)
```

### Colunas

Dentro da tabela, são informadas as colunas, através do objeto `Column`, as quais deverão conter o nome do campo, o tipo do dado e demais parâmetros adicionais. Observe a sintaxe básica:

```python
Column('coluna_id', Integer(), primary_key=True)
```

Dessa maneira, a construção de uma tabela pode ser feita da seguinte forma:

```python
from sqlalchemy import (
    Table, Column,
    Integer, Numeric, String, ForeignKey
)

cookies = Table(
    'cookies', metadata,
    Column('cookie_id', Integer(), primary_key=True),
    Column('cookie_name', String(50), index=True),
    Column('cookie_recipe_url', String(255)),
    Column('cookie_sku', String(55)),
    Column('quantity', Integer()),
    Column('unit_cost', Numeric(12, 2)),
)
```

Note que:

- `cookie_id`, foi definido como a chave primária da tabela
- `cookie_name` foi definido como um índice da tabela, o que faz as buscas por essa coluna serem mais rápidas

Além disso, é possível definir a obrigatoriedade dos campos e os valores a serem utilizados por padrão caso nenhum valor seja passado:

```python
from datetime import datetime
from sqlalchemy import DateTime

users = Table(
    'users', medatada,
    Column('user_id', Integer(), primary_key=True),
    Column('username', String(15), nullable=False, unique=True),
    Column('email_address', String(255), nullable=False),
    Column('phone', String(20), nullable=False),
    Column('password', String(25), nullable=False),
    Column('created_on', DateTime(), default=datetime.now),
    Column('updated_on', DateTime(), default=datetime.now, onupdate=datetime.now)
)
```

Note que:

- `nullable=False` determina que não pode haver valor nulo para o campo, isto é, este campo é obrigatório
- `unique=True` pode haver apenas um valor como o informado
- `default=datetime.now` caso um horário não seja informado, será usado o horário atual.
- `onupdate=datetime.now` o valor desta coluna será reiniciado toda vez que parte deste registro for alterado

Observe que não está sendo feita a chamada da função `datetime.now()`, pois isso definiria como padrão o horário de criação da tabela. Sem invocar a função, a mesma será executada a cada novo registro.

### Chaves e Restrições

Chaves e Restrições servem para garantir que um determinado registro satisfaça certos requisitos antes de ser armazenado no Banco de Dados. Abaixo, há três dos mais comumente utilizados:

```python
from sqlalchemy import (
    PrimaryKeyConstraint,
    UniqueConstraint,
    CheckConstraint
)
```

**PrimaryKeyConstraint**

É possível definir a chave-primária através da passagem de um atributo na definição da coluna, no formato `primary_key=True`, ou através da `PrimaryKeyConstraint`, passando como elemento(s) a(s) coluna(s) que será(ão) usada(s) para definir a chave primária da tabela. Após, é necessário informar o nome pelo qual essa chave será referênciada em futuras buscas.

```python
PrimaryKeyConstraint('user_id', name='user_pk')
```

```python
PrimaryKeyConstraint('user_id','username', name='user_key')
```

**UniqueConstraint**

Para garantir que um valor seja exclusivo no Banco de Dados, é preciso defini-lo como único. Para isso, pode-se passar o atributo `unique=True`, na criação da coluna, ou através da `UniqueConstraint`. Podendo ser passada uma ou mais colunas de uma vez, sendo a definição do nome opcional.

```python
UniqueConstraint('username', name='uix_username')
```

**CheckConstraint**

Para garantir que o valor passado satisfaça à uma determinada restrição, deve-ser usar o `CheckConstraint`, passando a verificação a ser realizada e o nome desta verificação, podendo ser feito de duas formas:

Na definição da Coluna

```python
Column('quantity', Integer(), CheckConstraint('quantity >= 0'))
```

Após a definição das Colunas

```python
CheckConstraint('quantity >= 0', name='quantity_positive')
```

**Índices**

Os índices são úteis para acelerar a busca por valores em um determinado campo ou conjunto de campos. Sendo o primeiro atributo o nome do índice, e os demais os valores que o compõe. Por exemplo:

```python
from sqlalchemy import Index
Index('ix_cookies_cookie_name', 'cookie_name')
```

É possível fazer tanto no formado de string, quanto através da passagem de variáveis:

```python
Index('ix_test', mytable.c.cookie_sku, mytable.c.cookie_name)
```

### Relacionamentos e Restrições de Chave Estrangeira

As chaves estrangeiras podem ser definidas de duas maneiras, ou diretamente na definição da coluna, ou através do `ForeignKeyConstraint`. No caso da segunda opção, é possível definir diversas chaves em uma única constraint, bastando passar a listagem de itens, sendo o primeiro a referência à própria tabela, e o seguindo a origem da informação.

Na definição da Coluna

```python
orders = Table(
    'orders', metadata,
    ...
    Column('user_id', ForeignKey('user.user_id')),
)
```

Após a definição das Colunas

```python
ForeignKeyConstraint(['user_id'], ['users.user_id'])
```

Note que no caso da chave-estrangeira, foram utilizadas strings como referência às tabelas e não objetos. Essa é uma boa prática, pois garante que as tabelas sejam definidas mesmo estando em momentos/arquivos diferentes sem que haja conflito entre eles quanto a importação e dependência.

### Criação das Tabelas

Uma vez que todas as tabelas foram definidas e associadas à uma instância de `metadata`, para criar as mesmas no banco de dados, basta invocar o método `create_all()` passando como argumento e engine que será utilizada:

```python
metadata.create_all(engine)
```

**Observação:** o método `create_all`, não tentará recriar tabelas já existentes, de forma que é seguro executar este comando outras vezes.
