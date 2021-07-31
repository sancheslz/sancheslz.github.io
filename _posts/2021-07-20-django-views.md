---
layout: post
date: 2021-07-20 00:00:00 -0300
title: "Views"
slug: django views
tag: tutorial
order: 6
category: "Django"
comments: false
---

## Views no Django

Com o `model` definido e sabendo como realizar operações com os registros no Banco de Dados (consultar, criar, atualizar e deletar), o próximo passo é definir a `view`. A `view` é a camada de controle da aplicação, responsável por receber a requisição, realizar as lógicas de negócio da aplicação e devolver a resposta desejada.

Toda requisição é recebida através das [URLs](https://sancheslz.github.io/django-urls), a qual redirecionará a requisição para o `controller` especificado. Este, por sua vez pode receber parâmetros e informações através da própria URL por diversos métodos HTTP, tal com `GET` e `POST`.

> As Views do Django podem ser estruturadas como tanto com FBV (_Function Based View_) quanto com CBV (_Class Based View_). No desenvolvimento da sua Aplicação Django, é provável que ambos os padrões sejam utilizados.

## Anatomia Geral da View

As `views`  (FBV) são funções que recebem como parâmetro a requisição e os parâmetros passados pela URL. Com base nos dados recebidos, pode-se realizar as consultas no Banco de Dados. Em seguida, devolve-se a requisição recebida, informando os valores a serem retornados, neste exemplo, a requisição é devolvida através de um template `HTML` com um contexto de preenchimento.

```python
from django.shortcuts import render
from mymodel import MyModel

def detail(request, param):
    query = MyModel.filter(field=param))
    context = {'data': query}
    return render(request, 'myapp/detail.html', context)
```

> Na requisição `book/10`, definida nas URLs como `book/<int:book_id`, a função receberá como parâmetros: `detail(request, book_id)`

## Objeto Requisição

## Objeto Resposta

### Tipos de Resposta

### Shortcuts

## Decoradores

## Function Based View
<!-- 
### Estrura Base

```python
def my_view(request, parametro):
    return HttpResponse('Ola Mundo, este é um {}'.format(parametro))

```

Toda _function_ da view recebe o `request` da URL e este `request` será devolvido na resposta.

Qualquer parâmetro da URL deve ser passado, também para a função, por exemplo, a URL `my_app/hello/2`, terá o seguinte padrão nas URLs do my_app `hello/<int:value>`. Esse parâmetro é o que será passado, posteriormente, para a função. -->

### Retornando um Erro

É possível realizar o tratamento de erros nas views do Django, para isso, uma forma prática de testá-los é responder com o código específico do erro buscado:

```python
from django.http import HttpResponse

def my_view(request):
    # ...
    return HttpResponse(status=201)
```

O erro 404, por ser o mais comum de ocorrer, possuí um método especial no Django, o `Http404`

```python
from django.http import Http404

def my_view(request, pk):
    try:
        p = Search.objects.get(pk=pk)
    except:
        raise Http404('Objeto não encontrado')
    return render(request, 'search/template.html', {'search': p})
```

## Decoradores

Os decoradores nas Views do Django tem o papel de restringir o acesso dos usuários de acordo com condições específicas.

- `require_http_methods()` recebe a lista com os métodos permitidos
- `require_GET()` permite acessar a view ser o método for GET
- `require_POST()` permite acessar a view se o método for POST
- `require_safe()` permite acesso se a view for GET ou HEAD, tradicionalmente seguras

## Shortcuts

- `render(request, template_name, context=None, content_type=None, status=None, using=None)`
  - **request** Requisição inicial da view a ser respondida
  - **template_name** Nome e caminho do template a ser     utilizado
  - **context** Dicionários de valores a serem passados ao     template
  - **content_type** O tipo de MIMEType que será utilizado     no documento
  - **status** Código de status da resposta, por padrão 200
  - **using** Nome do mecanismo de template utilizado

- `get_object_or_404(Model/QuerySet, *args, **kwargs)` invoca o método `get()` do gerenciador do manager, caso não consiga, levantará a excessão 404 (Não existe). Os argumentos são os aceitos pelo métodos `get()` e `filter()`

- `get_list_or_404()` equivalente ao anterior, mas retorna uma lista como resutado do `filter()` ou levanta o erro

- `redirect(to, permanent=False, *args, **kwargs)` os argumentos podem ser:
  - Um modelo chamado pela função `get_absolute_url()`
  - O nome de uma view, possivelmente com argumento `reverse()`
  - Um URL absoluto ou relativo

```python
from django.shortcuts import redirect

# Exemplo 1
def my_view(request):
    # ...
    obj = Model.objects.get(...)
    return redirect(obj)


# Exemplo 2
def my_view(request):
    # ...
    return redirect('view_name', args='value')
    

# Exemplo 3
def my_view(request):
    return redirect('path/to/url')
```












--------

--------

--------

--------

--------

--------

--------

--------

--------





























## Class Based View

### Estrutura Base

A estrutura-base da _Class Based View_ consiste na criação de uma classe para a view, com a indicação do template em `template_name`. Para sua chamada nas URLs, é preciso utilizar o método `as_view()`

```python
# views.py
from django.views.generic import TemplateView

class HomeView(TemplateView):
    template_name = 'index.html'
```

```python
# urls.py
from django.urls import path 
from meuapp.views import HomeView

urlpatterns = [
    path('home/', HomeView.as_view()),
]
```

#### Passandro atributos pela URL

```python
from django.views import View

class MyView(View):
    greeting = 'Bom dia'
    def get(self, request):
        return HttpResponse('{}'.format(self.greeting))
```

```python
urlpatterns = [
    #...
    # A renderização será de boa noite
    path('view/', MinhaView.as_view(greeting='Boa noite')), 
]
```

### Lidando com Formulários

Os métodos podem ser definidos individualmente através das funções da classe, sendo que o Form pode ser acessado através do atribuição do mesmo, exemplo `form_class`

```python
from django.http import HttpResponseRedirect
from django.shortcuts import render
from django.views import View
from app.forms import ArticleForm

class ArtigosView(View):
    form_class = ArticleForm
    initial = {'key': 'value'}
    template_name = 'article_form.html'
    
    def get(self, request, *args, **kwargs):
        form = self.form_class(initial=self.initial)
        return render(request, self.template_name, {'form':form})
    
    def post(self, request, *args, **kwargs):
        form = self.form_class(request.POST)
        if form.is_valid():
            # ... logica ...
            return HttpResponseRedirect('/sucesso/')
        return render(request, self.template_name, {'form': form})
```

### Decoradores de Classes

```python
from django.contrib.auth.decorators import login_required
from django.utils.decorators import method_decorator
from django.views.generic import TemplateView

@method_decorator(login_required, name='dispatch')
class ViewProtegida(TemplateView):
    template_name = 'segredo.html'
    # ...
```

### Classes Padrões

#### ListView

Retorna uma lista de objetos do Model definido

```python
from django.views.generic import ListView
from app.models import Autor

class AuthorList(ListView):
    model = Author # Modelo a ser listado
    template_name = 'authors_list.html' # Template a ser usado
    context_object_name = 'authors' # Contexto a ser passado para uso do Template
```

**Observações**:

- Caso não seja informado um template, automaticamente o Django buscará pelo padrão: `<model>_list.html`
- Se o `context_object_name` não for definido, o padrão será `object_list`

Caso seja necessário passar outras informações ao contexto, basta definir o método `get_context_data()`:

```python
class AuthorList(ListView):
    # ...
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['new_context'] = Books.objects.all()
        return context
```

Além disso, caso o uso de `model` não seja o ideal, é possível realizar a definição manual dos dados através do atributo `queryset`:

```python
class AuthorList(ListView):
    # ...
    queryset = Author.objects.fiter(pub_date__gt=2010)
    # ...
```

### Filtros Dinâmicos

```python
# urls.py
urlpatters = [
    # ...
    path('books/<year>', BooksList.as_view()),
]
```

```python
# views.py
class BooksList(ListView):
    template_name = 'books.html'
    
    def get_queryset(self):
        # self.kwargs[] é o dicionário de dispacher passado na URL
        return Books.objects.filter(year=self.kwargs['year']) 
```

### Trabalho Extra

Utilizado quando se deseja executar uma ação antes ou depois da chamada da View.

No exemplo abaixo, ao buscar um autor específico, o último acesso (última consulta) sobre este autor é atualizada. Para isso, deve-se definir o método `get_object()`

**Obs**. Por padrão o Django utiliza o identificador `pk` para carregar os objetos.

``` python
# urls.py
urlpatterns [
    # ...
    path('author/<int:pk>/', AuthorDetalheView.as_view()), 
]
```

```python
# views.py
from django.utils import timezone
from django.views.generic import DetailView
from app.models import Autor

class AuthorDetalheView(DetailView):
    queryset = Author.objects.all()
    
    def get_object(self):
        obj = super().get_object()
        obj.last_search = timezone.now()
        obj.save()
        return obj
```

## FormView

```python
from app.forms import ContactForm
from django.views.generic.edit import FormView

class ContactFormView(FormView):
    template_name = 'contact_form.html'
    form_class = ContacfForm
    success_url = '/obrigado/'
    
    def form_valid(self, form):
        # Método invocado se POST
        return super().form_valid(form)
```

**Exemplo com Validação de Usuário**:

Neste exemplo o formulário é criado e salva o usuário responsável pelo cadastro, permitindo que apenas este usuário acesse o conteúdo

```python
# models.py
from django.contrib.auth.models import User
from django.db import models

class Author(models.Model): 
    name = models.CharField(max_length=100)
    created_by = models.ForeignKey(User, on_delete=models.CASCADE)
```

```python
# views.py
from django.views.generic.edit import CreateView
from app.models import Autor

class AuthorCreate(CreateView):
    model = Author
    fields = ['name']
    
    def form_valid(self, form):
        form.instance.created_by = self.request.user
        return super().form_valid(form)
```
