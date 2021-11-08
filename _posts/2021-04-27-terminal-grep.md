---
layout: post
date: 2021-04-27 00:00:00 -0300
title: "Grep"
slug: terminal-grep
tag: tutorial
category: "Terminal"
comments: false
---

## O que é GREP

Iniciado em 1973, o grep - acrônimo de _Global Regular-Expression Print_ - é um programa de linha de comando com um objetivo claro: fazer buscas em arquivos à partir de expressões regulares. O comando `grep` permite que o usuário encontre um texto em um arquivo ou mais, de forma rápida e fácil.

Funciona assim: o usuário faz a busca usando o `grep` e informando um padrão de busca. O `grep` analisará o arquivo verificando todas as ocorrências do padrão informado. Encontradas as ocorrências, as mesmas serão devolvidas ao usuário.

Com isso, coisas como encontrar arquivos com marcadores como `FIXME` e `TODO` ou verificar quais arquivos importam uma determinada biblioteca que será descontinuada se torna uma tarefa simples.

## Visão Geral

```
grep [options] [regex] [filename]
```

O comando `grep` pode ser subdividido em três categorias de parâmetros:

- `options`: as opções de busca e de execução, também chamados de `flags`
- `regex`: a expressão ou padrão a ser buscado
- `filename`: nome do arquivo ou conjunto de arquivos em que a busca deverá ocorrer

Além desse padrão, o `grep` se subdivide em quatro grupos:

- `grep -G` ou `grep`: usado como modelo regular
- `grep -F` ou `fgrep`: usado para strings fixas
- `grep -E` ou `egrep`: usado expressões regulares extentidas
- `grep -P`: usado para expressões regulares no estilo `Perl`

## Exemplo de Uso

Ao desenvolver um projeto é comum deixamos comentários como `FIXME`, `HACK` e `TODO`. Para buscar todas essas ocorrências em nosso projeto, podemos utilizar o poder das expressões regulares:

```
grep -E "FIXME|HACK|TODO" -r .
```
### Informando o Padrão

Note que no exemplo acima, o padrão a ser encontrado foi passado entre aspas. Os padrões podem ser passados de quatro formas e combinados entre eles:

- `grep pattern`: utilizado para buscas de padrões simples de texto, sem espaço entre eles
- `grep 'pattern'`: utilizado para busca de padrões complexos, como expressões regulares, textos com espaços, etc.
- `grep "$env"`: utilizado para buscar a partir do resultado de uma variável de ambiente, por exemplo: `$HOME`
- ```grep `cmd` ```: utilizado para buscar a partir de um comando passado, por exemplo `whoami`

## Metacaracteres e definições POSIX

Elaborar expressões regulares, significa definir e identificar padrões que podem aparecer em nosso texto. Por exemplo, um CEP pode ser definido como um conjunto de 5 digitos, um traço e mais 3 dígitos. Com essa definição, encontrar todos os CEPs em um arquivo se torna uma tarafa fácil. Essa abstração de "qualquer caracter" ou conjunto de caracteres é o que chamamos de _metacaracter_. 

Metacaracter | Combinação
--- | ---
`.` | qualquer um caracter
`[...]` | qualquer caracter listado entre colchetes
`[^...]` | qualquer caracter não listado entre colchetes
`\char` | a barra invertida permite "escapar" caracteres que funcionam como metacaracteres, por exemplo: `\$`
`^` | começo da linha
`$` | fim da linha
`\<` | começo da palavra
`\>` | fim da palavra
`?` | caracter ou conjunto opcional
`*` | qualquer número de caracteres
`+` | uma ou mais repetição da expressão antecessora
`{N}` | exatamente `N` vezes
`{N,}` | pelo menos `N` vezes
`{min, max}` | de `min` a `max` vezes
`|` | operador lógico "ou"
`-` | indicador de intervalo
`(...)` | limita o escopo de alternação
`\1, \2, ...` | referência às combinações previamente encontradas
`\b` | combina com caracteres que tipicamente marcam o fim de uma palavra (espaço, ponto final, etc)
`\B` | forma alternativa ao `\\`
`\w` | qualquer caracter (letras, números e underscores)
`\W` | qualquer caracter não utilizado em palavras (letras, números e underscore)

