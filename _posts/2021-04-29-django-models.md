---
layout: post
date: 2021-04-29 00:00:00 -0300
title: "Models"
slug: django models
tag: tutorial
order: 3
category: "Django"
comments: false
---

## Models no Django

Conforme apresentado na [Visão Geral](https://sancheslz.github.io/django-intro), todas as requisições do Cliente serão tratadas pelo gerenciador de rotas do Django que fará o redirecionamento para as `views` as quais, conforme suas instruções fará consultas aos `models`. A criação de `models` no Django ocorrem de forma rápida, simples e elegante e graças à seu ORM, a criação e manipulação do Banco de Dados é automaticamente vinculada e coordenada com a sua aplicação.

## Anatomia Geral do Model

Para criar um `model` no Django basta criar uma classe Python que herde da classe `Models`, disponível em `django.db.models`:

```python
from django.db import models 

class MyModel(models.Model):
    # Definição do Campo a ser criado no Banco de Dados
    my_field = models.FieldType(parameter=value)
    
    # Define o que será exibido na chamada do model
    def __str__(self):
        return self.my_field
    
    # Permite passar meta-parâmetros para o model
    # como regras de ordenação, etc.
    class Meta:
        pass
```

## Criando Campos

Os campos (_fields_) possuem comportamento e características diferentes que influenciarão na sua construção no Banco de Dados, bem como sua renderização em tela. Por isso a escolha correta do tipo de campo é fundamental na criação de um bom projeto Django.

### Parâmetros dos Campos

Além dos tipos de campo, também é possível atribuir parâmetros para os campos, tornando-os ainda mais personalizáveis e funcionais.

- `choice: tuple`: recebe uma tupla de tuplas onde o primeiro valor corresponde ao valor a ser registrado no Banco de Dados e o segundo o valor a ser apresentado nas consultas.
- `null: bool`: recebe um valor booleano, definindo se o campo poderá ou não receber um valor que seja nulo
- `blank: bool`: recebe um valor booleano, definindo se o campo poderá ou não ser deixado em branco, sendo o padrão `""`
- `default: any`: recebe qualquer tipo de valor a ser utilizado como valor padrão para o campo caso nenhum outro valor seja informado
- `help_text: str`: recebe uma string com o texto de ajuda de preenchimento a ser exibido para o usuário
- `error_message: str`: recebe uma string com a mensagem a ser exibida ao usuário em caso de erro
- `primary_key: bool`: recebe um valor booleando determinando se o campo deverá ou não ser utilizado como chave-primária
- `unique: bool`: recebe um valor booleano definindo se o valor do campo deverá ser único no tabela do Banco de Dados
- `unique_for_date: bool`: recebe um valor booleano definindo se poderá haver mais de uma ocorrência do mesmo valor para uma determinada data
- `unique_for_month: bool`: recebe um valor booleano definindo se poderá haver mais de uma ocorrência do mesmo valor para um mesmo mês
- `unique_for_year: bool`: recebe um valor booleano definindo se poderá haver mais de uma ocorrência do mesmo valor para um mesmo ano
- `verbose_name: str`: recebe uma string com o valor que será utilizado na exibição do campo na renderização em tela
- `validators: list`: recebe uma lista de validadores que deverão ser executados antes de salvar o objeto, para isso, primeiro crie uma função validadora (`def validador (value): pass`), em seguida passe a mesma como valor da lista `validators=[validador]`

## Tipos de Campos

Formato | Parâmetros Obrigatórios | Descrição
--- | --- | ---
`models.AutoField()` | - | Campo de auto incremento, como chave primária.
`models.BigAutoField()` | - | Preenche números automaticamente de 1 a 9223372036854775807
`models.BigIntegerField()` | - | Gera números inteiros de -9223372036854775808 a 9223372036854775807
`models.BinaryField()` | `max_length=int` | Armazena dados binários como bytes
`models.BooleanField()` | - | Armazena dados Booleanos: `True` ou `False`
`models.CharField()` | `max_length=int` | Armazena caracteres em formado texto
`models.DateField()` | `auto_now_add=bool` e `auto_now=bool` | Campo em formato data, gera o mini calendário na interface. `auto_now_add` é a data de criação e `auto_now` é a data de modificação
`models.DateTimeField()` | `auto_now_add=bool` e `auto_now=bool` | Armazena a Data e a Hora. `auto_now_add` é a data de criação e `auto_now` é a data de modificação
`models.DecimalField()` | `max_digts=int` e `decimal_places=int` | Armazeda dados decimais, em formato texto. `max_digits` corresponde ao número máximo de dígitos permitidos, incluindo os depois da víngula e `decimal_places` determina o número de casas decimais depois da víngula
`models.DurationField()` | - | Armazena a duração em intervalos de dias e segundos
`models.EmailField()` | - | Armazena uma string de email e faz a validação se a string corresponde à um e-mail
`models.FileField()` | `upload_to=str` e `max_length=int` | Campo para upload de arquivos. Caso nenhum caminho seja passado, os arquivos serão salvos no `root`.
`models.FloatField()` | - | Armazena um dado do tipo float
`models.ImageField()` | `upload_to=str`, `height_field=int`, `width_field=int`, `max_length=int)`  | Armazena uma imagem
`models.IntegerField()` | - | Armazena valores do tipo inteiro
`models.NullBooleanField()` | - | Campo Booleano que quando `Null` é igual a `True`
`models.PositiveIntegerField()` | - | Valores inteiros positivos de 0 a 2147483647
`models.PositiveSmallIntegerField()` | - | Valores inteiros positivos de 0 a 32767
`models.SlugField()` | `max_length=int`, `allow_unicode=bool` | Armazena um slug-name
`models.SmallIntegerField()` | - | Valores inteiros de  -32768 a 32767
`models.TextField()` | - | Valores de texto de tamanho indefinidos
`models.TimeField()` | `auto_now=bool`, `auto_now_add=bool` | Armazena dados do tipo tempo
`models.URLField()` | `max_length=int)` | Armazena dados do tipo URL
`models.UUIDField()` | - | Arnazena um identificador único

### Campos de Relacionamento

#### ManyToManyField

Armazena relações `n->n`

```python
models.ManyToManyField('references')
```

#### ForeignKey

Armazena uma chave estrangeira `1->n`:

```python
models.ForeignKey(
    'references',
    on_delete=arg
) 
```

#### OneToOneField

Armazena relações `1->1`

```python
models.OneToOneField(
    'references',
    on_delete=arg
)
```

Os tipos `ForeignKey` e `OneToOneField` deve, obrigatoriamente receber um método `on_delete`, que pode ser dos seguintes tipos:

- `models.CASCADE`: elimina os relacionamentos em forma de cascata
- `models.PROTECT`: proibe a deleção se houver algum relacionamento no Banco de Dados
- `models.RESTRICT`: não permite a deleção direta em caso de relacionamentos, mas permite se fizer parte de um relacionamento com `on_delete`  do tipo `CASCADE`
- `models.SET_NULL`: transforma a chave-estrangeira em `null`. Funciona apenas se `null` estiver definido como `True`
- `models.SET_DEFAULT`: transforma a chave-estrangeira em seu valor `default`. Funciona apenas se o valor `default` houver sido definido para o campo.
- `models.SET`: recebe uma função que retornará o valor a ser utilizado no lugar em caso de deleção
- `models.DO_NOTHING`: instrui o Django a não tomar nenhuma ação. Atenção: Isso pode causar erro de integridade no Banco de Dados!

#### Parâmetros Adicionais

Os campos relacionais podem recebe alguns parâmetros opcionais, tais como:

- `limit_choices_to: object`: permite que seja indicado uma restrição aos valores que podem ser usados no relacionamento, por exemplo, limitando que apenas objetos com `{field_name: field_validation}` sejam utilizados
- `related_name: str`: define o nome que será utilizado na chamada reversa do objeto (Veja mais em [Queries](https://sancheslz.github.io/django-queries))
- `to_field: str`: define um campo diferente do `pk` ou `id` para realizar os relacionamentos. O campo indicado deve ter o parâmetro `unique=True`

> Importante notar que o `limit_choices_to` retorna um objeto `Q`, permitindo a construção de consultas complexas. Veja mais em [Queries](https://sancheslz.github.io/django-queries)

#### ManyToMany: throught

O Django automaticamente cria uma tabela para relacionar objetos do tipo `many-to-many`, contudo, caso queira criar uma tabela personalizada para essa relação, basta passar esse modelo para o parâmetro `throught`:

```python
class Person(models.Model):
    name = models.CharField(max_length=100)

class Group(models.Model):
    name = models.CharField(max_length=50)
    members = models.ManyToManyField(Person, throught='Membership')

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    position = models.CharField(max_length=100)
```

## Classe Meta

Como pôde ser visto na [Anatomia Gerald do Model](#anatomia-geral-do-model), a classe `Meta`, permite a passagem de meta-parâmetros, que cooperam com a definição do `model`, podendo receber como parâmetros:

- `abstract: bool`: Define se a classe é uma classe abstrata (apenas modelo de estrutura), ou se é uma classe concreta que deverá ser criada no Banco de Dados.
- `app_label: str`: Caso `model` seja definido fora de um *app*, é possível atribuí-lo ao mesma utilizando o parâmetro `app_label`
- `base_manager_name: str`: Define o nome do *manager* do `model`, o padrão seu nome é `objects`
- `get_latest_by: [str,list]`: Define o parâmetro que será utilizado na definição do último (`latest()`) e do primeiro registro (`earlest()`)
- `order_with_respect_to: str`: Utilizado quando possui-se chaves estrangeiras, geralmente `ForeignKey`, permite que se ordene com base em algum critério/campo do Modelo-Pai
- `ordering: list[str]`: Define a ordenação de retorno das informações do Banco de Dados, conforme a lista de campos especificados. **Obs**. Também é possível realizar a ordenação com base em *query expressions*: `ordering = [F('author').asc(nulls_last=True)]`
- `permissions = [('permission_code', 'Human Message')]`: Além das funções de CRUD (padrão), permite criar um tipo específico de permissão, sendo o primeiro parâmetro da tupla o código da permissão e o segundo o nome legível para humanos
- `unique_together: [list]`: Recebe uma lista de listas, de forma que os campos combinados, devem ser únicos.
- `verbose_name: str`: Define o nome a ser exibido para o usuário quando da chamada do `model`
- `verbose_name_plural: str`: Define o plural do nome do `model`. Por padrão é feita a adição de 's' ao término da palavra

## Reescrita do Método Save()

Caso seja necessário realizar algum tipo de verificação e/ou validação com o método `save()`, é possível reescrever o método padrão e adicionar mecanismos de controle ao mesmo. Após, basta invocá-lo com o método `super()`:

```python
class MyModel(models.Model):
    # ...
    
    def save(self, *args, **kwargs):
        if self.field == True:
            return
        else:
            super().save(*args, **kwargs)
```

### Sinais

Um recurso extremamente poderoso e delicado dos Models é o uso de sinais (`signals`), com eles, ações podem ser executadas antes ou depois da inicialização, criação ou deleção de um objeto. Podendo ser:

- `pre_init`: executando antes da chamado do método `__init__()`
- `post_init`: executado após a chamada do método `__init__()`
- `pre_save`: executado antes da chamada do método `save()`
- `post_save`: executado após a chamada do método `save()`
- `pre_delete`: executado antes da chamada do método `delete()`
- `post_delete`: executado após a chamada do método `delete()`

Exemplo:

```python
from django.db import models
from django.db.models.signals import pre_save

# Função que será executada quando o sinal ocorrer
def receiver_function(sender, instance, *args, **kwargs):
    instance.name.upper()
    instance.save()


class Person(models.Model):
    name = models.CharField(max_lenght=100)

    def __str__(self):
        return self.name

# Vinculação do model com a função definida
pre_save.connect(receiver=receiver_function, sender=Person)
```

Observe que a leitura da função é como segue: Quando a instância da classe `Person` emite o sinal de `save()`, a função `receiver_function` recebe a instância em questão realizando as modificações e ações necessárias.

> **Importante**: Caso o `sender` não seja especificado, a função será executada toda vez que o sinal em questão for emitido pelo Django.

Outra maneira de utilizar os sinais é através de decoradores, os quais podem receber uma lista de sinais:


```python
from django.db import models
from django.db.models.signals import pre_save, post_save

@receiver([pre_save], sender=Person)
def receiver_function(sender, instance, *args, **kwargs):
    instance.name.upper()
    instance.save()

class Person(models.Model):
    # ...
```

Para evitar efeitos colaterais, uma boa prática é salvar as funções de `signals` no método `ready` do `AppConfig`:

```python
# myproject/myapp/app.py
from django.apps import AppConfig
from django.db.models.signals import pre_save

# ...

class PersonConfig(AppConfig):
    name = 'person'
    verbose_name = "awesome person"

    def ready():
        pre_save.connect(receiver=receiver_function, sender=Person)
```

## Referências

- [Documentação Oficial](https://docs.djangoproject.com/en/3.2/topics/db/models/)
- [Repositório Oficial](https://github.com/django/django/tree/main/django/db/models)
