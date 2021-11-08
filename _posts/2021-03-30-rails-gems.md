---
layout: post
date: 2021-03-29 01:00:00 -0300
title: "Gems"
slug: rails gems
tag: tutorial
order: 100
category: "Rails"
comments: false
---

## O que é uma Gem?

Gem é o nome dado a um pacote ou biblioteca da linguagem Ruby. O próprio Rails é um Gem da linguagem Ruby. Outras gems podem ser encontradas no website [RubyGems](https://rubygems.org). Para garantir a qualidade e uso de uma Gem, visite o [RubyToolBox](https://www.ruby-toolbox.com).

Abaixo está descrito as principais gems utilizadas em um ambiente Rails e suas configurações básicas, sendo o objetivo de cada uma:

- **Rspec**: utilizada para testes unitários, mocks, stubs, etc.
- **Capybara**: utilizada para testes de aceitação
- **Rubocop**: utilizado para garantir o cumprimento do [Rails Style Guide](https://rails.rubystyle.guide)
- **FactoryBot**: utilizado para a criação de objetos de forma dinâmica
- **ShouldaMatchers**: utilizado para teste unitários e de validação dos models

> O uso de cada uma delas é independente da outra e totalmente facultativo. Entretanto, é uma boa prática de software e, principalmente na comunidade Rails, o desenvolvimento dirigido por testes.

---

### Instalando o Rspec

Apesar do _minitest_ vir como instrumento padrão de testes do Rails, o mesmo não tem sido comumente utilizado em aplicações comerciais, por isso, recomenda-se instalar o [Rspec](https://rspec.info).

No arquivo `Gemfile`, adicione o Rspec tanto no ambiente de teste quanto de desenvolvimento:

```ruby
group :development, :test do
  gem 'rspec-rails', '~> 5.0.0'
end
```

Então execute a instalação das dependências com o `bundle`:

```bash
bundle install
```

Em seguida, faça a instalação do Rspec como padrão de testes do sistema:

```bash
rails generate rspec:install
```

Para mais detalhes, veja as orientações de instalação e configuração no [Repositório Oficial](https://github.com/rspec/rspec-rails)

---

### Instalando o Capybara

O Rspec enfatiza a criação de testes unitários, contudo, tem grande conexão com outra ferramenta de testes do Rails, o Capybara.

No arquivo `Gemfile`, adicione o Capybara no ambiente de teste:

```ruby
group :test do
  gem 'capybara'
end
```

Então execute a instalação das dependências com o `bundle`:

```bash
bundle install
```

Para mais detalhes, veja as orientações de instalação e configuração no [Repositório Oficial](https://github.com/teamcapybara/capybara)

---

### Instalando o Rubocop

No arquivo `Gemfile`, adicione o Rubocop no ambiente de desenvolvimento:

```ruby
group :development do
  gem 'rubocop', require: false
end
```

Então execute a instalação das dependências com o `bundle`:

```bash
bundle install
```

E crie o arquivo `.rubocop.yml` no diretório raiz com a seguinte configuração:

```yml
require:
  - rubocop-rails

AllCops:
  TargetRubyVersion: 2.7
  NewCops: enable
  SuggestExtensions: false
  Exclude:
    - 'bin/**/*'
    - 'vendor/**/*'
    - 'db/**/*'
    - 'config/**/*'
    - 'node_modules/**/*'


Style/Documentation:
  Enabled: false

Style/FrozenStringLiteralComment:
  Enabled: false

Metrics/BlockLength:
  IgnoredMethods: ['describe', 'context', 'feature', 'scenario', 'let']
```

Para mais detalhes, veja as orientações de instalação e configuração no [Repositório Oficial](https://github.com/rubocop/rubocop)

---

### Instalando o FactoryBot

No arquivo `Gemfile`, adicione o FactoryBot no ambiente de desenvolvimento e de teste:

```ruby
group :development, :test do
  gem 'factory_bot_rails'
end
```

Então execute a instalação das dependências com o `bundle`:

```bash
bundle install
```

No arquivo `spec/rails_helper.rb` ative a seguinte linha:

```ruby
# ...
Dir[Rails.root.join('spec/support/**/*.rb')].sort.each { |f| require f }
# ...
```

E crie o arquivo `spec/support/factory_bot.rb`, com a seguinte configuração:

```ruby
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```

Para mais detalhes, veja as orientações de instalação e configuração no [Repositório Oficial](https://github.com/thoughtbot/factory_bot_rails)

---

### Instalando o ShouldaMatchers

No arquivo `Gemfile`, adicione o ShouldaMatchers no ambiente de teste:

```ruby
group :development, :test do
  gem 'shoulda-matchers', '~> 4.0'
end
```

Então execute a instalação das dependências com o `bundle`:

```bash
bundle install
```

Adicione, ao final do arquivo `spec/rails_helper.rb` o seguinte:

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

Para mais detalhes, veja as orientações de instalação e configuração no [Repositório Oficial](https://github.com/thoughtbot/shoulda-matchers)

### Referências

- [Site Oficial do RubyGems](https://rubygems.org)
- [Site Oficial do RubyToolBox](https://www.ruby-toolbox.com)
- [Rspec: Documentação Oficial](https://rspec.info)
- [Rspec Rails: Documentação Oficial](https://github.com/rspec/rspec-rails)
- [Capybara: Documentação Oficial](https://github.com/teamcapybara/capybara)
- [Rubocop: Documentação Oficial](https://github.com/rubocop/rubocop)
- [FactoryBot: Documentação Oficial](https://github.com/thoughtbot/factory_bot_rails)
- [Shoulds Matcher: Documentação Oficial](https://github.com/thoughtbot/shoulda-matchers)
