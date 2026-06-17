---
layout: post
date: 2026-06-15 03:00:00 -0300
title: "Matplot"
slug: math with python matplot
tag: resumo
category: "Doing Math with Python"
comments: false
---

Um dos superpoderes de alinhar matemática com programação é a capacidade de dar vida aos números. Em vez de olhar para tabelas extensas ou equações abstratas, podemos criar representações visuais que revelam padrões, tendências e anomalias instantaneamente.

No ecossistema Python, a biblioteca mais tradicional e utilizada para esta tarefa é o **Matplotlib**. Especificamente, utilizaremos o seu módulo `pyplot`, que disponibiliza uma interface muito semelhante à de softwares científicos como o MATLAB.

Neste artigo, vai aprender a traçar pontos num plano cartesiano, criar gráficos de linhas a partir de sequências de dados e personalizar eixos e marcadores para criar relatórios visuais claros.

---

## 1. O Plano Cartesiano Digital: Instalando e Importando

Antes de desenhar qualquer linha, precisamos de garantir que o Matplotlib está instalado no ambiente de trabalho:

```bash
pip install matplotlib

```

Na matemática tradicional, um ponto no plano cartesiano é definido por um par ordenado $(x, y)$. Para criar um gráfico no Matplotlib, fornecemos duas sequências de dados (geralmente listas): uma contendo todas as coordenadas do eixo horizontal ($X$) e outra com as correspondentes do eixo vertical ($Y$).

```python
import matplotlib.pyplot as plt

# Definindo os pontos: (1, 2), (2, 4), (3, 1) e (4, 5)
eixo_x = [1, 2, 3, 4]
eixo_y = [2, 4, 1, 5]

# Criando o gráfico de linhas que conecta estes pontos
plt.plot(eixo_x, axis_y)

# Exibindo a janela com o gráfico
plt.show()

```

Ao executar este código, o Matplotlib abre automaticamente uma janela interativa onde pode fazer zoom, mover o gráfico e guardar a imagem diretamente.

---

## 2. Personalizando o Gráfico: Marcadores, Títulos e Legendas

Um gráfico técnico precisa de ser autoexplicativo. Linhas simples e sem identificação não comunicam nada. Vamos enriquecer o nosso visual adicionando marcadores nos pontos exatos, rótulos nos eixos e títulos interpretativos.

```python
import matplotlib.pyplot as plt

meses = ['Jan', 'Fev', 'Mar', 'Abr']
vendas = [10, 15, 7, 19]

# Customizando a linha:
# color='green' altera a cor
# marker='o' adiciona círculos nos pontos exatos
# linestyle='--' transforma a linha contínua numa linha tracejada
plt.plot(meses, vendas, color='green', marker='o', linestyle='--')

# Adicionando informações contextuais
plt.title('Evolução de Vendas no Primeiro Quadrimestre')
plt.xlabel('Meses de Atuação')
plt.ylabel('Unidades Vendidas (em milhares)')

# Ativando as linhas de grelha ao fundo para facilitar a leitura dos valores
plt.grid(True)

plt.show()

```

---

## 3. Controlando os Limites dos Eixos com `axis()`

Por padrão, o Matplotlib tenta ajustar automaticamente as margens do gráfico para que todos os dados caibam perfeitamente na tela. No entanto, em muitas análises matemáticas ou financeiras, omitir a origem (o ponto $0,0$) ou cortar as extremidades pode distorcer a percepção dos dados.

Podemos fixar manualmente os limites mínimos e máximos visualizados utilizando a função `axis()`, passando uma lista com o formato `[xmin, xmax, ymin, ymax]`:

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [3, 5, 4, 8, 7]

plt.plot(x, y, marker='s') # 's' cria marcadores em formato de quadrado (square)

# Forçando o gráfico a mostrar o eixo X de 0 a 6, e o eixo Y de 0 a 10
plt.axis([0, 6, 0, 10])

plt.title('Gráfico com Escala Controlada')
plt.grid(True)
plt.show()

```

---

## 4. Guardando o Gráfico Automaticamente com `savefig()`

Se estiver a criar um script automatizado que gera relatórios diários ou processa volumes de dados em segundo plano, não vai querer que uma janela interativa se abra a cada execução. O comando `savefig()` permite exportar o gráfico gerado diretamente para um arquivo de imagem (PNG, PDF, SVG, etc.).

```python
import matplotlib.pyplot as plt

x = range(1, 11)
y = [i**2 for i in x] # Uma curva quadrática simples (y = x²)

plt.plot(x, y, color='blue')
plt.title('Curva Quadrática de Exemplo')

# Guardando o gráfico na mesma pasta do script
# dpi=300 garante uma qualidade de alta resolução para impressão
plt.savefig('meu_primeiro_grafico.png', dpi=300)

# Importante: feche o gráfico após salvar para libertar memória do sistema
plt.close()

```

## Conclusão

A visualização de dados com Matplotlib converte equações abstratas e listas numéricas estáticas em ferramentas visuais intuitivas. Compreender a mecânica de mapeamento de listas para coordenadas e saber como documentar o gráfico com títulos, grelhas e limites fixos é a base para qualquer apresentação de relatórios analíticos ou científicos de qualidade.
