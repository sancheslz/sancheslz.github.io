---
layout: post
date: 2026-06-15 03:00:00 -0300
title: "Aplicação Física"
slug: math with python aplicacao fisica
tag: resumo
category: "Doing Math with Python"
comments: false
---

Até agora, aprendemos a traçar linhas a partir de listas estáticas de dados e a criar símbolos para equações abstratas. O verdadeiro poder da programação científica surge quando combinamos estas duas habilidades: pegamos numa fórmula matemática que descreve uma lei da natureza, usamos o Python para calcular centenas de pontos e geramos um gráfico que nos permite prever o comportamento do mundo real.

Neste artigo, vamos modelar um problema clássico da física: o **lançamento oblíquo de um projétil**. Vamos ver como a velocidade inicial e o ângulo de disparo afetam a trajetória de um objeto sob o efeito da gravidade, desenhando a curva parabólica resultante através do Matplotlib.

---

## 1. A Matemática por Trás do Movimento

Quando um objeto é lançado no ar com uma velocidade inicial ($u$) e um ângulo ($\theta$), o seu movimento pode ser decomposto em dois eixos independentes:

1. **Eixo Horizontal ($X$):** Não há aceleração (desprezando a resistência do ar). O objeto move-se com velocidade constante:

$$x = u \cdot \cos(\theta) \cdot t$$


2. **Eixo Vertical ($Y$):** O objeto é desacelerado uniformemente pela força da gravidade ($g$):

$$y = u \cdot \sin(\theta) \cdot t - \frac{1}{2}g \cdot t^2$$



Onde $t$ representa o tempo em segundos e $g$ é a aceleração da gravidade (aproximadamente $9.81\text{ m/s}^2$).

Para plotar esta trajetória, precisamos de calcular os valores de $x$ e $y$ para pequenos intervalos de tempo, desde o momento do disparo ($t = 0$) até o objeto atingir o solo novamente ($y = 0$).

---

## 2. Traduzindo Fórmulas para Código Python

Para lidar com funções trigonométricas como o seno e o cosseno, utilizaremos o módulo nativo `math`. Como o Python trabalha com ângulos em radianos, utilizaremos também a função `math.radians()` para converter os graus do utilizador.

Vamos criar uma função reutilizável que calcula os pontos e desenha o gráfico:

```python
import matplotlib.pyplot as plt
import math

def plotar_trajetoria(velocidade_inicial, angulo_graus):
    # Constantes
    g = 9.81
    
    # Converter ângulo para radianos
    angulo_rad = math.radians(angulo_graus)
    
    # 1. Calcular o tempo total de voo (quando o objeto regressa ao solo, y=0)
    # Fórmula: t_voo = (2 * u * sin(theta)) / g
    tempo_voo = (2 * velocidade_inicial * math.sin(angulo_rad)) / g
    
    # 2. Criar intervalos de tempo pequenos (ex: 100 pontos entre 0 e t_voo)
    intervalos_tempo = []
    num_pontos = 100
    delta_t = tempo_voo / num_pontos
    
    for i in range(num_pontos + 1):
        intervalos_tempo.append(i * delta_t)
        
    # 3. Calcular as coordenadas X e Y para cada instante de tempo
    coordenadas_x = []
    coordenadas_y = []
    
    for t in intervalos_tempo:
        x = velocidade_inicial * math.cos(angulo_rad) * t
        y = (velocidade_inicial * math.sin(angulo_rad) * t) - (0.5 * g * t**2)
        coordenadas_x.append(x)
        coordenadas_y.append(y)
        
    # 4. Plotar o gráfico com Matplotlib
    plt.plot(coordenadas_x, coordenadas_y)
    plt.title(f'Trajetória do Projétil - Lançamento a {angulo_graus}°')
    plt.xlabel('Distância Horizontal (metros)')
    plt.ylabel('Altura Vertical (metros)')
    plt.grid(True)

```

---

## 3. Comparando Múltiplas Trajetórias

Uma das grandes vantagens da abordagem didática via programação é a facilidade de experimentação. O que acontece se dispararmos o objeto com a mesma velocidade, mas mudando o ângulo?

Podemos chamar a nossa função várias vezes seguidas. O Matplotlib irá, automaticamente, sobrepor as linhas na mesma janela e atribuir cores diferentes a cada uma, facilitando a análise visual.

Modificando ligeiramente o nosso script para suportar comparações e legendas:

```python
# Modificando a função para aceitar rótulos
def adicionar_curva(velocidade_inicial, angulo_graus):
    g = 9.81
    angulo_rad = math.radians(angulo_graus)
    tempo_voo = (2 * velocidade_inicial * math.sin(angulo_rad)) / g
    
    # Gerando os pontos usando List Comprehension para simplificar o código
    tempos = [i * (tempo_voo / 100) for i in range(101)]
    x = [velocidade_inicial * math.cos(angulo_rad) * t for t in tempos]
    y = [(velocidade_inicial * math.sin(angulo_rad) * t) - (0.5 * g * t**2) for t in tempos]
    
    # Adicionando a curva ao gráfico com uma etiqueta (label)
    plt.plot(x, y, label=f'{angulo_graus}°')

# Testando com diferentes ângulos e uma velocidade constante de 25 m/s
velocidade = 25
angulos = [30, 45, 60]

for angulo in angulos:
    adicionar_curva(velocidade, angulo)

# Configurações finais da janela
plt.title('Comparativo de Trajetórias com Velocidade de 25 m/s')
plt.xlabel('Distância Horizontal (m)')
plt.ylabel('Altura Vertical (m)')

# Ativando a legenda com base nos 'labels' definidos anteriormente
plt.legend()
plt.grid(True)
plt.show()

```

---

## 4. Análise Crítica dos Resultados Visuais

Ao executar o código de comparação, o gráfico gerado revela conceitos físicos cruciais de forma imediata:

* O lançamento a **$45^\circ$** atinge o maior alcance horizontal (distância máxima no eixo $X$).
* O lançamento a **$60^\circ$** atinge a maior altura máxima (eixo $Y$), mas o seu alcance horizontal é menor.
* Os ângulos complementares ($30^\circ$ e $60^\circ$) caem exatamente no mesmo ponto de alcance, embora façam parábolas com alturas muito distintas.

## Conclusão

A transição de fórmulas analíticas estáticas para simulações gráficas dinâmicas solidifica a compreensão matemática e científica de qualquer estudante ou analista. Ao estruturar problemas reais em blocos lógicos iterativos, transformamos linhas de código numa verdadeira ferramenta de experimentação laboratorial virtual.
