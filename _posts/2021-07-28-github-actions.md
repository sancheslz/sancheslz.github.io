---
layout: post
date: 2021-07-27 00:00:00 -0300
title: "Github Actions"
slug: github actions
tag: tutorial
category: "Terminal"
comments: false
---

## O que é

O GitHub Actions é uma ferramenta de Integração Contínua (CI) que permite a definição de comandos que deverão ser executados, de forma sequencial, para garantir a padronização e funcionalidade dos códigos. Para isso, ele utiliza um sistema baseado em eventos, onde cada evento (*event*) pré-configurado irá disparar um grupo de ações (*actions*) e seus respectivos passos (*steps*)

Para isso, é necessário a criação de um arquivo no formato `YAML`, no diretório: `.github/workflows`. Este arquivo será o responsável por definir como será a execução de cada ação, podendo haver mais de um arquivo de ação, conforme a necessidade.

## Anatomia do Arquivo

```yaml
name: Python Pipeline

env:
    PYTHON_VERSION: 3.8

on:
    push:
        branches: [ main, master ]
    pull_request:
        branches: [ main, master ]

jobs:
    workflow:
        name: Python Workflow
        runs-on: ubuntu-latest

        steps:
            - uses: actions/checkout@v2
            - uses: actions/setup-python@v2
              with: 
                python-version: {% raw %}${{ env.PYTHON_VERSION }}{% endraw %}
            
            - name: Install Dependencies
              run: |
                python -m pip install --upgrade pip
                pip install pipenv
                pipenv install --dev

            - name: Lint with Pylint
              run: |
                pipenv run pylint *.py --ignore test*.py

            - name: Test with Unittest
              run: |
                python -m unittest
```

Como pode-se observar, a configuração é realizada através da ramificação de chaves-valores, sendo a descrição de cada uma das chaves utilizadas:

chave | descrição
--- | ---
`name` | nome do arquivo de ações
`env` | definição das variáveis de ambiente
`on` | define o evento que irá disparar as ações
`on.<evento>.branches` | define em que branches o evento deve ocorrer
`jobs` | definição e sequenciamento das tarefas a serem executadas
`jobs.<job_id>` | identificador úncio do trabalho
`jobs.<job_id>.name` | nome do trabalho a ser realizado
`jobs.<job_id>.runs-on` | define sistema operacional de execução, podendo ser: `windows-latest`, `ubuntu-latest`, `macos-latest`
`jobs.<job_id>.steps` | define os passos de execução
`jobs.<job_id>.steps.uses` | seleciona uma ação a ser executada como parte do código. Por exemplo, `checkout@v2` indica que deverá ser utilizada a versão mais nova da branch e `setup-<language>` indica a linguagem que deve ser instalada
`jobs.<job_id>.steps.uses.with` | define os parâmetro a serem utilizados pela ação
`jobs.<job_id>.steps.name` | define o nome do passo a ser executado
`jobs.<job_id>.steps.run` | define a ação a ser executada

