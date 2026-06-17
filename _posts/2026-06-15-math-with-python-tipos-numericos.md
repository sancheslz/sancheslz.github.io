---
layout: post
date: 2026-06-15 00:00:00 -0300
title: "Tipos Numéricos"
slug: math with python tipos numericos
tag: resumo
category: "Doing Math with Python"
comments: false
---

Quando começamos a utilizar o Python para resolver problemas matemáticos, o primeiro passo é compreender como a linguagem armazena e manipula números. Diferente de uma calculadora comum, o Python oferece um controle preciso sobre a natureza dos dados numéricos, permitindo desde cálculos simples de aritmética até a manipulação exata de frações sem perda de precisão por arredondamento.

Neste artigo, vamos explorar os tipos numéricos nativos do Python, entender a diferença crucial entre eles e aprender a trabalhar com frações de forma exata.

---

## 1. Os Tipos Numéricos Fundamentais: `int` vs `float`

O Python possui dois tipos principais de dados para representar números:

* **Inteiros (`int`):** Representam números inteiros, positivos ou negativos, sem ponto decimal (ex: `-5`, `0`, `42`). No Python 3, os inteiros têm precisão arbitrária, o que significa que o tamanho do número é limitado apenas pela memória disponível no seu computador.
* **Ponto Flutuante (`float`):** Representam números reais e são caracterizados pela presença de um ponto decimal (ex: `3.14`, `-0.001`, `2.0`). Eles seguem o padrão IEEE 754 para aritmética de ponto flutuante.

### Operações Aritméticas Básicas

Os operadores padrão funcionam como esperado, mas com uma regra importante sobre o resultado:

```python
# Adição e Subtração
print(4 + 5)   # Saída: 9 (int)
print(5.0 - 2) # Saída: 3.0 (A presença de um float força o resultado a ser float)

# Multiplicação
print(3 * 7)   # Saída: 21 (int)

```

---

## 2. A Nuance da Divisão: `/`, `//` e `%`

A divisão em Python possui três operadores distintos, cada um projetado para uma necessidade matemática específica. Compreender a diferença entre eles é fundamental para a lógica de programação.

### Divisão Padrão (`/`)

Independentemente de os operandos serem inteiros, a divisão padrão **sempre** retorna um número de ponto flutuante (`float`).

```python
print(6 / 3)  # Saída: 2.0
print(5 / 2)  # Saída: 2.5

```

### Divisão Inteira ou Chão (`//`)

Retorna a parte inteira do quociente, descartando as casas decimais (arredonda sempre para baixo, em direção ao infinito negativo).

```python
print(5 // 2)  # Saída: 2 (Quantas vezes o 2 cabe inteiramente no 5?)
print(-5 // 2) # Saída: -3 (Arredondado para baixo)

```

### Operador Resto ou Módulo (`%`)

Retorna o resto da divisão inteira. É extremamente útil para verificar se um número é par ou ímpar, ou para criar ciclos.

```python
print(5 % 2)  # Saída: 1 (Pois 5 = 2 * 2 + 1)

```

---

## 3. O Problema da Imprecisão do `float`

Computadores representam números decimais em formato binário (base 2). Como resultado, algumas frações decimais simples (como `0.1` ou `0.2`) não podem ser representadas de forma exata em binário, gerando dízimas periódicas binárias.

Observe este comportamento no interpretador Python:

```python
print(0.1 + 0.2) # Saída esperada: 0.3
                 # Saída real: 0.30000000000000004

```

Para aplicações comerciais, financeiras ou científicas que exigem precisão absoluta, essa pequena diferença pode acumular-se e gerar erros graves. É aqui que entra o módulo nativo `fractions`.

---

## 4. Manipulação Exata com o Módulo `fractions`

Para resolver o problema da perda de precisão e trabalhar diretamente com a notação de frações (numerador e denominador), o Python disponibiliza o módulo `fractions`.

Para utilizá-lo, precisamos importar a classe `Fraction`:

```python
from fractions import Fraction

# Criando uma fração: Fraction(numerador, denominador)
f1 = Fraction(1, 3)
f2 = Fraction(1, 6)

print(f1) # Saída: 1/3

```

### Operações com Frações

O Python realiza o cálculo mantendo a propriedade de fração e, de forma estonteante, **simplifica o resultado automaticamente** para a sua forma irredutível.

```python
resultado = f1 + f2
print(resultado) # Saída: 1/2 (Pois 1/3 + 1/6 = 3/6, que simplificado é 1/2)

print(f1 * 2)    # Saída: 2/3

```

### Criando Frações a partir de Strings ou Floats

Uma boa prática para evitar a imprecisão inicial do `float` ao criar uma fração é passar o valor como uma *string*:

```python
# Maneira incorreta (traz a imprecisão do float para a fração)
print(Fraction(0.1)) 
# Saída: 3602879701896397/36028797018963968

# Maneira correta (usando string)
print(Fraction('0.1')) 
# Saída: 1/10

```

---

## 5. Dica do Especialista: Capturando Numerador e Denominador

Uma vez que você tem um objeto `Fraction`, você pode extrair o numerador e o denominador isoladamente para usar em outras partes da sua lógica de computação ou formatação visual:

```python
frac = Fraction('3/4')
print("Numerador:", frac.numerator)     # Saída: 3
print("Denominador:", frac.denominator) # Saída: 4

```

## Conclusão

Dominar a diferenciação entre `int` e `float`, compreender as três vertentes da divisão (`/`, `//`, `%`) e saber quando aplicar o módulo `fractions` garante que os seus programas em Python tenham uma base matemática sólida e livre de comportamentos inesperados.
