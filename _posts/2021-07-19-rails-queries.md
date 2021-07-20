---
layout: post
date: 2021-06-19 00:00:00 -0300
title: "Queries"
slug: rails queries
tag: tutorial
order: 5
category: "Rails"
comments: false
---

## Queries no Rails

Uma vez definido um `model`, o qual será responsável pela comunicação entre a nossa aplicação e o Banco de Dados, é preciso saber como fazer consultas aos nossos registos. Como vimos no uso do `console`, o Rails provê um conjunto de métodos que facilitam a realização de consultas dos nossos registros, graças ao `ActiveRecord`.

## Anatomia Geral da Query

```ruby
MyModel.<method>(<field>: <value>)
```

## Métodos de Consultas

### Retornar um Objeto

```ruby
Book.find(1)
# => #<Book id: 1, title: "The Lord of The Rings"> 
```

### Retornar Todos os Objetos

```ruby
Book.all()
# => #<ActiveRecord::Relation [#<Book id: 1, title: "The Lord of The Rings">, #<Book id:2, title: "The Hobbit">]>
```

### Retornar com Filtro

```ruby
tolkien = Author.find_by(name: 'Tolkien')
# O Método acima é equivalente a usar:
# Author.where(name: 'Tolkien').take

Book.where(author: tolkien)
# => #<ActiveRecord::Relation [#<Book id: 1, title: "The Lord of The Rings">, #<Book id:2, title: "The Hobbit">]>

Book.where.not(author: tolkien)
# => #<ActiveRecord::Relation [#<Book id: 1, title: "Narnia">]>
```

### Encadeamento de Filtros

```ruby
tolkien = Author.find_by(name: 'Tolkien')

Book.where(
    author: tolkien
).where.not(
    title: 'The Hobbit'
)
# => #<ActiveRecord::Relation [#<Book id: 1, title: "The Lord of The Rings">]>
```

> No método `where`, além da consulta através de condicionais com `hash`, também é possível realizar consultas com `SQL`

## Limitando e Ordenando o Retorno

Uma boa prática, principalmente em Bancos com muitos registros, é limitar a quantia de valores retornados a cada vez, para isso, existem dois métodos que podem ser utilizados conjuntamente:

- `limit`: determina quantos registros retornarão da consulta
- `offset`: determina índice de início da consulta

```ruby
Book.all.limit(2).offset(10)
```

Para order os valores retornados a partir de algum campos específico, basta utilizar o método `order` informando o campo desejado:

```ruby
Book.all.order(:created_at)
```

## Valores Condicionais

Para consultas com condicionais, a melhor forma é utilizar o marcado `?`, de modo a evitar injeções de depedência. Passando, como primeiro parâmetro a construção da consulta e, posteriormente, os valores a serem substituídos:

```ruby
Book.where("title = ? available = ?", params[:q], false)
```

Também é possível utilizar `placeholders` na construção das consultas:

```ruby
Book.where("title = :book_name", { book_name: params[:q] })
```

Em situações como datas, usar intervalos (`ranges`), pode ser uma ótima opção:

```ruby
Book.where(created_at: (Time.now.midnight - 1.day.ago)..(Time.now.midnight))
```

Subconjuntos também podem ser validados:

```ruby
Book.where(price: [10, 20, 30])
```

Para consultas do tipo `contém`, lembre-se de fazê-las de forma segura, sempre separando a consulta em si de seus valores:

```ruby
Book.where("title like ?", "%#{params[:name]}%")
```

## Enumerações

Quando criadas enumerações, o Rails, automaticamente, cria métodos adicionais que permitem validar, consultar e atualizar o valor da enumeração criada:

```ruby
class Task < ApplicationRecord
    enum status: [:todo, :doing, :done]
end

# Consulta todos com o status `done`
Book.todo

# Consulta todos com o status diferente de `done`
Book.not_todo

# Retorna um booleano informando se possui o status `done`
Book.first.done?

# Atualizar o item para `done`
Book.first.done!
```

## Consultas Avançadas

### Operadores Lógicas

- `AND` : `Book.where(title: 'The Hobbit').or(Book.where(author: tolkien))`
- `OR` : `Book.where(title: 'The Hobbit').and(Book.where(author: tolkien))`
- `NOT` : `Book.where(title: 'The Hobbit').or(Book.where.not(author: tolkien))`

### Agregação

```ruby
Book.count(:price)
Book.average(:price)
Book.minimum(:price)
Book.maximum(:price)
Book.sum(:price)
```

## Boas Práticas

### Retornando em Lotes

Em situações em que um conjunto grande de valores deverão ser utilizados, é recomendado fazer essa ação em lotes. No exemplo abaixo, deseja-se enviar e-mail para todos os clientes, para isso, é utilizado o métood `find_each` que gerencia essa realização em lotes, de forma a não sobrecarregar às consultas ao Banco de Dados.

```ruby
Customer.find_each(start: 1, finish: 1000, batch_size: 50) do |customer|
    NewsMailer(customer).deliver_now
end
```

Observer que há três parâmetros possíveis:

- `start`: índice de início da ação
- `finish`: índice de fim da ação
- `batch_size`: número de elementos de cada conjunto de ação

### Limitando os campos

Para otimizar as consultas ao Banco de Dados, busque, sempre que possível, retornar apenas o campos que realmente serão necessários, para isso, basta utilizar o método `select`:

```ruby
Book.where(author: tolkien).select(:title)
```

Caso deseja retornar apenas algunas poucas colunas do Banco de Dados, utilize o método `pluck`:

```ruby
Book.pluck(:title)
# => ["The Lord of The Rings", "The Hobbit"]
```

Se precisar listar todos os ids existentes, utilize o método `ids`:

```ruby
Book.ids
# => [1, 2, 3]
```

Se a consulta não busca retornar valores, mas apenas consultar a existência do registro, utilize o método `exists?`:

```ruby
Book.exists?(id: [1,2,3])
# => true
```

## Comandos SQL

Também é possível realizar consultas diretamente através de comandos `SQL`, como no exemplo abaixo:

```ruby
Book.find_by_sql("SELECT * FROM books")
```

## Referências
- [Documentação Oficial: Rails Query](https://guides.rubyonrails.org/active_record_querying.html)
- [Rails API: Rails Query](https://api.rubyonrails.org/v6.1.0/classes/ActiveRecord/QueryMethods.html)
