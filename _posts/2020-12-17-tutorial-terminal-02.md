---
layout: post
date:  2020-12-16 00:00:00 -0300
title:  "Navegação"
slug: terminal navegacao
tag: tutorial
category: "Terminal"
comments: true
---

![Duas pessoas apontando direções no mapa](https://media.giphy.com/media/8clNRa0KG1IngNcoPD/giphy.gif)

> \- Eu só queria saber que caminho tomar, pergunta Alice. \
> \- Isso depende do lugar aonde quer ir, diz o Gato tranquilamente. \
> \- Realmente não importa, responde Alice. \
> \- Então não importa que caminho tomar, afirma o Gato taxativo. 

Certamente você já viu ou ouviu esse diálogo da história da [Alice no País das Maravilhas](https://pt.wikipedia.org/wiki/Alice_no_Pa%C3%ADs_das_Maravilhas). Para ir para algum lugar, devemos saber onde queremos ir e também onde estamos. E se fizessemos essas perguntas para o computador, o que será que ele diria? Quais direções ele nos indicaria?

> **Nota** abaixo será apresentado o formado `terminal$`, apenas de modo ilustrativo, o texto antes do cifrão aparecerá de acordo com o seu computador. Quando o texto aparecer sem o cifrão significa que se trata de uma resposta do computador.

Perguntar não ofende, não é mesmo? Será que perguntar ao computador coisas como "Onde estou?", "Onde posso ir?" ou até mesmo "Quem sou eu?" são coisas possíveis?

Que tal testar essas opções? Vamos lá:

```
terminal$ Onde estou?
bash: Onde: command not found
```

Perguntamos ao para o computador e obtivemos uma resposta, mas o que ele disse? Quando passamos um comando para o computador, ele fará o processamento tentando interpretar o que dissemos e então apresentar uma resposta, porém seria muito difícil para ele processar todos os possíveis idiomas e formas de perguntarmos algo. Por isso, o computador possui parâmetros bem definidos, os quais devem ser informados corretamente para obtermos a resposta.

![Hommer Simpsom buscando uma palavra no dicionário](https://media.giphy.com/media/3o6Mb5EPQiC8zByLXq/giphy.gif)

Quando digitamos "Onde estou?" o computador tentou encontrar o comando "Onde", como se fosse em um dicionário, mas esse comando não é conhecido por ele, e foi exatamente o que ele respondeu "*command not found*" (comando não encontrado). Então qual seria forma correta de perguntarmos ao computador onde estamos?

Vamos tentar de outra maneira:

```
terminal$ pwd
/Users/sanches
```

Hum... Dessa vez tivemos uma resposta diferente, não parece um erro. Mas o que realmente fizemos? Primeiro perguntamos ao computador: Computador, por favor, imprima o caminho do diretório em que estou trabalhando (*Print Working Directory*). Ele então recebeu esse comando, buscou em sua lista de comandos disponíveis, executou o que foi pedido e imprimiu na tela para você.

No meu caso, ele mostra que estou na pasta do meu usuário que está dentro das pastas de usuários do computador. Note que a resposta pode variar de acordo com seu computador e em qual pasta você estiver.

Certo, então já sabemos onde estamos, concluímos nosso primeiro passo. Agora gostaríamos de saber onde podemos ir, certo? Que tal se pedíssemos para o computador nos listar todos os caminhos possíveis a partir de onde estamos? Assim fica mais fácil decidir para onde ir.

Então, vamos lá, digite o seguinte:

```
terminal$ ls
Applications    Downloads       Music
Desktop         Library         Pictures
Documents       Movies          Public
```

O comando `ls` diz ao computador fazer *lists* (listas), com as opções que temos de navegação. No meu caso, posso ir para vários lugares tal como a pasta de Programas (*Applications*), de Arquivos Baixados (*Downloads*), entre outros. Que tal se começarmos explorando a nossa Área de Trabalho (*Desktop*)?

Quero, neste caso, mudar meu diretório atual de `/Users/sanches` para `/Users/sanches/Desktop`. A mudança do diretório pode ser feita através do comando *Change Directory* (Mudar Diretórios), bastando informar o destino que queremos:

```
terminal$ cd Desktop
terminal$ pwd
/Users/sanches/Desktop
terminal$ ls
terminal$
```

O computador processou a mudança do diretório, a qual foi confirmada através do comando `pwd`, em seguida, verificamos o que está na Área de Trabalho através do comando `ls`. No meu caso, como não há nenhum pasta ou arquivo, o computador não teve nada para exibir.

Que tal criarmos uma pasta para começarmos nossos experimêntos? Para criar uma pasta *Make Directory* precisamos passar o comando `mkdir` seguido do nome da pasta que desejamos criar. Por padrão, pastas não possuem espaços, mas hífens (`-`) ou underscore (`_`), entretanto, caso seja necessário fazer um nome composto com espaço, o mesmo deve ser escrito entre aspas. Vejamos três exemplos:

```
terminal$ mkdir fotos
terminal$ mkdir musicas_legais
terminal$ mkdir "Viagem dos Sonhos"

terminal$ ls
Viagem dos Sonhos
fotos
musicas_legais
```

Observe que criamos as três pastas e em seguida verificamos que as três realmente aparecem na nossa Área de Trabalho. Vamos então entrar na pasta de fotos usando o comando `cd fotos`.

Ops... entramos na pasta errada, queríamos ter entrado na pasta de `musicas_legais`. E agora? Será que precisamos fechar o Terminal e fazer tudo de novo?

![Dois cachorros que em um primeiro momento parecem uma vassoura](https://media.giphy.com/media/73pf3UEa7L1du/giphy.gif)

Lembra que o comando `ls` lista todos os itens e caminhos possíveis? Pois é, mas nem todos os caminhos são exibidos por padrão, eles ficam "ocultos", por isso precisamos passar um parâmetro, também chamdo de *flag* para o comando de modo a exiber todas as suas opções, ocultas ou não, neste caso a flag `-a` (*all*):

```
terminal$ ls -a
. 
..
```

Ué, ele só mostrou uns pontos inúteis, no que isso nos ajuda? Bem esses pontos, fazem parte do sistema de roteamento dos diretórios, o ponto único significa a pasta atual (`.`) e os dois pontos a pasta anterior (`..`).

Então, para irmos novamente para a nossa Área de Trabalho, basta mudarmos o diretório para o `..`. Bastanto, para isso, digitamos `cd ..`. Mas espere, nosso objetivo não é ir para o Desktop, mas sim para a pasta de `musicas_legais`, por isso, ao invés de usar `cd ..` e depois `cd musicas_legais`, podemos usar os dois de uma única vez, dessa forma:

```
terminal$ cd ../musicas_legais
terminal$ pwd
/Users/sanches/Desktop/musicas_legais
```

Uma última coisa, digitar o nome completo de uma pasta/arquivo, não é a coisa mais agradável do mundo, não é mesmo? Que tal deixar o computador fazer o trabalho pesado apenas com uma ajudinha? Ao invés de digitar `Desktop`, poderiamos ter digitado apenas `De` e pressionar a tecla `Tab`. Com isso, o computador faria a busca dos arquivos/pastas que possuam o padrão informado e completar o que falta. 

![Joe, personagem da série Friends vestido de capitão](https://media.giphy.com/media/W5O7qeDQiTx0vEXTKn/giphy.gif)

Agora que você já sabe navegar pelo seu computador utilizando o Terminal, pode ir tranquilamente para onde quiser. Treine bastante a sua navegação e até o nosso próximo tutorial.