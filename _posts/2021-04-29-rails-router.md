---
layout: post
date: 2021-04-29 00:00:00 -0300
title: "URLs"
slug: rails urls
tag: tutorial
order: 2
category: "Rails"
comments: false
---

![Logotipo do Rails](../assets/posts/rails_urls.png)

## Rotas no Rails

Conforme apresentado na [Visão Geral](https://sancheslz.github.io/rails-intro) do Rails, todas as requisições feitas pelo cliente passam inicialmente pelo arquivo de rotas, o `config/routes.rb` que herda suas funcionalidades do framework Rails `Router`. Por isso, definir rotas no Rails é uma tarefa extremamente simples e eficiente.

### Convenção de Rotas

Em todas as suas camadas, o Rails adota a filosofia _CoC_. Por isso, as URLs geradas pelo framework são pré-definidas a partir dos métodos que `Controller` pode executar:

Verbo HTTP | URL | Controller#Método | Rota Nomeada | Uso Recomendado
--- | --- | --- | --- | ---
GET | /articles | articles#index | articles_path | para retornar todos os itens cadastrados
GET | /articles/new | articles#new | new_article_path | para renderizar o formulário de criação
POST | /articles | articles#create | articles_path | para criar o item desejado
GET | /articles/:id | articles#show | article_path | para exibir o item do `id` desejado
GET | /articles/:id/edit | articles#edit | edit_article_path | para editar o item do `id` informado
PATCH/PUT | /articles/:id | articles#update | article_path | para atualizar os dados do item do `id` informado
DELETE | /articles/:id | articles#delete | article_path | para deletar o item do `id` informado

## Anatomia Geral das URLs

A geração de todas as URLs no formato apresentado acima é feito no arquivo `config/routes.rb` que possui a seguinte estrutura-base:

```ruby
Rails.application.routes.draw do
    # suas rotas aqui
end
```

O método `draw` será o responsável por gerar cada uma das todas informadas, sendo que elas podem ser atribuidas de diferentes formas:

### Rota Única

Quando o controller possui apenas um método, o mesmo pode ser definido informando o verbo HTTP que ele deverá responder, o formato da URL que deverá receber e para qual `controller#método` irá lidar com a requisição:

```ruby
Rails.application.routes.draw do
    get 'home', to: 'home#index'
end
```

### Rota Principal

A estratégia de uma rota com um único método, é comumente utilizada quando se fala da _HomePage_, por isso, o Rails permite a definição dessa rota de forma diferente. Ao invés de informar todos os detalhes como anteriormente, basta utilizar a palavra `root` e o `controller#método` que deverá atender.

```ruby
Rails.application.routes.draw do
    root: 'home#index'
end
```

### Uso de um Recurso

Quando o usuário **não** pode manipular mais do que um objeto, por exemplo, um perfil associado ao usuário logado, pode-se utilizar o `resource`. A principal diferença entre o `resource` e o `resources` é que o mesmo não possuirá acesso ao método `index`.

```ruby
Rails.application.routes.draw do
    resource :profile
end
```

### Uso de Recursos

Quando o usuário pode visualizar e manipular mais do que um objeto, informar o componente ao `resources` permitirá que todos os métodos e rotas daquele componente estejam disponíveis:

```ruby
Rails.application.routes.draw do
    resources :clients
end
```

## Adicionando Controle

### Controlando Acesso

Ao fazer uso de recursos, uma boa prática é restringir as rotas que estarão ou não disponíveis. Para isso, pode-se usar o `only` e o `except`.

O `only` recebe uma lista de símbolos correspondendo a cada um dos métodos permitidos. Assim, qualquer requisição que ocorra que não esteja dentro da lista, será rejeitada.

Já o `except` recebe uma lista de símbolos correspondendo ao métodos proibidos. Assim, se houver uma requisição de um método que esteja nesta lista, a mesma será rejeitada.

```ruby
Rails.application.routes.draw do
    resources :clents, only: %i[index show]

    resources :products, except: %i[edit update destroy]
end
```

> **Dica**: apesar de haver estas duas formas de controlar as rotas no Rails, é recomendado o uso do `only` por tornar mais explícito o que é permitido ao usuário

### Adicionando Ações

Além das sete ações pré-definidas pelo Rails, é possível adicionar novas ações às Rotas. Para isso, é necessário abrir o bloco dos recursos, informar o verbo HTTP, o padrão da URL e se a mesma será aplicável a apenas um objeto (`member`) ou em um conjunto de objetos (`collection`).

```ruby
Rails.application.routes.draw do
    resource :products do
        get 'promote', on: :member
    end
end
```

> Note que no caso do `member`, a requisição esperará que seja passado o `id` de um objeto na requisição.

## Personalizando as Rotas

### Rotas Nomeadas

Muitas vezes, usar o nome do recurso para definir as rotas e os `helpers` pode não ser uma boa opção, por isso, o Rails permite que as rotas sejam nomeadas, bastando utilizar o parâmetro `as:`

```ruby
get 'exit', to: 'sessions#destroy', as: :logout
```

Neste caso, ao invés de ter a rota `sessions_exit_path`, teremos acesso ao `logout_path`, melhorando a legibilidade do código.

Também é possível nomear um recurso:

```ruby
resources :products, as: 'items'
```

Neste caso, a URL permanecerá como `products`, mas o `helper` do Rails será utilizado como `items_path`.

### Customizando um Controller

Para permitir que tanto a URL quando o Helper do Rails sejam utilizados com um nome diferente do `Controller` é necessário alterar essa comportamento padrão. Para isso, faça a atribuição de `resources`, seguido de `controller:` indicando o `controller` que será utilizado:

```ruby
resources :photos, controller: 'images'
```

Neste caso, a URL apontará para `/photos` e o helper será `photos_path`, mas o controller continuará sendo `images#`.

### Namespaces

Se você deseja separar um grupo de controles em um caminho diferente da URL, é possível fazê-lo usando `namespace`:

```ruby
namespace 'api' do
    resources :articles
end
```

Neste caso, todas as rotas de `articles` serão precedidas de `api`, de modo que:

- a URL será: `api/articles`
- o controller será: `api/articles#`
- o helper será: `api_articles_path`

> Nota: Lembre-se que nesse caso, o `ArticlesController` deverá estar em um subdiretório de controller

## Restrições

O Rails permite que a rota tenha _constraints_, isto é, que suas ações sejam restritas mediante algumas validações. Permitindo tanto restringir o tipo de verbo HTTP quando um seguimento específico de rota.

### Verbos HTTP

Para restringir um verbo HTTP, deve-se usar o atributo `:via` passando a lista de métodos permitidos, podendo ser: `get`, `post`, `put`, `patch` e `delete`:

```ruby
match 'photos', to: 'photos#show', via: [:get, :post]
```

### Seguimento de Rota

Para validar um determinado seguimento da rota, basta passar a expressão regular de validação para o parâmetro `:constraints`:

```ruby
get 'photos/:id', to: 'photos#show', constraints: { id: /[A-Z]\d{5}/ }
get 'client/:id', to: 'clients#show', id: /d{4}/
```

Ambas as formas funcionam para validação do seguimento de rota. No primeiro caso, observa-se que o id `A12345` funcionará, mas o id `893` não.

## Recursos Adicionais

### Traduzindo Paths

Para traduzir os caminhos gerados automaticamente do `resources`, é necessário utilizar o método `scope`, passando os `path_names` que deseja alterar:

```ruby
scope(path_names: { new: 'novo', edit: 'editar'}) do
    resources :categories, path: 'categorias'
end
```

### Sobrescrevendo os Parâmetros da Rota

Por padrão o Rails utiliza para suas rotas o parâmetro `id` do model, caso seja necessário alterar este comportamento, basta passar o atributo `param` para a rota:

```ruby
resources :photos, param: :sku
```

## Separando as URLs

Em arquivos **muitos** grandes, é possível separá-los em múltiplos pequenos arquivos de rota:

```ruby
# config/routes.rb
# ---

Rails.application.routes.draw do
    root 'home#index'

    draw(:profile)
end

# config/routes/profile.rb
# ---

namespace :profile do
    resources :client
end
```

> **Nota**: Avalie bem a necessidade ou não de separar as rotas em mais de um arquivo. Apesar da prática ser válida, a mesma não é encorajada pela documentação do framework.
## Referências

- [Documentação Oficial](https://guides.rubyonrails.org/routing.html)
- [Rails API](https://api.rubyonrails.org/classes/ActionDispatch/Routing.html)
