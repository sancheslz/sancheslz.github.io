---
layout: post
date: 2021-07-27 00:00:00 -0300
title: "Pre-Commit"
slug: pre-commit
tag: tutorial
# order: 6
category: "Terminal"
comments: false
---

## O que é Pre-Commit

O `pre-commit` é um framework destinado ao gerenciamento e manutenção de `hooks` do Git em diversas linguages, sendo aplicável ao Python, Ruby, Node, etc. Com ele, é possível que, mediante a execução de algum `stage` do Git (como o `commit` e o `push`), uma bateria de verificações de garantia da Qualidade de Software sejam executadas, tais como testes, linters, etc.

## Instalação e Configuração

Para instalar o `pre-commit`, basta utilizar o seguinte comando `pip` no Terminal:

```
pip install pre-commit
```

Uma vez instalado o framework, o próximo passo é fazer a instalação dos `hooks` no `Git`, com o seguinte comando:

```
pre-commit install
```

> **Atenção**: para executar este comando é necessário que o repositório Git já tenha sido iniciado!

Uma vez feita esta configuração, basta criar o arquivo `.pre-commit-config.yaml` na raiz do projeto. 

> O framework fornece um arquivo de exemplo, bastando para isso executar o comando: `pre-commit sample-config > .pre-commit-config.yaml`.

Uma vez criado o arquivo de configuração, deve-se fazer a verificação dos arquivos existentes no projeto, através do comando:

```
pre-commit run --all-files
```

## Anatomia da Configuração

```yaml
# .pre-commit-config.yaml
# ---

# Configurações Gerais
default_stages:
  - commit
  - push

default_language_version:
  python: python3.8

files: '.*\.py$'
exclude: '^tests*py$'
fail_fast: true

repos:
    # Configurações Específicas
  - repo: git@github.com:PyCQA/flake8.git
    rev: v3.9.2
    hooks:
    - id: flake8
      name: Padrão PEP8
      alias: pep8
      language_version: 3.8
      files: ''
      exclude: '^$'
      types: [python]
      types_or: [python]
      exclude_types: [text]
      args: [-v, --max-line-length=80]
      stages: [commit, push, merge]
      always_run: false
      verbose: true
      log_file: 'flake_erros.log'
```

## Configuração 

A Configuração Geral, será aplicada para todos os `hooks` definidos, entretanto, caso o `hook` tenha uma especiicação diferente, a configuração específica prevalecerá.

### Geral

Parâmetros possíveis na **Configuração Geral**: 

parâmetro | definição | observação
--- | --- | ---
`default_stages` | momento em que o hook será ativado | *default*: todos os stages
`default_language_version` | linguages e suas respectivas versões | `-`
`files` | regex com o padrão de arquivos a serem incluídos na análise | *default*: `''`
`exclude` | regex com padrão de arquivos a serem excluídos da análise | *default* `'^$'`
`fail_fast` | encerra a execução ao primeiro erro | *default* `false`

### Específica

Parâmetros possíveis na **Configuração Específica**: 

parâmetro | definição | observação
--- | --- | ---
`repo` | endereço fonte usado pelo `git clone` | caso seja um item local, utilize `local`
`rev` | versão a ser utilizada | se `local` não utilizar
`hooks` | lista de definições do `hook` | `-`

### Hooks

Cada `hook` utilizado na configuração específica pode ter algum dos seguintes parâmetros:

parâmetro | definição | observação
--- | --- | ---
`id` | `hook` a ser utilizado | **obrigatório**
`name` | nome exibido durante a execução | 
`alias` | apelido para o `hook` | exemplo de uso: `pre-commit run pep8`
`language_version` | versão utilizada da linguagem | `-`
`files` | regex com o padrão de arquivos a serem incluídos na análise | *default*: `''`
`exclude` | regex com o padrão de arquivos a serem excluidos na análise | *default* `'^$'`
`types` | tipos de arquivos analisados | executa como `AND`
`types_or` | tipos de arquivos analisados | executa como `OR`
`exclude_types` | tipo de arquivo à excluir da análise | `-`
`args` | lista de argumentos passados na execução | `-`
`stages` | momento em que o hook será ativado | *default*: todos os stages
`always_run` | se definido, executará mesmo que não tenha nenhum arquivo correspondente | *default*: `false`
`verbose` | exibe todos os logs de execução | *default*: `false`
`log_file` | nome do arquivo de logs | string

### Stages

A lista de `stages` possiveis é:

- `commit`
- `merge-commit`
- `push`
- `prepare-commit-msg`
- `commit-msg`
- `post-checkout`
- `post-commit`
- `post-merge`
- `manual`

### Linguages Aceitas

