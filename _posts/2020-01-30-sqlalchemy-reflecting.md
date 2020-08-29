---
layout: post
date:  2020-01-30 00:00:00 -0300
title: "Core: Refletindo Tabelas"
slug: "sql-alchemy-essencial"
tag: books
category:  "SQL Alchemy Essencial"
comments: true
---

![Capa do Livro](../assets/img/sql-alchemy-essencial.jpg)

Em algumas situações, temos um banco de dados já existente e queremos que o mesmo seja convertido para o padrão de uso do SQL Alchemy. Para garantir que não seja necessário recriar toda a estrutura, o SQL Alchemy possui o recurso de `reflecting` no qual o Banco já existente é interpretado pela biblioteca. Veja um exemplo:

```python
from sqlalchemy import (
    MetaData, # Responsável por mapear as tabelas
    create_engine # Responsável pela conexão com o Banco
)

metadata = MetaData()
engine = create_engine('sqlite:///data.db')
```

Após a definição do Banco de Dados que será utilizado, pode-se receber a tabela através dos parâmetros `autoload=True` e informando a engine utilizada em `autoload_with`:

```python
from sqlalchemy import Table
artist = Table('Artist', metadata, autoload=True, autoload_with=engine)
```

Neste momento o objeto `artist` funciona da mesma forma que as demais tabelas criadas anteriormente pelo próprio SQL Alchemy.

**Observação** caso as tabelas não seja refletidas ao mesmo tempo, relacionamento de chave-estrangeira não serão carregados, devendo, dessa forma, serem feitos manualmente através do `ForeignKeyConstraint`.

```python
from sqlalchemy import ForeignKeyConstraint
album.append_constraint(
    ForeignKeyConstraint(['ArtistId'], ['artist.ArtistId'])
)
```

### Refletindo Todo o Banco

Para refletir todo o Banco de Dados de uma única vez, basta utilizar o seguinte:

```python
metadata.reflect(bind=engine)
```

Com isso, todas as tabelas estarão contidas em `metadata` e poderão ser acessadas e passadas para variáveis:

```python
metadata.tables.keys() # Exibe a lista de tabelas

mytable = metadata.tables['mytable'] # Passa a tabela para uma variável
```

Uma vez que a tabela foi passada para uma variável. Todos os comandos e recursos anteriormente mencionados se tornam disponíveis.
