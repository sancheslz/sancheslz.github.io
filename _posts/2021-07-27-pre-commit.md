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

O `pre-commit` é um framework para o gerenciamento e manutenção de `hooks` do Git em diversas linguages, sendo aplicável ao Python, Ruby, Node, etc. Com ele, é possível que, mediante a execução de algum `stage` do Git (como o `commit` e o `push`), uma bateria de verificações de garantia da Qualidade de Software sejam executadas, tais como testes, linters, etc.

## Instalação e Configuração

Para instalar o `pre-commit`, basta utilizar o seguinte comando `pip` no Terminal:

```
pip install pre-commit
```

Uma vez instalado o framework, o próximo passo é fazer a instalação dos `hooks` no `Git`, com o seguinte comando:

```
pre-commit install
```

> **Atenção**: para executar este comando é necessário que o repositório Git já tenha sido iniciado

Uma vez feita essa configuração, basta criar o arquivo de configurações na raiz do projeto com o seguinte nome: `.pre-commit-config.yalm`. O framework fornece um arquivo de exemplo, bastando para isso utilizar o seguinte comando: `pre-commit sample-config > .pre-commit-config.yaml`. Em seguida, para garantir a execução e a verificação de arquivos já existentes, utilize o seguinte comando:

```
pre-commit run --all-files
```

## Anatomia da Configuração

```yaml
default_stages:
    - commit
    - push

default_language_version:
    python: python3.8

files: '.*\.py$'
exclude: '^tests*py$'
fail_fast: true

repos:
-   repo: git@github.com:PyCQA/flake8.git
    rev: v3.9.2
    hooks:
    -   id: flake8
        name: Padrão PEP8
        alias: pep8
        language_version: 3.8
        files: ''
        exclude: '^$'
        types: [python]
        types_or: [python]
        exclude_types: [text]
        args: [-v]
        stages: [commit, push, merge]
        always_run: false
        verbose: true
        log_file: 'flake_erros.log'
```

## Configuração 

### Geral

A Configuração Geral, será aplicada para todos os `hooks` definidos, entretanto, caso o `hook` tenha uma especiicação diferente, ela prevalecerá apenas para este `hook`.

parâmetro | definição | observação
--- | --- | ---
`default_stages` | momento em que o hook será ativado | *default*: todos os stages
`default_language_version` | linguages e suas respectivas versões | `-`
`files` | padrão de arquivos a serem incluídos na análise | *default*: `''`, regex
`exclude` | padrão de arquivos a serem excluídos da análise | *default* `'^$'`, regex
`fail_fast` | encerra a execução ao primeiro erro | *default* `false`

### Específica

parâmetro | definição | observação
--- | --- | ---
`repo` | endereço fonte do `git clone` | caso seja um item local, utilize `local`
`rev` | versão a ser utilizada | se `local` não utilizar
`hooks` | lista de definições do `hook` | `-`

### Hooks

parâmetro | definição | observação
--- | --- | ---
`id` | `hook` a ser utilizado | **obrigatório**
`name` | nome exibido duranta a execução | 
`alias` | apelido para o `hook` | exemplo de uso: `pre-commit run pep8`
`language_version` | versão utilizada da linguagem | `-`
`files` | padrão de arquivos a serem incluídos na análise | *default*: `''`
`exclude` | padrão de arquivos a serem excluidos na análise | *default* `'^$'`
`types` | tipos de arquivos analisados | executa como `AND`
`types_or` | tipos de arquivos analisados | executa como `OR`
`exclude_types` | tipo de arquivo à excluir da análise | `-`
`args` | argumentos passados na execução | `-`
`stages` | momento em que o hook será ativado | *default*: todos os stages
`always_run` | se definido, executará mesmo que não tenha nenhum arquivo correspondente | *default*: `false`
`verbose` | exibe todas as mensagens de execução | *default*: `false`
`log_file` | nome do arquivo de logs | string
`entry` | comando a ser executado | exemplo: `flake8 -v`

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

## Referências

- [Documentação Oficial](https://pre-commit.com/)