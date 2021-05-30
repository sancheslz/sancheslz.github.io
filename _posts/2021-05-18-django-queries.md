---
layout: post
date: 2021-05-18 00:00:00 -0300
title: "Queries"
slug: django queries
tag: tutorial
order: 5
category: "Django"
comments: false
---


## Queries no Django

Uma vez definido um `model`, o qual será responsável pela comunicação entre a nossa aplicação e o Banco de Dados, é preciso saber como fazer consultas aos nossos registos. Como vimos no uso do `shell`, o Django provê um `manager` chamado `objects` o qual é responsável pela abstração destas consultas.

Este `manager`, proporciona diversos métodos e filtros de consulta, facilitando a tarefa de consultas aos nossos registros. Uma detalhe importante de saber sobre as queries no Django é que todas elas são `lazy`, isto é, a consulta no Banco de Dados, apenas será realizada ao término, de modo que o encadeamento e filtro da consulta pode ser feito de modo encadeado sem que isso afete a performance da aplicação.

## Anatomia Geral da Query

Para realizar uma `query` é necessário que o `model` seja importando para o arquivo em questão. Uma vez importando, basta invocar o `manager` `objects` proporcionado pelo Django e utilizar o método escolhido passando os campos e seus respectivos valores.

```python
# Importação dos Modelos
from application.models import MyModel

# Execução das Queries
MyModel.objects.<method>(<field>=<value>)
```

## Métodos do Manager

### Retornar um Objeto

```python
Book.objects.get(id=1)
# >>> <Book: The Lord of The Rings> 
```

### Retornar Todos os Objetos

```python
Book.objects.all()
# >>> <QuerySet [<Book: The Lord of The Rings>]> 
```

### Retornar com Filtro

```python
Book.objects.filter(name__icontains='The')
# >>> <QuerySet [<Book: The Lord of The Rings>, <Book: The Hobbit>]> 

Book.objects.exclude(name__icontains='The')
# >>> <QuerySet [<Book: Sauron Defeated>]> 
```

### Encadeamento de Filtros

```python
Book.objects.filter(
    author='Tolkien'
).exclude(
    available=True
).filter(
    format='ebook'
)
```

## Limitando e Ordenando o Retorno

Por padrão, o retorno das `queries` no Django ocorrem de acordo com o `id` do objeto. Entretanto, é possível redefinir esta ordem tanto na construção do `model`, quanto na própria consulta, ao invocar o método `order_by`. Além disso, é possível limitar o número de itens retornados fazendo `slices` do mesmo:

```python
Book.objects.all()[0:5]
```

> **Observação:** índices negativos não são permitidos!

```python
Book.objects.filter(pub_date__gt=2010).order_by('month') 
```

## Os Lookups dos Campos

O Django proporciona diversos `lookups`, filtros de pesquisa, os quais abstraem as consultas `SQL`, de modo a retornar objetos a partir de critérios específicos de seus campos. Por exemplo, para buscar todos os itens que tenham exatamente o nome "The Hobbit", o comando `SQL` a ser utilizado seria `SELECT * FROM Book WHERE name="The Hobbit"`. Para fazer essa mesma consulta no Django, através dos seus `lookups`, basta passar o campo seguido de `__exact`: `Book.objects.filter(name__exact="The Hobbit")`.

lookup | definição | exemplo
--- | --- | ---
`exact` | deve ser exatamente igual ao valor informado | `name__exact='Tolkien'`
`iexact` | deve ser igual ao valor informado (_case insensitive_) | `name__exact='tolkien'`
`contains` | deve conter o valor informado | `nome__contains='olk'`
`icontains` | deve conter o valor informado (_case insensitive_) | `name__icontains='olk'`
`in` | deve estar contido na lista informada | `price__in=[30,40,50]`
`gt` | deve ser maior do que o valor informado | `price__gt=30`
`gte` | deve ser maior ou igual ao que o valor informado | `price__gte=30`
`lt` | deve ser menor do que o valor informado | `price__lt=18`
`lte` | deve ser menor ou igual ao que o valor informado | `price__lte=18`
`startswith` | deve iniciar com o valor informado | `name__startswith='To'`
`istartswith` | deve iniciar com o valor informado (_case insensitive_) | `name__istarstwith='Joh'`
`endswith` | deve terminar com o valor informado | `name__endswith='now'`
`iendswith` | deve terminar com o valor informado (_case insensitive_) | `name__iendswith='now'`
`range` | deve estar dentro do intervalor informado | `date__range=('2019-01-01','2019-02-02')`
`year` | deve corresponder ao ano informado | `date__year='2019'`
`month` | deve corresponder ao mês informado | `date__month=3`
`day` | deve corresponder ao dia informado | `date__day=10`
`week` | deve corresponder à semana informada | `date__week=52`
`week_day` | deve corresponder ao dia da semana informado | `date__week_day=2`
`quarter` | deve corresponder ao quadrimestre informado | `date__quarter=2`
`time` | deve corresponder ao horário informado | `date__time=datetime.time(18, 25)`
`hour` | deve corresponder à hora informado | `date__hour=18`
`minute` | deve corresponder ao minuto informado | `date__minute=35`
`second` | deve corresponder ao segundo informado | `date__second=18`
`isnull` | deve ter o valor igual a `null` | `note__isnull=True`
`regex` | deve corresponder a `regex` informada | `name__regex=r'^(An|Ar) +'`
`iregex` | deve corresponder a `regex` informada (_case insensitive_) | `name__iregex=r'^(An|Ar) +'`

