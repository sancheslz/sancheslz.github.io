---
layout: post
date: 2021-04-28 00:00:00 -0300
title: "URLs"
slug: django urls
tag: tutorial
order: 2
category: "Django"
comments: false
---


## Rotas no Django

Conforme apresentado na [Visão Geral](https://sancheslz.github.io/django-intro) do Django todas as requisições feitas pelo cliente passam inicialmente pelo arquivo de rotas, o `urls.py` um arquivo Python que herda as funcionalidades do `django.urls`. Por isso, conhecer como definir as rotas é uma tarefas essência para trabalhar com o framework.

## Anatomia Geral das URLs

No Django, as URLs são definidas através de uma lista (array), contendo uma lista de funções `path`. Essa função recebe os parâmetros que irão indicar como o Framework deve lidar com as requisições:

```python
path(route, view, kwargs=None, name=None)
```

- `route`: string correspondendo indicando a URL ou seu padrão que deverá invocar a `view`
- `view`: camada que receberá a requisição e deverá retornar uma resposta ao cliente
- `kwargs`: dicionário de argumentos a serem passados como parâmetros para a `view`
- `name`: nome atribuido a rota, podendo ser utilizados posteriormente para reversão de URLs no templates do Django.

Todas as rotas, devem ter início no arquivo principal de rotas:

```python
# ./project/urls.py
from django.contrib import admin
from django.urls import path
from home.views import main, other

urlpatterns = [
    path('admin/', admin.site.urls),
    path('current/', atual, {'current_year': 2005}),
    path('home/', main, name='super_app'),
]
```

## Separando as URLs

Conforme o projeto cresce em número de apps, manter todas as URLs em um único lugar pode não ser a melhor alternativa. Por isso o Django permite que as URLs sejam quebradas em arquivos específicos para cada aplicação, ficando o arquivo principal responsável de vincular todos os demais. Para isso, no arquivo `urls.py`, use a função `include`:

```python
# ./project/urls.py
# ...
from django.urls import path, include

urlpatterns = [
    # ...
    path('home/', main, name='super_app'),
    include('promotion.urls', namespace=promotion)
]
```

A função `include` pode receber dois parâmetros, uma string formada pelo nome do app e o nome do arquivo urls e um `namespace` o qual, em essência, possui o mesmo comportamento do atributo `name` já apresentado.

Com isso, basta criar um arquivo `urls.py` no diretório do app, importar `from django.urls import path` e criar a lista `urlpatterns`.

Os atributos `namespace` e `name` podem ser utilizados nos templates dessa maneira:

```html
{% raw %}<a href="{% url 'promotion:new' %}">Link</a>{% endraw %}
```

> O namespace é uma forma de criar uma referência nomeada para um URL específica, devendo seguir o padrão adotado pelo Django: `app_name:instance_name`, por exemplo, a página principal do DjangoAdmin seria `admin:main`, onde a primeira parte é o `name` da URL principal e o `main` é o namespace da URL específica.

### Outras Formas

Além da separação das URLs em arquivos separados, é possível separá-las no mesmo arquivo de duas formas:

#### URL em Listas

```python
app_list = [
    path('new_product', app.views.product),
    path('see_record', app.views.records), 
]

urlpatterns = [
    # ...
    path('app', include(app_list)),
]
```

#### URL Aninhada

```python
urlpatterns = [
    path('my_app', include([
        path('new', view.new),
        path('search', views.search),
    ])),
]
```

## Dispatcher

Os _dispatchers_ são os responsáveis por proteger, receber a passar os parâmetros recebidos na composição da URL, permitindo a definição das rotas e direcionamentos para as `views`.

Tipo | Formato | Nota
--- | --- | ---
string | `my_app/<str:value>` | permite a passagem de strings, mas não permite endereços com o uso de "/", por exemplo.
inteiros | `'my_app/<int:value>'` | valida se o padrão apresentado for um número inteiro
slug | `'my_app/<slug:value>'` | valida os padrões de código ASCII (letras, números, hífens e underscores). Exemplo `my_awesome_article`
uuid | `'my_app/<uuid:value>'` | valida se o padrão apresentado corresponder a um UUID (Universally Unique Identifier). Exemplo: `0737sad172-081289273a-12098as098`
path | `'my_app/<path:value>'` | valida se o padrão apresentado for um caminho, podendo, com isso incluir as barras "`/`"
regex | `'my_app/?(<value>:[a-z]{2}[0-9]{4})'` | valida se o padrão apresentado corresponder com a expressão regular. Neste exemplo, será validado caso possua duas letras e quatro números. Exemplo: `my_app/az4529`

Ademais, na construção de URLs com Expressões Regulares, pode-se utilizar argumento não capturados, bastando para isso, utilizar o seguinte padrão `?:`. Por exemplo:

```python
from django.urls import re_path

urlpatterns = [
    # Irá chamar a view posts com os seguinres argumentos: page_number. Exemplo: `posts/page` e `posts/page-2`
    re_path(r'^posts/(?:page-(?P<page_number>\d+/)?$', posts)
]
```

## Reversão de URLs

Para saber a URL absoluta a partir do `name` atribuído a ela, basta utilizar a função `reverse`, essa função pode receber os seguintes parâmetros:

```python
reverse(viewname, urlconf=None, args=None, kwargs=None, current_app=None)
```

Com isso, além de informar o `name` da rota, é possível passar uma lista ou dicionário de argumentos como parâmetro:


```python
from djanog.http import HttpResponseRedirect
from django.urls import reverse

def backHome(request):
    # ...
    return HttpResponseRedirect(reverse('url_name', args=(year, month, day)))
```

### Conversões de URL x Python

Por padrão o Django já oferece algumas formas de conversão das entradas de URL em formato Python são: `<int:value>`, `<str:value>`, `<slug:value>`, `<path:value>` e `<uuid:value>`. Entretanto, é possível criar o próprio método de conversão

Para isso, crie uma classe para o novo método:

```python
# converter.py
class ConvertFourDigits:
    
    regex = '[0-9]{4}' # padrão da regex

    # Método padrão de URL > Python
    def to_python(self, valor):
        return int(valor)
    
    # Método padrão de Python > URL
    def to_url(self, valor):
        return '%04d' % valor
```

Em seguida, no arquivo de URLs, chame a função `register_converter`. Essa função receber dois argumentos:

```python
register_converter(converter, type_name)
```

O `converter` é a referência à classe criada, já o `type_name` é o nome que será utilizado para referênciar esse padrão nas URLs:

```python
# urls.py
from django.urls import path, register_converter
from . import converter, views

register_converter(converters.ConvertFourDigits, 'fd_year')

urlpatterns = [
    path('articles/2003', views.special_article_2003),
    path('articles/<fd_year:year>', views.year_article),
]
```

## Referências

- [Documentação Oficial](https://docs.djangoproject.com/en/3.2/topics/http/urls/)
- [Repositório Oficial](https://github.com/django/django/tree/main/django/urls)
