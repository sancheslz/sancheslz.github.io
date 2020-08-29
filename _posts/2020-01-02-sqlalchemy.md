---
layout: post
title:  "Introdução"
date:   2020-01-02 23:06:28 -0300
slug: "sql-alchemy-essencial introducao"
tags: "books"
category: "SQL Alchemy Essencial"
comments: true
---

O Livro **Essential SQLAlchemy** (tradução livre: *SQLAlchemy Essencial*), segunda edição, dos autores Jason Myers e Rick Copeland, foi publicado em dezembro de 2015, pela O'Reilly - sem publicação em português. Contém cerca de 200 páginas, abordando os conceitos essenciais para uso da biblioteca Python "SQLAlchemy" ([link](https://www.sqlalchemy.org/) da documentação).

Dividida em três partes: o SQLAlchemy Core, o SQLAlchemy ORM e o Alembic, a obra apresenta exemplos de criação e manipulação de Banco de Dados através da biblioteca. Seu aspecto prático torna a aprendizagem eficiênte frente a brevidade com que alguns assuntos são tratados. Uma obra interessante e que vale a pena ser adquirida.

**Mas afinal, o que é o SQLAlchemy?**

O SQLAlchemy é uma abstração Python para a utilização de bancos de dados relacionais que possui como base a sintaxe SQL. Segundo os autores, a biblioteca *"encoraja um pensamento de alto nível através de uma abordagem mais "Pythonica" e amigável"*. Isso porque, através da biblioteca, os comandos SQL podem facilmente ser encadeados e combinados através da sintaxe Python.

Por exemplo, para buscar todos os itens em uma determinada tabela do Banco de Dados, é preciso executar o seguinte comando SQL:

```sql
SELECT * FROM books
```

Já com SQL Alchemy (Core), bastaria digitar:

```python
select([books])
```

Já o SQLAlchemy ORM é similar ao padrão utilizados por outros mapeadores de objetos relacionais (como o Django ORM), por isso, é necessário pensar bem quando escolher um ou outro.

Os autores trazem a seguinte sugestão:

**Qual abordagem escolher?**

- Se você está trabalhando com o *framework* que há possui um ORM embutido, mas quer adicionar relatórios mais poderosos, use Core
- Se você quer ver seus arquivos em um padrão centrado em esquemas (como usado no SQL), use Core
- Se você tem informações cujos objetos do negócio não dependem, use Core
- Se você visualiza as informações como objetos do negócio, use ORM
- Se você está construindo um protótipo rápido, use ORM
- Se você tem uma combinação de necessidades que realmente poderia alavancar os objetos do negócio e outros dados não relacionados ao domínio do problema, use ambos!

### Como usar?

A instalação do SQLAlchemy é similar a de qualquer outra biblioteca Python, basta acessar o terminal, configurar seu ambiente virtual e digitar:

```python
pip install sqlalchemy
```

Uma vez instalado, basta configurar o tipo de engine (motor de banco de dados) que deseja utilizar e utilizar o seguinte padrão:

```python
engine+driver://username:password@host:port/database
```

Veja os exemplos para três diferentes motores:

**SQLite**
```python
engine = create_engine('sqlite:///data2.db')
```

**PostgreSQL**
```python
engine = create_engine('postgresql+psycopg2://scott:tiger@localhost/mydatabase')
```

**MySQL**
```python
engine = create_engine('mysql+pymysql://scott:tiger@localhost/foo')
```

Alguns parâmetros opcionais que podem ser passados no `create_engine`:

- `echo`: retorna o log das ações processadas pelo banco, sendo o padrão `False`
- `encoding`: define a codificação das strings utilizadas no banco. O padrão é `utf-8`
- `isolation_level`: define o tipo de acesso do banco, útil tanto para o PostgreSQL quanto para o MySQL. (Veja a documentação para maiores detalhes).
- `pool_recyle`: intervalo regular de reconexão do banco de dados em segundos.

Para maiores informações sobre a forma correta de definir o banco de dados escolhido, acesse o site da biblioteca em [SQLAlchemy: Engines](https://docs.sqlalchemy.org/en/14/core/engines.html)