- `conda`
- `coursier`
- `docker`
- `docker_image`
- `dotnet`
- `fail`
- `golang`
- `node`
- `perl`
- `pygrep`
- `python`
- `python_venv`
- `r`
- `ruby`
- `rust`
- `script`
- `swift`
- `system`

## Hooks Personalizados

Também é possível criar `hooks` personalizados, para isso, é necessário criar um arquivo `pre-commit-hooks.yaml`, conforme o exemplo:

```yaml
# pre-commit-hooks.yaml
# ---

- id: unittest
  name: Run Python Tests
  description: Run Python tests using Unittest
  entry: python -m unittest
  language: system
  stages: [commit]
```

Na criação de `hooks` personalizados e/ou locais, pode-se usar os seguintes parâmetros:

parâmetro | definição | observação
--- | --- | ---
`id` | `hook` a ser utilizado | **obrigatório**
`name` | nome exibido duranta a execução | 
`entry` | comando a ser executado | exemplo: `flake8 -v`
`language` | linguagem usada pelo `hook`
`files` | regex com o padrão de arquivos a serem incluídos na análise | *default*: `''`
`exclude` | regex com o padrão de arquivos a serem excluidos na análise | *default* `'^$'`
`types` | tipos de arquivos analisados | executa como `AND`
`types_or` | tipos de arquivos analisados | executa como `OR`
`exclude_types` | tipo de arquivo à excluir da análise | `-`
`always_run` | se definido, executará mesmo que não tenha nenhum arquivo correspondente | *default*: `false`
`verbose` | exibe todos os logs de execução | *default*: `false`
`pass_files` | se `false` os nomes dos arquivos não serão passados para o `hook` | *default* : `true`
`require_serial` | se `true` os `hooks` serão executados em processos únicos e não em paralelo | *default* : `false`
`description` | descrição usada pelo `hook` | *default*: `''`
`language_version` | versão da linguagem utilizada | *default*: `default`
`minimun_pre_commit_version` | permite indicar a versão mínima compatível do pre-commit | *default*: `0`
`args` | lista de argumentos opcionais passado ao `hook` | *default*: `[]`
`stages` | momento em que o hook será ativado | *default*: todos os stages

Todo arquivo de `hook` tem que estar em um diretório/repositório que seja passível de responder ao comando `git clone`. Neste exemplo, este `hook`, poderia ser invocado no arquivo de configuração, da seguinte forma:

```yaml
# .pre-commit-config.yaml
# ---

# Configurações Gerais
# ...

repos:
  # Configurações Específicas
- repo: https://github.com/username/project_name
  rev: 0.1.0
  hooks:
  - id: unittest
```

> `rev` refere-se a versão utilizada, podendo ser um `label` uma `release` ou até mesmo o `hash` do commite da versão desejada.

### Hooks Locais

Também é possível criar e utilizar `hooks` locais, isto é, que utilizam as configurações e dependências do projeto para executar as tarefas/funções. Ele deve possuir como `repo` o valor `local` e definir obrigatoriamente: `id`, `name`, `language`, `entry`, e `files` / `types`.


```yaml
- repo: local
  hooks:
  - id: pylint
    name: pylint
    entry: pylint
    language: system
    types: [python]
```

### CLI

O `pre-commit` possui diversos comandos úteis em sua Interface de Linha de Comando (CLI)

- `pre-commit install`: instala as dependência no diretório do `git`
- `pre-commit autoupdate`: atualiza os hooks listados
- `pre-commit autoupdate --freeze`: atualiza os hooks e troca a referência (`rev`) para a `hash` da versão atual
- `pre-commit run`: executa os `hooks`
- `pre-commit run <hook_id>`: executa um `hook` específico
- `pre-commit run <hook_id> --all-files`: executa um `hook` específico em todos os arquivos
- `pre-commit run <hook_id> --files [filename]`: executa um `hook` específico nos arquivos indicados
- `pre-commit run <hook_id> --show-diff-on-failure`: executa o `git diff` em caso de falha
- `pre-commit run <hook_id> --verbose`: exibe as saidas dos `hooks`, independente de sucesso ou falha
- `pre-commit uninstall`: desinstala as dependência no diretório do `git`
- `SKIP=first,second git commit -m "spam"`: pula os `hooks` especificado ao fazer o commit
- `git commit -m "foo" --no-verify`: não executa nenhuma verificação

### Badge

![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)

Para instalar a badge do `pre-commit` em seu repositório, utilize: 

```md
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
```

## Referências

- [Documentação Oficial](https://pre-commit.com/)
- [Documentação Oficial: Lista de Hooks](https://pre-commit.com/hooks.html)
