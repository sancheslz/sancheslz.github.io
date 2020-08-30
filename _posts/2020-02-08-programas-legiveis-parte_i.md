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


### Nomes que não podem ser mal interpretados

Uma preocupação que deve existir ao criar nome dos elementos em seu código é como ele será interpretado, como alerta os autores *"Que outros significados alguém poderia extrair deste nome?"*. Veja um exemplo trazido no livro:

```python
results = Database.all_objects.filter('year <= 2011')
```

Nesse código, é possível fazer duas interpretações de qual será o valor contido em *results*:

- Objetos cujo ano é menor ou igual à 2011
- Objetos cujo ano **não** é menor ou igual a 2011

Note que o problema está na palavra `filter`, pois a mesma não expressa claramente o objetivo pretendido. Termos como `select` ou `exclude` fariam o filtro se tornar mais evidente para outras pessoas que lessem o código.

Outro problema comum é nos intervalos inclusivos/exclusivos. O último elemento estará presente? Para suprir esse problema, uma dica é utilizar:

- `first` e `last` para os modelos inclusivo/inclusivo
- `begin` e `end` para os modelos  inclusivo/exclusivo

```html
fisrt                   last     => func(first, last) => [a, b, c, d, e, f]
| a | b | c | d | e | f | g |   |
begin                        end => func(begin, end)  => [a, b, c, d, e, f, g]
```


Variáveis booleanas são outro elemento que trazem dificuldade de interpretação, `authenticated` significa que o usuário está autenticado ou que deve se autenticar? Para evitar ruídos na interpretação, use palavras como `is_` e `has_`. Por exemplo:

```python
if user.is_authenticated() and user.has_permission():
    ...
```

**ATENÇÃO** evite ao máximo termos negados na construção de variáveis booleanas, tal como `disable_ssl`, prefira deixá-los em uma forma afirmativa `user_ssl`.

### Estética

Um item muito negligenciado na maioria dos códigos é a sua estética. Por estética, diz-se a organização e disposição dos elementos de forma a facilitar a leitura. Por isso, sempre siga esses três princípios:

- utilize layouts consistentes e padrões com os quais o leitor possa se acostumar
- faça com que códigos semelhantes tenham um visual semelhantes
- agrupe linhas de código relacionado em blocos

Em linguagens como Python, que possuem um forte senso estético, seguir essas regras se torna fácil, mas imagine um código em outra linguagem, como este apresentado pelo autor:

```java
class StatsKeeper {
    public:
    // Uma classe para monitorar uma série de doubles
    void add(double d); // e métodos para obter estatísticas sobre eles
    private: int count; /* quanto até agora
    */ public: 
        double Average();

    private:    double minimum;
    double Average();
    list<double>
        past_items
        ; double maximum;
};
```

Sobre o aspecto funcional, um código como esse pode funcionar perfeitamente, mas quantas vezes foi necessário relê-lo ou quanto tempo foi gasto para entendê-lo? A questão estética torna tudo mais fácil:

```java
// Uma classe para monitorar uma série de doubles
// e métodos para obter estatísticas sobre eles
class StatsKeeper {
    public:
        void Add(double d);
        double Average();

    private:
        list<double> past_items;
        int count; // quantos até agora

        double minimum;
        double maximum;
}
```

Outro recurso poderoso no quesito estético são as quebras de linha. Com os espaçamentos e quebras de linha, o código pode se tornar muito mais legível. Veja três formas de apresentar o mesmo código:

```java
public class PerformanceTester {
    public static final TcpConnectionsSimulator wifi = new TcpConnectionSimulator(
        500, /* Kbps */
        80, /* latência em milissegundos */
        200, /* oscilação */
        1 /* % da perda de pacotes */
    );
}
```

```java
public class PerformanceTester {
    public static final TcpConnectionsSimulator wifi = 
        new TcpConnectionSimulator(
            500,    /* Kbps                         */
            80,     /* latência em milissegundos    */
            200,    /* oscilação                    */
            1       /* % da perda de pacotes        */
        );
}
```

```java
public class PerformanceTester {
    // TcpConnectionSimulator( throughput, latency, jitter, packet_loss)
    //                           [Kbps]      [ms]    [ms]   [percentual]
    public static final TcpConnectionsSimulator wifi = 
        new TcpConnectionSimulator(500,       80,     200,       1);
}
```

Isoladamente pouca parece ser a diferença, mas se esse teste fosse feito com vários tipos de conexões. O segundo formato ganharia vantagem sobre o primeiro, pois os parâmetros e suas informações ficam organizadas, facilitando a consulta. Já o terceiro modelos supera o segundo, pois fica fácil saber a qual valor cada item corresponde.

O terceiro exemplo gera um fenômeno denominado *gestalt*, onde indiretamente o leitor "vê" a divisão dos itens como colunas. O que permite o agrupamento de itens e a separação dos mesmos. 

```python
city     = requests.GET('city')
state    = requests.GET('state')
country  = requests.GET('country')
username = requests.GET('username')
```

Note como a criação dessas colunas evidencia o papel de cada item e permite que a leitura do código se torne mais fluída. O mesmo acontece com listas e dicionários:

```python
states = [
    {'state': 'SP',   'capital': 'Sao_Paulo',         'year': '2010'},
    {'state': 'MG',   'capital': 'Belo_Horizonte',    'year': '2010'},
    {'state': 'SC',   'capital': 'Florianopolis',     'year': '2009'},
    {'state': 'GO',   'capital': 'Goiania',           'year': '2012'},
    {'state': 'RJ',   'capital': 'Rio_de_Janeiro',    'year': '2007'},
]
```