## Expressões Avançadas

### F Expressions

As expressões `F` são utilizada para realizar a `query` de um Model em comparação com seu próprio Model, por exemplo:

```python
from django.db.models import F

# Retorna os artigos que tem mais estrelas do que curtidas 
Article.objects.filter(n_stars__gt=F('n_likes'))
```

As operações matemáticas (`+`, `-`, `*`, `/`, `%`, `**`) são permitidas na construção de F expressions.

Por exemplo, para apresentar todos os artigos modificados após três dias da publicação

```python
Article.objects.filter(updated_at__gt=F('pub_date')+timedelta(days=3))
```

### Q Objects

Objetos `Q` são utilizados para realizar consultas múltiplas em que ao invés do "E" lógico, utiliza-se a chamada "Ou" lógico no Banco de Dados. Por exemplo, todos os artigos do Blog que começam com "In" ou "On"

```python
from django.db.models import Q

Article.objects.filter(Q(article__startswith="In")|Q(article__startswith="On"))
```

Os filtros do objetos `Q` podem ser combinados com os seguintes operadores:

- `|` : OU
- `&` : E
- `˜` : Não

### Agregação

```python
from django.db.models import Avg # Média
from django.db.models import Max # Máximo
from django.db.models import Min # Mínimo

# Retorna os artigos com maior data de publicação
Article.objects.aggregate(Max('pub_date'))
```

### Anotação

Retorna um valor pré-definido para cada item da *query*

```python
from django.db.models import Count

q = Book.objects.annotate(n_authors = Count('author'))
q[0].n_authros # 2
```

Note que quando usamos o *annotate*, o parâmetro passado, retorna como campo a ser consultado na `query`.

## Boas Práticas

### Cache

O primeiro ponto a entender sobre o Django é que suas consultas são `lazy`, isso significa que a consulta só é executada quando se tenta extrair alguma informação da consulta. Além disso, ela é mantida em `cache`, de forma que novas consultas no valor em `cache` não farão novas consultas no Banco de Dados:

```python
books = Book.objects.get(id=1) # Gera a query mas não executa a consulta
book.title # Executa a consulta e mantém em cache
book.author # Consulta o valor em cache e não refaz a consulta no Banco de Dados
```

### Valores únicos

Colunas do Banco de Dados definidas como `unique` ou `db_index`, performam de forma mais rápida e eficiente do que outro método de consulta. Por exemplo, a chave-primária de uma tabela é definida para ser única, retornando de forma mais rápida e sem risco de conflitos.

### Retorne Tudo que você precisa

Se você sabe que precisará de algum valor de um relacionamento da tabela, pode utilizar os métodos `select_related` ou `prefetch_related` para retornar os dados em uma única chamada ao Banco de Dados:

```python
books = Book.objects.all()
books.select_related('authors')
```

### Não retorne o que não precisa

Todo campo retornado em uma consulta possui um custo, para reduzí-lo, recomenda-se retornar apenas os campos que realmente serão necessários. Para isso, pode-se utilizar os métodos `values` ou `values_list`

```python
# Retorna apenas os campos título e nome do autor
Book.objects.all().values('title', 'author__name')

# Retorna apenas os valores do registros
Book.objects.all().values_list()
```

Caso deseje saber apenas a quantia de registros ou se o mesmo existe, deve-se utilizar, respectivamente, os métodos `count` e `exists`:

```python
# Retorna o número de registros
Book.objects.filter(author__name__contains='Tolkien').count()

# Retorna True se o objeto existe no Banco de Dados
Book.objects.filter(author__name__contains='Tolkien').exists()
```

### Não ordene objetos desnecessariamente

Realizar a ordenação de objetos de forma desnecessária pode resultar em perda de performance da sua aplicação, caso o `model` tenha uma ordenação definida, mas a mesma não será utilizada, basta utilizar o método `order_by()` sem nenhum parâmetro.

## Comandos SQL

Caso seja necessário, é possível realizar a `query` com comandos SQL através da seguinte estrutura:

```python
Book.objects.raw('SELECT * FROM book_book')
```

Vale lembrar que a tabela, por padrão do Django possui o nome composto por: `appname_modelname`

> **Obs.** apesar da possibilidade, o uso desse recurso é desencorajado pela documentação por proporcionar riscos a segurança do sistema.

## Referências

- [Documentação Oficial: Queries](https://docs.djangoproject.com/en/3.2/topics/db/queries/)
- [Documentação Oficial: Otimização](https://docs.djangoproject.com/en/3.2/topics/db/optimization/)