Definição POSIX | Definição
--- | ---
`[:alpha:]` | qualquer caracter alfabético, independente de ser maiúsculo ou minúsculo
`[:digit:]` | qualquer caracter numérico
`[:alnum:]` | qualquer caracter alfanumérico
`[:blank:]` | espaços ou tabs
`[:xdigit:]` | qualquer caracter hexadecimal
`[:punct:]` | qualquer símbolo de pontuação
`[:print:]` | qualquer caracter imprimível
`[:space:]` | qualquer espaço em branco
`[:graph:]` | exclui caracteres de espaço em branco
`[:upper:]` | qualquer letra maiúscula
`[:lower:]` | qualquer letra minúscula
`[:cntrl:]` | caracteres de controle

## Grupo GREP

- `grep -f file_with_patterns file_name`: permite que seja informado um arquivo em que cada linha do arquivo corresponde a um padrão a ser encontrado
- `grep -i pattern file_name`: realiza a busca dos padrões informados de modo _case-insensitive_, isto é, não diferenciando maiúsculas e minúsculas
- `grep -v pattern file_name`: inverte a busca, de forma que será retornado qualquer valor que não combine com o padrão informado
- `grep -w pattern file_name`: retorna apenas os casos em que o padrão buscado corresponda completamente à uma palavra
- `grep -x pattern file_name`: retorna se o padrão buscado corresponde à uma linha do arquivo buscado
- `grep -c pattern file_name`: conta o número de ocorrências do padrão buscado
- `grep pattern file_name --color`: retorna o padrão buscado e destaca (de forma colorida) a ocorrência do padrão
- `grep -l pattern file_name`: retorna os arquivos com resultados encontrados
- `grep -L pattern file_name`: retorna os arquivos com resultados encontrados
- `grep -o pattern file_name`: retorna apenas a ocorrência e não a linha inteira
- `grep -m limit_number pattern file_name`: limita o número de ocorrências antes de retornar, utilizado, principalmente em arquivos extensos para limitar a busca à `limit_number` ocorrências
- `grep -h pattern file_name`: retorna as buscas sem exibir o nome do arquivo
- `grep -H pattern file_name`: retorna as buscas exibindo o nome do arquivo
- `grep -n pattern file_name`: retorna o número da linha em que houve a ocorrência
- `grep -A number pattern file_name`: retorna `number` linhas antes da ocorrência
- `grep -B number pattern file_name`: retorna `number` linhas após a ocorrência
- `grep -C number pattern file_name`: retorna `number` linhas antes e depois da ocorrência
- `grep pattern file_name --exclude=path`: exclui um determinado diretório ou conjunto de arquivos da busca
- `grep pattern file_name --include=path`: inclui um determinado diretório ou conjunto de arquivos da busca
- `grep -r pattern file_name`: realiza a busca de forma recursive nos diretórios ou arquivos informados

## Grupo EGREP

O grupo `egrep` ou `grep -E` extende várias funções de expressões regulares, modificando, inclusive, como alguns metacaracteres funcionam:

- `egrep 'patterns?' file_name`: torna o caracter ou conjunto anterior opcional, isto é, pode ocorrer ou não
- `egrep 'pattern+' file_name`: permite que o caracter anterior ocorra uma ou inúmeras vezes
- `egrep 'pattern{n,m}' filen_ame`: determina o número de ocorrência do caracter ou conjunto anterior
- `egrep 'pattern1|pattern2' file_name`: operador lógico `ou` permitindo que seja encontrada a primeira ou a segunda ocorrência
- `egrep 'patt(a|e)rn' file_name`: agrupa o padrão informado permitindo a construção de padrões mais complexos

## Grupo FGREP

O grupo `fgrep` ou _fixed strings_, também conhecido como _fast grep_ é um padrão de busca mais rápido que os demais, uma vez que não utiliza expressões regulares, mas sim padrões de texto precisos e específicos: `fgrep string_pattern file_name`.

## Referências

- [Documentação Oficial](https://www.gnu.org/software/grep/manual/grep.html)
- BAMBENEK, John. KLUS, Agnieszka. **grep: pocket reference**. Sebastopol/CA: O'Reilly, 2009.