Ser consistente em uma padrão é extremamente importante para legibilidade do código. Seja na separação do código em blocos, na ordem com que os itens são passados, na criação de quebras de linha em códigos extensos.

**Mantenha a ordem com que os elementos são apresentados**
```python
user = {
    'username': '',
    'name':     '',
    'email':    '',
}
```

Quando temos a ordem de apresentação de elementos, é necessário preservar a mesma, isso facilita na busca de informaçÕes e correspondências:

```python
# Não faça isso
user['name'] = 'Bob Johns'
user['email'] = 'bob.johns@gmail.com'
user['username'] = 'bjohns'

# Faça isso
user['username'] = 'bjohns'
user['name'] = 'Bob Johns'
user['email'] = 'bob.johns@gmail.com'
```

Criar quebras de linhas, separando o aspecto de cada etapa do código, facilita na compreensão do qeu cada pequeno bloco está executando.

**Crie quebras de linhas**
```python
def suggest_new_friends(user, email_password):
    # Acessa os endereços de e-mail dos amigos do usuário
    friends = user.friends()
    friends_emails = set(f.mail for f in friends)

    # Importa todos os endereços de e-mail da conta de e-mail deste usuário
    contacts = import_contacts(user.email, email_password)
    contacts_emails = set(c.email for c in contacts)

    # Encontra usuários correspondentes com os quais ele/ela ainda não tem amizade
    non_friends_emails = concact_emails - friends_emails
    suggested_friends = User.objects.select(email__in=non_friends_emails)

    # Apresenta essas listas na página
    display['user'] = user
    display['friends'] = friends
    display['suggested_friends'] = suggested_friends

    return render('suggested_friends.html', display)

```

**Resumo**

- Se vários blocos de código estão realizando tarefas semelhantes, tente fazer com que tenham a mesma silhueta
- Alinhe partes de seu código em "colunas"para facilitar a leitura
- Se o código menciona A, B e C em um local, evite dizer B, C e A em outro. Escolha uma ordem significativa e se atenha a ela
- Utilize linhas em branco para quebrar blocos extensos em parágrafos lógicos

**Lembre-se:** utilizar um estilo consistente é mais importante do que escolher o estilo "certo"

### Como saber o que comentar

Em essência, os comentários devem aparecer para facilitar a compreensão do código. Por isso, comentários sobre o porque algo foi criado de tal forma, erros já percebidos, mas deixados propositalmente, restrições e motivos de constantes serem aplicados, etc. tornam a leitura fluída. Os comentários devem garantir que o leitor não se percam em detalhes, mas sim que tenha a capacidade de entender a essência do código e assim, analisar com facilidade seu funcionamento.

**O que não deve ser comentado:**

- fatos que podem ser rapidamente deduzidos a partir do próprio código
- "comentários-muleta" que compensam códigos ruins (como um nome de função inadequada) - é melhor corrigir seu código

**Pensamentos que devem ser registrados:**

- informações que explicam o funcionamento do seu código (o "comentário do diretor")
- falhas de seu código, utilizando marcadores como `TODO: `
- a "história" que conta como uma função recebeu seu valor

_Lista de Possíveis Marcadores_

| Marcador | Uso |
| --- | --- |
| TODO | Elementos ainda não implementados |
| FIXME | Código que sabidamente apresenta problemas | 
| HACK | Solução admitidamente deselegante para um problema | 
| XXX | Perigo! Há um grande problema aqui | 

**Coloque-se na posição do leitor:**

- antecipe quais partes de seu código farão seus leitores pensarem "Hein?" e crie comentários explicando-as
- documente comportamentos surpreendentes que talvez sejam inesperados
- utilize comentários "abrangente" no nível do arquivo/classe para explicar como todos os elementos de seu código se encaixam
- resuma blocos de código com comentários para que o leitor não se perca nos detalhes


### Crie comentários precisos e compactos

A ideia-chave na criação de comentários é que os mesmos possuam alta taxa de informação-por-espaço. Ou seja, em poucas palavras/linhas deve ser possível ter as informações necessárias para compreender o código. Por isso, seguem algunas recomendações:

Evite pronomes ambíguos como "ele/ela" e "isso" quando puderem ser mal interpretados.

```python
# Evite isso
# Insere o dado no cache, verificando primeiro se ele não é grande demais

"""
Quem é grande demais? O dado ou o cache?
"""

# Faça isso
# Se o dado for pequeno o bastante, insira-o no cache
```

Descreva o comportamento de uma função com o máximo de precisão e praticidade e utilize exemplos de entrada/saída para ilustrar situações confusas

```python
# Evite isso
# Retorna o número de linhas do arquivo
def count_lines(filename):
    ...

# Faça isso
# Conta quanto bytes de nova linha ('\n') existem no arquivo
# Exemplo: "hello\n cruel\n world\n" => 3
def count_lines(filename):
    ...
```

Declare a intenção abrangente do seu código, em vez de se concentrar em detalhes óbvios. Por exemplo, ao invés de dizer: `Itera a lista na ordem reversa`, diga algo como `Apresenta cada preço, do maior para o menor`.

Por fim, utilize palavras que carreguem o máximo de significado possível. Por exemplo, os dois comentários abaixo retratam a mesma coisa:

```python
# Esta classe contém membros que armazenam a mesma informação do banco de dados, mas
# que foram armazenados aqui por velocidade. Quando esta classe for lida no futuro,
# verificaremos primeiro se esses membros existem, e, se afirmativo, eles serão
# retornados; do contrário o banco de dados será lido e armazenaremos os dados nesses
# campos para uso futuro
```

```python
# Esta classe atual como uma camada de cache para o banco de dados
```
