---
layout: post
date: 2026-06-15 02:00:00 -0300
title: "Equações e Sistemas Lineares"
slug: math with python equacoes e sistemas lineares
tag: resumo
category: "Doing Math with Python"
comments: false
---

Na jornada da matemática e da ciência de dados, resolver equações para isolar uma incógnita ou encontrar os pontos de intersecção entre diferentes funções são tarefas rotineiras. Fazer isso manualmente para equações complexas ou sistemas com muitas variáveis torna-se rapidamente exaustivo e propenso a erros.

No artigo anterior, vimos como o SymPy lida de forma brilhante com expressões literais. Hoje, daremos um passo adiante: aprenderemos a usar a função `solve()` do SymPy para solucionar equações de primeiro e segundo grau, além de sistemas lineares inteiros, de forma totalmente automatizada e exata.

---

## 1. O Conceito de Equações no SymPy

Para que o SymPy consiga resolver uma equação, precisamos de compreender como ele a interpreta. No papel, escrevemos equações com um sinal de igual, por exemplo:

$$x + 5 = 7$$

No entanto, em Python, o sinal `=` é um operador de atribuição. Para contornar isso, a função `solve()` assume por padrão que **qualquer expressão passada para ela é igual a zero ($= 0$)**.

Portanto, antes de programar, precisamos de reorganizar a equação algebraicamente para igualá-la a zero.

* Para resolver $x + 5 = 7$, nós subtraímos 7 de ambos os lados, transformando-a em:
$$x + 5 - 7 = 0 \implies x - 2 = 0$$



```python
from sympy import symbols, solve

x = symbols('x')

# Criando a expressão (que o SymPy interpretará como igual a zero)
equacao = x + 5 - 7

# Solucionando para x
solucao = solve(equacao, x)

print(solucao)  # Saída: [2]

```

O SymPy retorna uma lista contendo todas as soluções possíveis (neste caso, apenas o número 2).

---

## 2. Resolvendo Equações Quadráticas (Segundo Grau)

Equações de segundo grau (como $x^2 + 5x + 6 = 0$) podem ter até duas raízes reais ou complexas. O SymPy identifica o grau da equação automaticamente e calcula as raízes exatas sem que precise de aplicar manualmente a fórmula de Bhaskara.

Como a equação já está igualada a zero, podemos passá-la diretamente:

```python
# Definindo a equação x² + 5x + 6 = 0
equacao_quadratica = x**2 + 5*x + 6

# Resolvendo para x
raizes = solve(equacao_quadratica, x)

print("As raízes da equação são:", raizes)  
# Saída: As raízes da equação são: [-3, -2]

```

### Lidando com Raízes Complexas ou Irracionais

Se a equação tiver soluções que envolvem números complexos ou raízes não exatas, o SymPy mantém a representação matemática perfeita em vez de arredondar para números decimais aproximados:

```text
equacao_complexa = x**2 + 1
print(solve(equacao_complexa, x))  # Saída: [-I, I] (onde I é a unidade imaginária)

```

---

## 3. Resolução de Sistemas de Equações Lineares

Muitas vezes, lidamos com problemas onde precisamos de encontrar valores para múltiplas variáveis simultaneamente. Imagine o seguinte sistema linear clássico:

1. $2x + 3y = 7$
2. $x - y = 1$

Para resolver este sistema no SymPy, convertemos ambas as equações para o formato igualado a zero e passamos uma lista com as equações e outra com as variáveis que desejamos descobrir:

```python
from sympy import symbols, solve

x, y = symbols('x y')

# Convertendo as equações para igualar a zero
eq1 = 2*x + 3*y - 7
eq2 = x - y - 1

# Resolvendo o sistema para x e y
solucao_sistema = solve([eq1, eq2], (x, y))

print(solucao_sistema)  # Saída: {x: 2, y: 1}

```

O resultado é devolvido num dicionário Python muito prático, indicando com precisão que $x = 2$ e $y = 1$.

---

## 4. Aplicação Prática: Ponto de Equilíbrio (Break-Even Point)

Para contextualizar o uso de sistemas de equações, imagine um cenário de negócios em que queira descobrir quantas unidades de um produto precisam de ser vendidas para cobrir os custos operacionais (Ponto de Equilíbrio).

* **Função de Custo Total:** $Custo = 4x + 1000$ (onde 1000 é o custo fixo e 4 é o custo de fabricação por unidade).
* **Função de Receita Total:** $Receita = 9x$ (onde o produto é vendido a 9 por unidade).

O ponto de equilíbrio ocorre quando o $Custo = Receita$, ou seja, $Custo - Receita = 0$.

```python
x = symbols('x')  # x representa a quantidade de produtos

custo = 4*x + 1000
receita = 9*x

# Ponto de equilíbrio: receita - custo = 0
ponto_equilibrio = solve(receita - custo, x)

print(f"É necessário vender {ponto_equilibrio[0]} unidades para atingir o ponto de equilíbrio.")
# Saída: É necessário vender 200 unidades para atingir o ponto de equilíbrio.

```

## Conclusão

A automatização de resoluções algébricas com o método `solve()` transforma o Python numa calculadora científica e analítica robusta. Seja para verificar trabalhos académicos, validar modelos económicos ou automatizar regras de negócio complexas, o uso correto de símbolos e equações estruturadas poupa tempo e elimina falhas humanas de cálculo.
