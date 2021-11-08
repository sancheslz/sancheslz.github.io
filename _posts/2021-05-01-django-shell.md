---
layout: post
date: 2021-05-01 00:00:00 -0300
title: "Shell"
slug: django shell
tag: tutorial
order: 4
category: "Django"
comments: false
---

## O que é Shell

Uma vez definido o modelo (`model`) responsável pelo registro e controle das informações no Banco de Dados, um passo muito comum é testar o modelo construído e seu comportamento. O teste pode ser feito de forma automatizada, pode ser feito através do `admin` do Django, ou pela própria interface que criarmos para nossa aplicação. Entretanto, para testes rápidos e exploratórios, é comum a utilização do `shell`.

O `shell` é uma interface de linha de comando que carrega todas as informações do projeto Django e possui conexão com o Banco de Dados. Dessa forma, a criação e manipulação de dados no sistema torna-se fácil e rápida. Além disso, o `shell` é um ótimo lugar para testar e redesenhar as `queries` que farão às consultas dos nossos registros.

Para acessar, basta utilizar o comando:

```
manage.py shell
```

## Criando um Registro

Para criar qualquer registro no Banco de Dados, primeiro é necessário importar o `model` a ser utilizado. Para isso, execute o comando:

```
from <app_name>.models import <ModelName>
```

Importado o `model` há alguns caminhos possíveis para a criação de um novo objeto:

A primeira opção é criar uma instância do model, passar cada um dos atributos individualmente e depois salvar:

```python
from book.models import Book

got = Book()
got.title = 'Game of Thrones'
got.save()
```

Outra opção é instanciar o model, já atribuindo os atributos e encadear o método `save()`:

```python
from book.models import Book

Book(title='Game of Thrones').save()
```

Por fim, há a opção do utilizar o método `create` do `Manager` do Django, responsável por criar e salvar o registro:

```python
from book.models import Book

Book.objects.create(title='Game of Thrones')
```

### Vários Registros

Caso seja necessário criar vários registros de uma única vez, o método `bulk_create()` deve ser utilizado no lugar do `create()`. Este método realiza apenas uma chamada no Banco de Dados, sendo, portanto, mais performático. Além disso, ele recebe um lista de objetos a serem criados:

```python
from book.models import Book

Book.objects.bulk_create([
    Book(title='The Hobbit'),
    Bookt(title='The Lord of The Rings')
])
```

## Criando Objetos Relacionais

### ForeignKey

Nos casos de relacionamento do tipo `ForeignKey`, deve ser passado como atributo ao `model` a referência do objeto da chave-estrangeira:

```python
from book.models import Book, Author

tolkien = Author(name='J.R.R. Tolkien')
tolkien.save()

lotr = Book()
lort.title = 'The Lord of The Rings'
lort.author = tolkien
lotr.save()
```

### ManyToMany

Já nos casos de relacionamento do tipo `ManyToMany`, o relacionamento deve ser passado através do método `add()`, passando como argumento os objetos a serem relacionados:

```python
from book.models import Book, Author

bob = Author(name='Bob')
bob.save()

john = Author(name='John')
john.save()

silly = Book(lort.title='A Silly Book')
silly.author.add(bob, john)
silly.save()
```

## Consultando um Registro

A realização de consultas no Banco de Dados do Django pode ser feita facilmente graças ao `Manager` do seu ORM, que fornece o método `objects`. Através dele diversos métodos são fornecidos para consultar os registros do `model` especificado.

### Todos os Registros

Para acessar todos os registros de um determinado `model`, basta utilizar o método `all()`:

```python
from book.models import Book, Author

Book.objects.all()
```

### Primeiro e Último Registro

Para acessar o primeiro e último registro de determinado `model`, basta utilizar os métodos `first()` e `last()`:

```python
from book.models import Book, Author

Book.objects.first()
Author.objects.last()
```

### Registros Específicos

Para consultar registros específicos a partir de determinado valor, como chave-primária ou nome, por exemplo, pode-se utilizar os métodos `get()` e `filter()`. Enquanto o primeiro método retorna apenas um objeto que satisfaça a consulta, o segundo retorna uma lista (`QuerySet`) com todos os objetos que satisfaçam o padrão consultado:

```python
from book.models import Book, Author

Author.objects.get(name='Tolkien')
Book.objects.filter(year=2009)
```

## Atualizando um Registro

A partir das consultas dos objetos cadastrados, é possível fazer atualizações nos mesmos, por exemplo:

```python
from book.models import Book, Author

book = Book.objects.last()
book.available = False
book.save()
```

Também é possível atualizar todos os objetos de um `QuerySet`, através do método `update()`

```python
from book.models import Book, Author

Book.objects.filter(year=2012).update(available=False)
```

## Deletando um Registro

Para deletar um determinado registro, basta utilizar o método `delete()`:

```python
from book.models import Book

book = Book.objects.last()
book.delete()
```

Também é possível deletar todos os objetos de um `QuerySet`, bastando encadear o método `delete()` na consulta realizada:

```python
from book.models import Book

Book.objects.filter(year=1950).delete()
```

## Observações

O `shell` do Django utiliza o ambiente (`settings`) padrão definido pelo framework. Dessa forma, as alterações e modificações realizadas através do `shell` serão refletidas no Banco de Dados definido no arquivo de configurações. Para evitar esse comportamento e realizar as operações em um ambiente de testes isolado, há duas opções:

### Configurando um ambiente de teste no Shell

```python
# Setup the environment
from django import test
test.utils.setup_test_environment() 

# Create the test db
from django.db import connection
db = connection.creation.create_test_db() 

# Destroy the test db
connection.creation.destroy_test_db(db)

# Teardown the test env 
test.utils.teardown_test_environment() 
```

### Criando um ambiente para testes

Crie um arquivo com as configurações de teste, semelhantes do `settings.py` e defina o Banco de Dados que será utilizado nos testes. Em seguida, execute os comandos do `manage.py` informando o ambiente escolhido. Lembre-se que será fazer a migração neste novo ambiente:

```console
manage.py migrate --settings=<project_name>.<settings_file_name>
manage.py shell --settings=<project_name>.<settings_file_name>
```

## Referências

- [Documentação Oficial: Queries](https://docs.djangoproject.com/en/3.2/topics/db/queries/)
- [Documentação Oficial: QuerySet](https://docs.djangoproject.com/en/3.2/ref/models/querysets/)
