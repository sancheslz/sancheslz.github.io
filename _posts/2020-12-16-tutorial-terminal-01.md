---
layout: post
date:  2020-12-15 00:00:00 -0300
title:  "Introdução"
slug: terminal introducao
tag: tutorial
category: "Terminal"
comments: true
---

![Cena do Filme Matrix - Vários computadores rodando um código na tela preta](https://media.giphy.com/media/sk6yL9EGVeAcE/giphy.gif)

Se você já assistiu ao filme [The Matrix](https://pt.wikipedia.org/wiki/Matrix), lançado em 1999, antes do chamado "bug do milênio", provavelmente deve ter sido marcado com esta cena em que os códigos simplesmente passam em alta velocidade em uma tela preta e o programador interpreta cada um desses símbolos com uma capacidade sobre humana.

Cenas como essa, tornaram o Terminal (chamado de Prompt no Windows) em um vilão para muitas pessoas. Na realidade, o Terminal, ou Linha de Comando (*Command Line*), é um programa que permite a execução de comandos diretamente no Sistema Operacional, tal como navegação, manipulação de arquivos, execução de programas, etc. sem o uso de uma Interface Gráfica.

Existem várias vantagens no uso da Linha de Comando, dentre eles pode-se destacar:

- Velocidade de Execução das Tarefas
- Não fica limitado aos ícones e menus de uma interface
- Encadeamento de instruções
- Criação de scripts de rotinas
- Amplo uso em servidores web

![Pessoa coçando a cabeça e expressando dúvida](https://media.giphy.com/media/gGpudgwaDhBy1wjKMj/giphy.gif)

Você provavelmente está se perguntando o porquê poucas pessoas utilizam o Terminal se ele é tão vantajoso assim. Realmente, essa é uma ótima pergunta, a primeira e mais óbvia resposta é: desconhecimento. Poucas pessoas até mesmo sabem que ele existem, a maioria dos usuários comuns passará sua vida utilizando as interfaces gráficas e isso lhes será suficiente. Mas você não é um usuário comum, não? Então, deixe-me dar um exemplo:

> *João salvou vários arquivos em sua Área de Trabalho,* (quem nunca?) *e agora quer organizar esses arquivos por categoria, a partir do tipo de arquivo, ou seja, quer colocar todos os arquivos ".mp3" na pasta "Músicas", todos os arquivos ".png" e ".jpg" na pasta "Fotos" e todos os demais arquivos na pasta "Outros"*

Se o total de arquivos na Área de Trabalho de João for apenas 10, é fácil, basta clicar e arrastar, ou quem sabe, usar o `Ctrl+X` e `Ctrl+V`, não é mesmo? Selecionar cada arquivo individualmente e então colocá-los em sua pasta. Para fazer isso você levaria no máximo 5 minutos, isso se precisasse criar as pastas, não é mesmo? Mas e se ao invés de 10 arquivos houvessem 1.000 ou 100.000? Só de pensar em arrastar o mouse tantas vezes já cansa, não é mesmo?

![Pessoa dormindo e sendo cutucada por um mouse](https://media.giphy.com/media/fUGifL6kpXSZh6ANE3/giphy.gif)

Mas não desanime, vamos lá, você possui em seu computador o super, o master, o poderoso **TERMINAL**. Então vamos ajudar João em sua Tarefa com apenas poucas linhas?

```bash
# Criar as pastas
mkdir Músicas Fotos Outros

# Mover os arquivos .mp3 para Músicas
mv *.mp3 Músicas/

# Mover os arquivos .jpg e .png para Fotos
mv *.{jpg,png} Fotos/

# Mover todos os demais arquivos para Outros
mv *.* Outros/
```

Sim, com apenas quatro linhas você criou as pastas e moveu os arquivos conforme João precisava. Ainda está duvidando? Então vamos fazer um teste. Abra seu terminal e digite o seguinte:

```bash
cd Desktop/ # Abre a pasta Desktop
mkdir Teste/ # Cria uma pasta de Teste
cd Teste/ # Abre a pasta de Teste
touch {1..1252}.{mp3,png,jpg,pdf,txt,xls,doc,ppt} # Gera vários arquivos
```

Na primeira linha, você acessou sua Área de Trabalho, em seguida criou uma pasta chamada "Teste" e entrou na mesma. Por fim, criou 10.016 arquivos de 8 extensões diferentes. Abra essa pasta e observe os arquivos que foram gerados. Agora, tente organizá-los, assim como fizemos para ajudar o João. E *voilà*, todos os arquivos foram movidos corretamente. Pois é, simples assim.

![Pessoa amplianto seus paradigmas](https://media.giphy.com/media/SACoDGYTvVNhZYNb5a/giphy.gif)

Essa foi apenas uma introdução sobre o Terminal, te vejo nos próximos Tutoriais, onde abordaremos cada um dos comandos utilizados e muito mais.
