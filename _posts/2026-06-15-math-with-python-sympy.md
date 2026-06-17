---
layout: post
date: 2026-06-15 01:00:00 -0300
title: "SymPy"
slug: math with python sympy
tag: resumo
category: "Doing Math with Python"
comments: false
---

Quando utilizamos o Python nativo ou calculadoras tradicionais, estamos acostumados com a matemática *numérica*. Se pedirmos para o computador calcular `x + x`, ele gerará um erro se `x` não tiver um valor numérico previamente definido.

No entanto, na matemática escolar e acadêmica, frequentemente precisamos trabalhar com equações de forma abstrata — expandindo polinómios, fatorando expressões ou isolando incógnitas sem necessariamente atribuir números a elas. É para preencher essa lacuna que serve a **Matemática Simbólica**, e no ecossistema Python, a biblioteca principal para esta tarefa é o **SymPy**.

Neste artigo, vai aprender a configurar símbolos, manipular expressões algébricas e realizar substituições de forma exata.

---

## 1. O que é o SymPy e como configurar os Símbolos?

O SymPy é uma biblioteca de computação simbólica que permite escrever expressões matemáticas exatamente como as escreve num papel. Antes de começar, precisa de garantir que a biblioteca está instalada no seu ambiente:

```bash
pip install sympy

```

Diferente do Python padrão, onde as variáveis armazenam apenas valores, no SymPy precisamos de declarar explicitamente que uma variável de código representa um **símbolo matemático**.

```python
from sympy import symbols

# Definindo os símbolos x e y
x, y = symbols('x y')

# Agora podemos escrever uma expressão sem que o Python reclame que 'x' não foi definido
expressao = x + x + y
print(expressao)  # Saída: 2*x + y

```

Repare que o SymPy simplificou automaticamente `x + x` para `2*x`. O Python tratou a variável como uma entidade puramente algébrica.

---

## 2. Expansão e Fatoração de Polinómios

Duas das tarefas mais comuns na álgebra são a **expansão** (multiplicar termos para abrir uma expressão) e a **fatoração** (agrupar termos em fatores comuns). O SymPy resolve isto com duas funções simples: `expand()` e `factor()`.

### Expandindo Expressões com `expand()`

Imagine o produto notável $(x + y)^2$. Se quisermos expandi-lo para a forma $x^2 + 2xy + y^2$, usamos:

```python
from sympy import expand

expressao = (x + y)**2
expressao_expandida = expand(expressao)

print(expressao_expandida)  # Saída: x**2 + 2*x*y + y**2

```

### Fatorando Expressões com `factor()`

O caminho inverso também é automatizado. Se tivermos um polinómio complexo e quisermos transformá-lo num produto de fatores mais simples:

```python
from sympy import factor

polinomio = x**2 - y**2
polinomio_fatorado = factor(polinomio)

print(polinomio_fatorado)  # Saída: (x - y)*(x + y)

```

---

## 3. Substituição de Valores com `subs()`

Trabalhar de forma simbólica não significa que nunca iremos avaliar numericamente a expressão. O SymPy permite que construa e simplifique toda a sua equação de forma literal e, apenas no final, aplique valores numéricos às variáveis utilizando o método `.subs()`.

O método `subs` (de *substitution*) aceita um dicionário mapeando cada símbolo ao seu respetivo valor:

```python
# Criando uma expressão algébrica
equacao = x**2 + 2*x*y + y**2

# Substituindo x por 2 e y por 3
resultado = equacao.subs({x: 2, y: 3})

print(resultado)  # Saída: 25 (Pois 2² + 2(2)(3) + 3² = 4 + 12 + 9 = 25)

```

---

## 4. Apresentação Visual: "Pretty Printing" (Impressão Elegante)

Ver expressões escritas como `x2 + 2*x*y` pode tornar-se confuso à medida que as equações crescem. O SymPy possui um recurso chamado `pprint` (Pretty Print) que desenha as equações no terminal utilizando caracteres Unicode para simular a escrita matemática tradicional.

```python
from sympy import pprint

equacao = (x**2 + y**3) / (x - y)

print("Impressão padrão:")
print(equacao)

print("\nImpressão elegante (pprint):")
pprint(equacao)

```

**Resultado no terminal:**

```text
Impressão padrão:
(x**2 + y**3)/(x - y)

Impressão elegante (pprint):
 2    3
x  + y 
-------
 x - y 

```

---

## Conclusão

A transição da matemática numérica para a simbólica abre as portas para a criação de ferramentas educativas, solucionadores de equações e scripts de engenharia altamente sofisticados. Ao dominar os conceitos de símbolos, expansão, fatoração e substituição, passou a ter o controlo sobre a álgebra através do código.
