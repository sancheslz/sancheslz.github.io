---
layout: post
date: 2021-04-30 00:00:00 -0300
title: "Models"
slug: rails models
tag: tutorial
order: 3
category: "Rails"
comments: false
---

## Models no Rails

Conforme apresentado na [Visão Geral](https://sancheslz.github.io/rails-intro), todas as requisições do Cliente serão tratadas pelo gerenciador de rotas do Rails que fará o redirecionamento para os controllers os quais, conforme suas instruções farão consultas aos models. A criação de models no Rails ocorrerem de forma simple e rápida, graças à seu ORM e as convenções adotadas pelo `ActiveRecord`.

## Anatomia Geral do Model

```ruby
class Product < ApplicationRecord
  validates :name, presence: true

  def up_name
    self.name.upcase
  end
end
```

Vale notar que a estrutura dos models do Rails é extremamente enxuta, uma vez que a criação das tabelas no Banco de Dados através do seu ORM se dá, principalmente através das suas `migrations`. Dessa forma, as alterações e inclusões no `model` ocorrem como forma de complementar e implementar comportamentos. Por isso, analisaremos a estrutura das `migrations` como parte essencial da definição dos `models`.


### Tipos de Campos

Para facilitar a criação dos `models`, o Rails oferece o Rails CLI, uma ferramenta de interface de linha de comando que automaticamente gera os arquivos de migração (`migrations`) necessários para a vinculação do `model` com o Banco de Dados.

```
rails generate model <model_name> <field>:<field_type>
```

Podendo ser atribuido ao campo os seguintes tipos de dados:

- `:primary_key`: define o campo a ser utilizado como chave-primária (por padrão o Rails criar uma chave-primária como `id`)
- `:string`: usado para textos curtos
- `:text`: usado para textos longos
- `:integer`: usado para números inteiros
- `:bigint`: usado para valores inteiros de grande valor
- `:float`: usado para números de ponto flutuante
- `:decimal`: usado para números decimais
- `:numeric`: usado para campos numéricos de modo geral 
- `:datetime`: usado para o padrão de data e hora
- `:timestamp`: usado para o padrão de data e hora no formato `timestamp`
- `:time`: usado para horas
- `:date`: usado para datas
- `:binary`: usado para binários
- `:boolean`: usado para valores booleanos
- `:references`: usado para a vinculação de chave-estrangeira

Ao executar esse comando e informar os campos necessários e seus tipos, o Rails gerará o arquivos de migração em `db/migration/` e o arquivo de model em `app/models/`. Para personalizar alguma funcionalidade dos campos, é necessário alterar a migração antes de aplicá-la ao `schema`. Por isso, conhecer as `migrations` é essencial na construção dos `models`.

> Caso nenhum campo seja informado ao executar o comando acima, os mesmos poderão ser definidos, posteriormente, no próprio arquivo de migração

## Anatomia Geral da Migration

Toda vez que uma migration é criada, o Rails gerará um arquivo com a data de geração e o nome da migração, a qual será utilizada para o controle de versão do `schema` do Banco de Dados.

```ruby
class CreateProducts < ActiveRecord::Migration[6.0]
  def change
    create_table :products do |t|
      t.string :name, null: false
      t.integer :age

      t.timestamps
  end
end
```

Este arquivo possui, em essência: a ação (`creata_table`), nome da tabela (`products`), o tipo de campo (`string`), nome do campos (`name`) e sua validação (`null: false`) no Banco e as validações.

### Validações

As migrações permitem que sejam inseridas validações no Banco de Dados, podendo ser elas:

- `:limit`: recebe um valor inteiro que limite o número de caracteres, números ou bytes
- `:default`: define um valor padrão caso nenhum valor seja informado
- `:null`: recebe um valor booleano que define se o campo poderá ou não ser nulo
- `:precision`: recebe um valor inteiro que define a precisão de valores numéricos, decimais, datas e horas
- `:scale`: define a escala das casas decimais em valores decimais e numéricos
- `:collation`: define o tipo de `encoding` de strings e textos
- `:comment`: especifica um comentário para o campo
- `:if_not_exists`: recebe um valor booleano que define que se a coluna já existe no Banco do ORM não tentará inseri-la novamente
- `:unique`: recebe um booleano que define se o campo deverá ser único da tabela do Banco de Dados
- `:length`: recebe um inteiro que define o tamanho máximo do campo

## Associações

O relacionamento entre as tabelas e models do Rails pode ser feita de diferentes formas, cada uma com sua especificidae e funcionalidades. Saber qual modelo utilizar é fundamental para criar boas associações.

### belongs_to

O `belongs_to` cria um relacionamento do tipo `book <- author`, porém não garante a integridade no Banco de Dados. Para garantir essa integridade, é necessário criar um relacionamento bi-direcional atribuindo uma referência de associação como `has_one` ou `has_many`.

```ruby
class Book < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
end
```

Neste exemplo, todo livro terá um autor, mas nem todo autor terá um livro.

### has_one

O `has_one` cria um relacionamento do tipo `book -> author`, porém não garante a integridade no Banco de Dados. Para garantir essa integridade, é necessário criar um relacionamento bi-direcional atribuindo uma referência do tipo `belongs_to`.

```ruby
class Book < ApplicationRecord
end

class Author < ApplicationRecord
  has_one :book
end
```

Neste exemplo, todo autor terá um, e apenas um, livro, mas nem todo livro terá um autor.

### has_many

O `has_many` cria um relacionamento do tipo `book * <- author`, porém não garante a integridade no Banco de Dados. Para garantir essa integridade, é necessário criar um relacionamento bi-direcional atribuindo uma referência do tipo `belongs_to`.

```ruby
class Book < ApplicationRecord
end

class Author < ApplicationRecord
  has_many :books
end
```

Neste exemplo, todo autor terá um ou mais livros, mas nem todo livro terá um autor.

> **Atenção**: Observe que a associação usa o plural da referência à `book`

### has_and_belongs_to

O tipo de relacionamento `has_ang_belongs_to` ocorre quando na definição das associações, duas tabelas possuem uma relação do tipo muitos para muitos e não há um `model` intermediário. Indicando que cada `model` possui zero ou mais associações com o outro `model`.

```ruby
class Assembly < ApplicationRecord
  has_and_belongs_to :parts
end

class Part < ApplicationRecord
  has_and_belongs_to :assemblies
end
```

### has_many: through

O tipo de relacionamento `has_many: through` ocorre quando na definição de modelos associativos, deseja-se criar um novo modelo e através dele. Ao definir o `has_many: ..., through: ...` é possível indicar o nome a ser utilizado por aquela tabela para acessar os valores da tabela relacionada.

Observe que a definição possui o seguinte padrão:

```ruby
class OneModel < ApplicationRecord
  has_many :other_model_name
  has_many :any_name, through: :other_model_name
end
```

Dessa forma, dentro do model sempre inicia-se definindo a associação com o outro model (`other_model_name`), em seguida é possível definir um nome pelo qual esse model será acessado (`any_name`).

```ruby
class Patient < ApplicationRecord
  has_many :appointments
  has_many :physicians, through: :appointments
end

class Physician < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments
end

class Appointment < ApplicationRecord
  belongs_to :patient
  belongs_to :physician
end
```

No exemplo acima, observa-se que um paciente (`patient`) pode ter um ou mais agendamentos (`appointments`). Caso o `through` não houvesse sido informado, para acessar essa associação seria necessário invocar `patient.appointments.physician.name`. Ao utilizar o `through` o nome do médico (`physician`) pode facialmente ser acessado através de `patient.pyshicians.name`.

> **Dica**: caso não haja um nome que corresponda claramente a essa associação, uma boa prática é utilizar o padrão o nome dos models associados em ordem alfabética `FirstModelSecondModel`

### Opções das Associações

Apesar de possuir uma configuração padrão, as associações podem receber diversos parâmetros como opções, por exemplo:

```ruby
class Book < ApplicationRecord
  belongs_to :author, counter_cache: true
end
```

Veja abaixo a lista de opções disponíveis:

Parâmetro | Tipo | Descrição
--- | --- | ---
`:autosave` | booleano | se `true`, o Rails salvará alteração na associação entre os membros. Por padrão é `false`
`:class_name` | string | recebe o nome da classe a ser associada, quando o nome do outro model não é derivado pela associação
`:counter_cache` | booleano | mantém em cache a quantidade (`count`) de associações entre os membros. É possível sobrescrever o nome da coluna a ser utilizada na contagem informando um symbol. Padrão de nome da coluna `<model_name>_count`.
`:foreign_key` | string | recebe o nome que será utilizado para referenciar a chave-estrangeira. Por padrão é `<model_name>_id`
`:primary_key` | string | recebe o campo que será utilizado como chave-primária da classe associada. Por padrão é o `id`
`:inverse_of` | symbol | recebe o nome que será utilizado para referenciar o model associado. É utilizado em relacionamento bi-direcioanis como `has_one` e `has_many` com o `belongs_to`. Funciona de modo semelhante ao `through`.
`:touch` | booleano | se `true` o `update_at` ou `update_on` timestamp será atualizado para a hora atual sempre que o objeto for salvo ou destruido.
`:validate` | booleano | se `true` fará a validação dos objetos associados sempre que o mesmo for salvo. Por padrão é `false`
`:optional` | booleano | se `true`, a presença do objeto associado não será validada. Por padrão é `false`.

## Callbacks

Várias ações ocorrem durante o ciclo de vida de um objeto, por isso, o Rails provê acesso à `callbacks`, isto é, funções que podem ser passadas para serem executadas em determinado momento do ciclo de vida do objeto.

```ruby
class Book < ApplicationRecord
  before_create :to_uppercase

  private
  def to_uppercase
    self.name = self.capitalize
  end
end
```

O registro do `callback` pode ser feito tanto pela passagem da função como argumento (como no exemplo acima), como por meio de bloco (exemplo abaixo):

```ruby
class Book < ApplicationRecord
  before_create do
    self.name = self.capitalize
  end
end
```

Veja a lista de `callbacks` disponíveis e o momento em que cada um deles ocorre:

Callback | Momento
--- | ---
`before_validation` | imediatamente após a validação do objeto
`after_validation` | imediatamente antes da validação do objeto
`before_save` | após o registro ser salvo
`around_save` | entre o início e fim do registro ser salvo
`after_save` | após o registro ser salvo
`before_create` | antes do registro ser criado
`around_create` | entre o início e fim da criação do registro
`after_create` | após o registro ser criado
`before_update` | antes o registro ser atualizado
`around_update` | entre o início e fim da atualização do registro
`after_update` | após o registro ser atualizado
`before_destroy` | antes do registro ser destruido
`around_destroy` | entre o início e fim da destruição do registro
`after_destroy` | após o registro ser destruido
`after_initialize` | após o objeto ser inicializado, por exemplo, na chamada do `new`
`after_find` | após o carregamento do registo do banco de dados. Ocorre após o `after_initialize` se ambos estiverem definidos
`after_touch` | ocorre após o registro do objeto ser tocado

### Callbacks Condicionais

É possível estabelecer condições para que um determinado `callback` ocorra, podendo ser feito através do `if` e do `unless`:

```ruby
class Order < ApplicationRecord
  before_update :normalize_card_number, if :paid_with_card?

  private
  def normalize_card_number
      # ...
  end
end
```

Dependendo o caso, também é possível criar a condicional à partir do objeto `Proc`:

```ruby
class Order < ApplicationRecord
  before_update :normalize_card_number, 
                if: Proc.new { |order| order.paid_with_card? }

  private
  def normalize_card_number
      # ...
  end
end
```

## Referências

- [Documentação Oficial: ActiveRecord](https://guides.rubyonrails.org/active_record_basics.html)
- [Documentação Oficial: Migrations](https://guides.rubyonrails.org/active_record_migrations.html)
- [Documentação Oficial: Callbacks](https://guides.rubyonrails.org/active_record_callbacks.html)
- [Documentação Oficial: Associations](https://guides.rubyonrails.org/association_basics.html)
