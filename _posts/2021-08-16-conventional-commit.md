---
layout: post
date: 2021-08-16 00:00:00 -0300
title: "Conventional Commmit"
slug: conventional-commit
tag: tutorial
# order: 6
category: "Terminal"
comments: false
---

## O que é Conventional Commit

A especificação do *Conventional Commit* é uma convenção leve que proporciona um conjunto de regras para criar *commits* que explicitem a história do projeto. Ele se baseia nas definições de versões do [SemVer](https://semver.org/). Dessa forma, permite a rápida identificação de novas funcionalidades, correções de *bugs*, mudanças de versão, entre outros, tudo através da padronização do *commit*.

SemVer | Conventional Commit
--- | ---
Major (XX.yy.zz) | breaking change
Minor (xx.YY.zz) | feat
Patch (xx.yy.ZZ) | fix

> A Regra é: faça tantos *commits* quanto forem necessários, use as quebras para manter tudo organizado

## Anatomia Geral

No *Conventional Commit*, o histórico possui itens opcionais e itens obrigatório, podendo ter a seguinte estrutura:

```console
<type> (<scope>): <short_description>

<long_description>

<key>: <description>
```

- `type`: (obrigatório) define a natureza do *commit* realizado, isto é, se é uma nova funcionalidade, um novo teste, etc.
- `scope` (opcional): escopo de alteração deste *commit*, se afeta um rota, um serviço, uma interface, etc.
- `short_description`: descrição curta das alterações realizadas, sendo máximo 70 caracteres
- `details` (opcional): bloco de detalhes
- `key` (opcional): rodapé com especificações da *issue* encerrada, responsável pela tarefa ou da definição da nova versão (`BREAKING CHANGE`). Recomenda-se o uso do padrão `kebab-case`

## Tipos

Seguindo a convenção da equipe do Angular, os tipos são classificados da seguinte forma:

Tipo | Descrição
--- | ---
`chore` | adição ou alteração nas configurações do projeto
`build` | afeta o build do sistema ou dependências externas
`ci` | altera as configuração do CI e seus scripts
`docs` | após mudanças na documentação
`feat` | implementação de nova funcionalidade
`fix` | correção de algum bug
`perf` | código que melhora a performance
`refactor` | melhora de código, sem alteração das funcionalidades ou corrigir um bug
`style` | mudanças que não afetam o significado do código (espaços, formatação, pontos-e-virgulas faltantes, etc)
`test` | adição ou correção de testes existentes

## Breaking Changes

As alterações de versão podem ser feitas de duas formas:

- Adicional no footer o padrão: `BREAKING CHANGE: <description>`
- Adicional uma exclamação após o tipo/escopo: `type (scope)!: <description>`

## Referências

- [Documentação Oficial](https://www.conventionalcommits.org/en/v1.0.0/)
- [Tipos do Angular](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)
