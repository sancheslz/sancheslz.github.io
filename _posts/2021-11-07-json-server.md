---
layout: post
date: 2021-11-07 01:00:00 -0300
title: "Json Server"
slug: json server
tag: tutorial
order: 1
category: "JavaScript"
comments: false
---

Json Server, é uma maneira rápida de criar APIs REST para teste, permitindo criá-las em menos de 30 segundos. Seu objeto é permitir que, a partir de um arquivo `.json`, desenvolvedores front-end, possam fazer protótipos e mocks do backend de modo a auxiliar no desenvolvimento do projeto.


## Instalação

Assim como outras bibliotecas JavaScript, o Json Server também está disponível para instalação através do [NPM](https://www.npmjs.com/package/json-server), podendo ser instalando tanto globalmente como localmente, da seguinte maneira:

Para instalar globalmente, execute o seguinte comando no Terminal: `npm install -g json-server`.

Para instalar localmente, execute os seguintes comandos no Terminal:

```console
npm init -y
npm install json-server
```

Com isso, será criado um arquivo `package.json` e a pasta `node_modules`, contento a biblioteca e o controle das demais dependências utilizadas.

## Anatomia Geral do Arquivo

As definições das rotas e os objetos a serem retornados através do Json Server, são definidas a partir da construção de um arquivo `.json`, sendo que as chaves correspondem a rota e os valores ao conjunto de dados a serem retornados por esta rota:

```json
{
  "route": [
    {"id": "123", "key": "one_value"},
    {"id": "345", "key": "other_value"},
  ],
  "other": {"message": "hello world"}
}
```

Observe que o nome usado pela rota será no singular ou plural, dependendo do tipo de retorno da mesma. No exemplo acima, a rota `other`, será singular, uma vez que devolve apenas um objeto. Veja a diferença entre elas:

verbo http | objetivo | singular | plural
--- | --- | --- | ---
`get` | receber todos os objetos | | `/posts`
`get` | receber o objeto especificado | `/author` | `/posts/1` 
`post` | criar um novo objeto | `/author` | `/posts`
`put` | atualizar um objeto existente | `/author` | `/posts/1` 
`patch` | atualizar um objeto existente | `/author` | `/posts/1` 
`delete` | deletar um objeto existente | | `/posts/1` 

> Para executar os métodos `Put`, `Patch` e `Post`, é necessário incluir na requisição o `Content-Type: application/json`.

## Definindo Retornos

Além de permitir retornar um ou mais objetos, o Json Server possui opções de chamada que auxiliam na definição do retorno esperando.


### Filtros

Para filtar um determinado item, na hora da requisição, basta passar um sinal de interrogação com nome da chave, o sinal de igual e o tipo de valor esperado (`?key=value`). Caso seja necessário passar mais de uma chave, combine-as com o e-comercial `&`:

```console
/books?title=Rings

/books?title=Rings&author=Tolkien

/books?author.name=Tolkien
```

> Observe que no último exemplo, foi utilizado o `.` para acessar propriedades mais profundas do objeto

### Paginação

Além de permitir filtrar por um determinado resultado, também é possível realizar a paginação das chamadas, bastando para isso utilizar os parâmetros `_page` que indica a página atual e o `_limit` que define o número de itens por página (padrão é de 10 itens).

```console
/posts?_page=7&_limit=20
```

> No `header` da resposta desta requisição, serão devolvidos os seguintes links: `first`, `prev`, `next`, `last`

### Ordenação

Caso seja necessário ordenar o resultado, é possível passar o parâmetro `_sort` definindo qual campo será utilizado para a ordenação, além disso, pode-se definir se a ordem será ascendente (`asc`) ou descendente (`desc`) através do parâmetro `_order`:

```console
/posts?_sort=author.name&_order=desc
```

> Caso seja necessário fazer multiplas ordenaçÕes use a virgula (`,`) para separar cada uma delas. Exemplo: `/posts?_sort=user,views&_order=desc,asc`

### Fatiamento

Caso deseje receber como resposta apenas uma parte dos resultado, é possível fazer o fatiamento através dos parâmetros `_start`, `_end` e `_limit`:

```console
/posts?_start=20&_end=30

/posts/1/comments?_start=20&_end=30

/posts/1/comments?_start=20&_limit=10
```

> Importante notar que no fatiamento, o `start` é inclusivo e o `end` é exclusivo

### Operadores

Para consultas mais elaboradas, é possível utilizar operadores, similares às queries:

operador | objetivo
--- | ---
`_gte` | retorna valores maiores do que
`_lte` | retorna valores menores do que
`_ne` | retorna valores diferentes de
`_like` | retorna valores semelhantes a (suporta o uso de regex)
`_q` | retorna valores com correspondência (`full-text search`)
`_embed` | inclui recursos-filho na consulta
`_expand` | inclui recursos-pai na consulta

```console
/posts?views_gte=10&views_lte=20

/posts?id_ne=1

/posts?title_like=server

/posts?q=internet

/posts?_embed=comments
/posts/1?_embed=comments

/comments?_expand=post
/comments/1?_expand=post
```

Caso deseje obter toda a base de dados de todas as rotas, acesse a url `/db`

Para ver o arquivo `index` - salvo em `public/`, acesse a url `/`. Este arquivo é útil para a documentação das rotas e tipos de retornos.


## Criando Dados Dinamicamente

A criação de grandes volumes de dados de teste pode ser uma tarefa tediosa e custosa, para agilizar este processo, é possível criar dados de forma programática através de um arquivo Java Script, seguindo a seguinte estrutura:

```js
const faker = require('faker')

module.exports = () => {
  const data = {
    users: [],
    posts: []
  }

  // ...

  for (let element = 1; element < 100; element ++) {
    data.users.push({
      id: element,
      name: faker.internet.userName(),
      email: faker.internet.email()
    })
  }

  // ...

  return data
}
```

Observe que é possível retornar quantas rotas e objetos quiser, bastanto que a função, ao fim, retorne um objeto que siga as especificações Json.

> **Dica**: para criar grandes volumes de dados, combine o Json Server com bibliotecas como o [faker](https://github.com/Marak/faker.js), [chance](https://github.com/chancejs/chancejs), [casual](https://github.com/boo1ean/casual) ou [json-schema](https://github.com/json-schema-faker/json-schema-faker/tree/master/docs) para ganhar produtividade


## Rotas Customizadas

Para casos em que as rotas necessitam de algum tipo de personalização, é possível criar uma rota customizada, como no exemplo abaixo:

```json
{
  "/api/*": "/$1",
  "/:resource/:id/show": "/:resource/:id",
  "/posts/:category": "/posts?category=:category",
  "/articles\\?id=:id": "/posts/:id"
}

```

Cada linha apresenta um comportamento específico, onde a chave representa a url acessada e o valor, a ação interna:

- `/api/*`: define que tudo o que vier após o `/api/` será considerado, dessa forma, acessar `/api/posts`, internamente irá o recurso `/posts`
- `/:resource/:id/show`: define que sempre que for utilizado o complemento `/show`, o mesmo será abstraido e o recurso retornado será o correspondente ao recurso e seu id `/:resource/:id`
- `/posts/:category`: neste caso, a categoria informada na url servirá como parâmetro na *query string* de categorias: `?category=:category`
- `/articles\\?id=:id`: informa que ao ser acessado um `articles` com uma *query string* de id, o recurso retornado será de `/posts/:id`

Observe que para rodar o servidor, com as rotas costumizadas, é preciso informar qual arquivo será utilizado para a definição das rotas, isso pode ser feito através da flag: `--routes <routes_filename>.json`. Exemplo:

```console
json-server db.json --routes routes.json
```

> Também é possível adicionar lógicas de `SSL` e `Middleware` nas definições do seu JsonServer, para saber mais, consulte a documentação oficial


## Json Server CLI

O Json Server possui uma interface de linha de comando (CLI), que permite que diversas configurações ou comportamentos sejam adicionado ao servidor em sua execução, para utilizá-los basta seguir o padrão:

```console
json-server [options] <source>
```

Sendo as opções possíveis:

opção | uso | valor padrão
--- | --- | ---
`--config`, `-c` | path para o arquivo de configuração | `json-server.json`
`--port`, `-p` | porta utilizada pelo servidor | `3000`
`--host`, `-H` | host utilizado pelo servidor | `localhost`
`--watch`, `-w` | define se o arquivo deverá ou não ser monitorado por alterações | `boolean`
`--routes`, `-r` | path para o arquivo de rotas | 
`--middlewares`, `-m` | path para os arquivos de middleware | `array`
`--static`, `-s` | diretório dos arquivos estáticos | 
`--read-only`, `--ro` | permite apenas requisições do tipo `get` | `boolean`
`--no-cors`, `--nc` | desabilita a função de *Cross-Origin Resource Sharing*  (CORS) | `boolean`
`--no-gzip`, `--ng` | desabilida o enconding de `gzip` |  `boolean`
`--snapshots`, `-S` | configura o diretório onde serão salvos os snapshots | `.`
`--delay`, `-d` | adiciona um delay (em milisegundos) para as respostas
`--id`, `-i` | configura o tipo de id da base de dados | `id`
`--foreignKeySuffix`, `--fks` | configura o sufixo adicionado às chaves-estrangeiras, por exemplo, `_id` como em `post_id` | `Id`
`--quiet`, `-q` | suprime as mensagens de log | `boolean`
`--help`, `-h` | exibe a ajuda | `boolean`
`--version`, `-v` | exibe a versão | `boolean`

Exemplos de uso:

```console
json-server db.json
json-server file.js
json-server http://example.com/db.json
```

### Referências

- [Repositório Oficial](https://github.com/typicode/json-server)
