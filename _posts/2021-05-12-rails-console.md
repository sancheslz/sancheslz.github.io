---
layout: post
date: 2021-05-12 00:00:00 -0300
title: "Console"
slug: rails console
tag: tutorial
order: 4
category: "Rails"
comments: false
---

## O que é o Console

Uma vez definido o modelo (`model`) responsável pelo registro e controle de informações em nosso Banco de Dados, um passo muito comum é testar o modelo construído e seu comportamento. Esse teste pode ser feito de forma automatizada ou através da interface que criamos para nossa aplicação. Entretanto, para testes rápidos e exploratórios, é comum a utilização do `console`.

O `console` é uma interface de linha de comando que carrega todas as informações do projeto Rails e possui conexão com o Banco de Dados. Dessa forma, a criação e manipulação de dados no sistema torna-se fácil e rápida. Além disso, o `console` é um ótimo lugar para testar e redesenhar as `queries` que farão às consultas dos nossos registros.

Para acessar, basta utilizar o comando:

```console
rails console
```

> **Dica**: É possivel utilizar apenas `rails c` para rodar o console do Rails.

## Criando um Registro

Ao abrir o `console`, todos os `models` registrados no Rails são automaticamente carregados, havendo algumas possibilidades na criação de objetos no Banco de Dados. A primeira opção é criar uma instância do `model`, passar os atributos e em seguidas salvar o registro:

```ruby
book = Book.new
book.title = 'The Hobbit'
book.save()
```

A segunda opção é utilizar o método `create`, passando os atributos para o `model` e, em seguida, utilizar o método `save()`:

```ruby
book = Book.create(title: 'The Hobbit')
book.save()
```

A terceira opção é utilizar o `bang` (`!`). Os métodos com `bang` permitem a criação direta e retorna um erro caso haja algum problema na criação do objeto:

```ruby
Book.create!(title: 'The Hobbit')
```

A quarta opção é a criação do objeto a partir de um bloco `do-end`:

```ruby
book = Book.new do |b|
  b.title = 'The Hobbit'
end
```

### Vários Registros

Caso seja necessário criar vários registros de uma única vez, basta passar uma lista de hashes ao método `create()`

```ruby
Book.create([
  {title: 'The Hobbit'},
  {title: 'The Lord of The Rings'}
])
```

## Consultando um Registro

A realização de consultas no Banco de Dados do Rails pode ser feita facilmente graças aos métodos herdados do `ApplicationRecord`. Através dele diversos métodos são fornecidos para consultar os registros do `model` especificado.

### Todos os Registros

Para acessar todos os registros de um determinado model, basta utilizar o método `all()`:

```ruby
Book.all()
```

### Primeiro e Último Registro

Para acessar o primeiro e último registro de determinado model, basta utilizar os métodos `first()` e `last()`:

```ruby
Book.first()
Book.last()
```

### Registros Específicos

Para consultar registros específicos a partir de determinado valor, como chave-primária ou nome, por exemplo, pode-se utilizar os métodos `find()`, `find_by()` e `where()`. O primeiro método retorna um objeto a partir de seu `id`, já o segundo método, retorna todos o registro a partir de um atributo específico. Por sua vez, o terceiro método retorna um `array` de registros dos campos especificados:

```ruby
Book.find(1)
Book.find_by(name: 'The Hobbit')
Book.where(author: 'Tolkien', printed: true)
```

## Atualizando um Registro

A partir das consultas dos objetos cadastrados, é possível fazer atualizações nos mesmos, por exemplo:

```ruby
book = Book.find_by(name: 'The Robbert')
book.name = 'The Hobbit'
book.save()
```

Também é possível atualizar todos os objetos de uma consulta, através do método `update()`

```ruby
Book.find_by(name: 'The Robbert').update(name: 'The Hobbit')
Book.where(author: 'Tolkiens').update(author: 'Tolkien')
```

## Deletando um Registro

Para deletar um determinado registro, basta utilizar o método `destroy()`:

```ruby
book = Book.find_by(name: 'The Robbert')
book.destroy()
```

Também é possível deleter todos os objetos de uma consulta, bastando encadear o método `destroy()` na consulta realizada. Outra opção é utilizar o método `destroy_by()` passando o atributo buscado como parâmetro:

```ruby
Book.where(author: 'Tolkien').destroy
Book.destroy_by(author: 'Tolkien')
```

## Observações

Ao utilizar o `console` diretamente, será carregado o ambiente em uso (por padrão o de desenvolvimento), contudo, é possível alterar o ambiente a ser utilizado informando a flag `-e`:

```
rails console -e staging
```

O Rails também fornece um ambiente de testes em que todas as alterações criadas serão excluidas ao término, bastando para isso utilizar a flag `--sandbox`:

```
rails console --sandbox
```

## Referências
- [Documentação Oficial: Rails CLI](https://guides.rubyonrails.org/command_line.html#bin-rails-console)
- [Documentação Oficial: Rails CRUD](https://guides.rubyonrails.org/active_record_basics.html#crud-reading-and-writing-data)
