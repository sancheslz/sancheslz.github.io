---
layout: post
date:  2020-02-08 00:00:00 -0300
title:  "Melhorias Superficiais"
slug: a-arte-de-escrever-programas-legiveis melhorias superficiais
tag: books
category: "A Arte de Escrever Programas Legíveis"
comments: false
---

![Capa do Livro](../assets/img/a-arte-de-escrever-programas-legíveis.jpg)

A parte de "Melhorias Superficiais" aborda os pontos básicos de melhoria do código. Não se trata de alteração do código em sí, mas da escolha correta de nomes, alinhamento, comentários, etc. São recursos simples e poderosos que tornarão seu código ainda mais compreensível.

### Criação de nomes informativos

A criação de nomes informativos possui dois requisitos: *"Inclua informações em seus nomes"* e *"É melhor ser claro e preciso do que exagerar"*. Isto é, os nomes devem permitir que o leitor do código compreenda o que cada elemento (variável, função, classe, etc) sem que precise ler o código mais de uma vez.

Na busca por uma palavra mais específica, é válido utilizar sinônimos pode garantir um código muito mais compreensível. Por exemplo, ao invés de *send/enviar* como nome de um método, pode ser útil definir algo como: *deliver/entregar*, *dispatch/despachar*, *annouce/anunciar*, *distribute/distribuir* e *route/rotear*.

Sempre tenha em mente que o nome do método ou da variável deve descrever seu propósito. No caso de iteradores, nomes como `i`, `k`, `j`, `n`, não representam muita coisa. Que tipo de objeto é retornado do iterador?

Veja o exemplo de uma função que soma os quadrados de uma lista:

**Função Original**
```python
def getSum(dataset: list) -> int:
    tmp = 0
    for i in dataset:
        tmp += i ** 2
    return tmp
```

Veja alguns elementos que podem ser melhorados nessa função:

- `getSum` apesar de se compreensível, o retorno será a soma simples? Mudar o nome da função para `sum_squares`, deixa mais claro que o objetivo é retornar a soma do quadrado dos números passados;
- `tmp` é comumente utilizado para expressar uma variável temporária. Qual a função dela empregada aqui? Ela armazena o total da operação. Nomeá-la como `total` expressa muito mais sentido.
- `i` expressa um elemento do conjunto de valores passados. O que é esse `i`? Pode ser um elemento de texto? Um objeto? Nomeá-lo como `number` torna mais rápido saber o que é esperado que essa lista receba, facilitando, por exemplo, na hora de *debug*

Veja como pequenas alterações essa função simples se torna ainda mais expressiva:

**Função com Ajustes**
```python
def sum_squares(dataset: list) -> int:
    total = 0
    for number in dataset:
        total += number ** 2
    return total
```

**ATENÇÃO** quando utilizar variáveis com nomes genéricos (`tmp`, `temp`, `retail`, etc) é preciso que fique evidente que a característica principal dessa variável é ter curta duração dentro do código, além disso, sempre deve haver uma boa justificativa para essa escolha.

A definição do nome pode, muitas vezes, demandar a adição de detalhes para torná-lo ainda mais claro. Por exemplo, uma variável de tempo pode ter a definição de sua medida, de modo que ao invés de `duration` poderia ser escrita como `duration_ms` o que torna evidente que a duração retornada será em milissegundos.

Outra técnica útil é a [Notação Húngara](https://pt.wikipedia.org/wiki/Nota%C3%A7%C3%A3o_h%C3%BAngara). Nesse sistema a definição das variáveis leva como primeiro elemento o tipo da variável:

**Notação Húngara**
```python
def fn_sum_squares(rgNumbers: list) -> int:
    iTotal = 0
    for iNumber in rgNumbers:
        iTotal += iNumber ** 2
    return iTotal
```

Neste exemplo, as variáveis recebem um prefixo como o tipo de dado que a mesma terá, por exemplo a lista de valores recebe o `rg` que corresponde ao *range* ou intervalo do *array* já as demais recebem o `i` para expressar que deverão ser número inteiros. Até mesmo a função recebe um `fn` para defini-lá como uma função.

Importante lembrar que esse sistema de anotação deve estar claro para todos os membros da equipe, caso contrário, a inclusão desse elementos servirá para dificultar a leitura e interpretação do código.

Além disso, a preocupação com o tamanho dos nomes hoje não deve ser um problema graças à tecnologia de *autocomplete* presente nas IDEs. Por isso, utilize nomes curtos para escopos menores, de forma que fique claro o significado de cada elemento. Evite o uso de acrônimos e abreviações como `BKM` ou `BKManager`, prefira utilizar o nome completo como `BackEndManager`. Lembrando, evidentemente, de evitar termos desnecessários como em `ConvertToString`, basta escrever `ToString` que o sentido continuará o mesmo.

Mais importante de tudo. Seja consistente na escolha de padrões. Se optar por utilizar *underscores* para funções e variáveis e *CamelCase* para classes, utilize esse padrão em todo o seu código. Isso facilitará a leitura e o esforço para compreender cada elemento do código.

**Resumo:**

- Utilize palavras específicas
- Evite nomes genéricos
- Utilize nomes concretos
- Adicione detalhes importantes
- Utilize nomes longos para escopos maiores
- Utilize letras maiúsculas, sublinhados e outros recursos significativos
