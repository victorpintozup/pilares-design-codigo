* contexto importa para a análise da carga cognitiva
* descreve os componentes mais comuns numa aplicação web, que geralmente não tem operação de uso intenso de memória. 
* supõe o problema... tem muita literatura que te faz olhar para camadas e escreve textos sobre quando criar coisas(controllers, service, vo, model, repository), mas eu sinto que tudo parece mais uma história de drama do que computação. Um código, por mais que represente uma necessidade de negócio, é um troço lógico, então show me the logic. Minha mente tem problemas em entender quando duas pessoas debatem o papel de um domain service. Ou elas não leram o capítulo do livro ou o capítulo realmente não é tão claro. Ex:

When a significant process or transformation in the domain is not a natural responsibility of an ENTITY or VALUE OBJECT, add an operation to the model as standalone interface declared as a SERVICE. Define the interface in terms of the language of the model and make sure the operation name is part of the UBIQUITOUS LANGUAGE. Make the SERVICE stateless.

* que um código não é 100% coerente, você não precisa ser 100% coeso o tempo todo
* Essa aqui, para mim, é a arquitetura do mundo real. Da plebe, do proletariado, de quem faz aplicação web que recebe um request, trabalha sobre ele e grava ou recupera dados do banco. 

DDD do proletariado

## O DDD está presente no nosso dia a dia

Podemos dizer que o livro mais influente entre as práticas de software dos dias atuais é o DDD. As sugestões do livro são amplamente disseminadas e é fácil perceber que muita equipe mundo a fora tentando aplicar tais ideias em seus sistemas. Inclusive existem frameworks que mencionam o livro regularmente como inspiração para algumas de suas evoluções, como o Spring. 

Olhando para as sugestões de abstrações presentes nos livros, temos algumas bem comuns. Vou começar pela família dos services;

* Application Service; em geral são representados pelos controllers nas aplicações web;
* Domain Service; em geral são classes dos sistemas que tem o sufixo Service,UseCase etc.
* Infrastructure Service; em geral são representadas por aquelas classes que encapsulam envio de emails, chamadas http, mensagens para filas/tópicos. 

Agora vem a sequência de abstrações que são utilizadas pelos tipos de services listados.

* Entity; aquelas classes que em geral mantêm o estado do sistema e as operações sobre este estado. 
* Value Object; Aqui temos alguns sabores. 
   * Classes que nascem da combinação eventual de estados do sistema;
   * Classes criadas para adicionar semântica à alguma representação que o tipo primitivo da linguagem não suporta, alguns casos classicos: SenhaCriptografada, CPF, Email etc.
* Aggregate root; Esse nome gera bastante confusão. Podemos dizer que um Aggregate root é uma entidade composta por outras entidades ou objetos de valor e que controla o acesso a estas composições. 
* Repository; abstrações que isolam o acesso aos dados. Normalmente são implementadas através de alguma tecnologia de acesso a dados específica. 

## A distorção

O grande desafio enquanto lemos o livro é entender e ser capaz de aplicar os conceitos apresentados de forma a realmente verificar ganhos no design da aplicação. O que encontramos fortemente nos sistemas escritos são interpretações distorcidas das ideias apresentadas por Eric Evans no decorrer do livro. Abaixo deixo algumas. 

### Primeira distorção: controllers só acessam services de domínio

Em primeiro lugar, os controllers que implementamos são chamados de Application Services pelo livro. Você encontra na página 102, no capítulo onde ele descreve os tipos de Service. Para completar, ele cita que Application Services podem acessar diretamente serviços de infraestrutura. Repare, não sou eu que estou dizendo, está escrito. 

### Segunda distorção: services de domínio concentram toda inteligência

Isso inclusive já foi tema de post de Martin Fowler, os tais dos modelos anêmicos. O assunto é velho, mas o problema persiste. Ficamos com controllers de uma linha nas aplicações, entidades e objetos de valor só com atributos e toda operação sobre esses estados é feita dentro de Domain Services.

É escrito claramente no capítulo sobre camadas, na última seção, que é na CAMADA de DOMÍNIO que reside o modelo. Esse modelo é expresso por Entities, Value Objects, Domain Services, Aggregate Roots e Repositories. 

### Terceira distorção: camada superior só acessa a camada inferior subsequente

Ainda no capítulo sobre camadas, na seção chamada "Relacionado as camadas" ele escreve a seguinte frase:

"As camadas superiores podem utilizar ou manipular elementos das camadas inferiroes de forma bastante simples, chamando suas interfaces públicas, fazendo referências a eles(pelo menos temporariamente) e geralmente usando meios convencionais de interação". 

Não está escrito em nenhum santo lugar que Controller não chama Repository. Ele não escreveria isso, porque não faz sentido o mesmo que programa que pode ser escrito com um arquivo e uma linha seja escrito com dois arquivos e duas linhas. Voltarei a essa parte de novo no artigo. 

## Relembrando o real motivo para você criar N arquivos em vez de UM para resolver um problema

## O DDD do proletariado

aqui eu falo do fluxo padrao de uma requisicao e falo que agora de juntar os padrões estabelecidos por um livro super famoso com algo mega prático e que pode ser usado já por você. 



## As sugestões arquiteturais de hoje são sujeitas a interpretação

Antes de começar eu vou deixar aqui uma lista de possíveis influências que você usa ou que talvez já tenha ouvido falar:

* Domain Driven Design
* Onion Architecture
* Hexagonal Architecture
* The Clean Architecture

Todas elas são interessantes, afinal de contas, pelo menos na minha opinião, todo conhecimento pode expandir seu horizonte de mundo. 

E será que falta alguma coisa nelas? Para mim sim. Vou listar alguns itens que eu gostaria de ver:

* Um jeito lógico de definir que tipo de código vai em cada camada. Como eu sei que meu controller está perto de algo ok? Como eu sei que meu service está realmente interessante?
* Especificidade. Contexto importa e eu não quero algo genérico, preciso de algo específico. 

Por exemplo, olha essa frase extraída do DDD:

> When a significant process or transformation in the domain is not a natural responsibility of an > ENTITY or VALUE OBJECT, add an operation to the model as standalone interface declared as a > SERVICE. Define the interface in terms of the language of the model and make sure the operation > name is part of the UBIQUITOUS LANGUAGE. Make the SERVICE stateless.

Em algum momento eu posso passar um formulário de investigação para pegar respostas dos(as) devs sobre esse trecho. Enquanto não faço isso, minha observação diz que eu teria muitas respostas diferentes, e essa é uma situação que eu não posso aceitar. Eu não aguento quando duas pessoas estão discutindo sobre o que é um Domain Service, dado que o suposto conceito está definido em um livro. Ou elas não leram o livro, ou o trecho não foi tão bem escrito.

Existe uma terceira opção para a visão diferente sobre o mesmo conceito, a tal da interpretação. 

## A interpretação arquitetural é danosa

Quanto maior a equipe, mais complicado é você manter uma linha coerente de raciocínio dentro de um mesmo sistema. São muitas cabeças diferentes, com interpretações diferentes sobre o mesmo conceito, escrevendo códigos que refletem a visão delas sobre como uma aplicação web deveria ser programada.  

Tem um cenário talvez ainda pior. Duas pessoas interpretaram o mesmo conceito de forma igual, mas na hora de implementar fazem diferentes. 

## Os componentes e fluxos básicos da maioria das aplicações web

A grande maioria das aplicações web baseadas em linguagens orientadas a objetos, sejam elas pequenas ou grandes, tem alguns componentes muito claros, são eles:

* Controllers que recebem as requisições com seus parâmetros e cabeçalhos;
* Classes que representam os dados enviados na request. De vez em quando são chamados de YYYDTO, YYYViewModel, YYYForms, YYYRequests ou outra nomenclatura que você queira colocar;
* Classes que representam as respostas dos controllers. De vez em quando são chamados de YYYDTO, YYYResponse, YYYOutput ou outra nomenclatura que você queira colocar;
* Classes que representam os tais dos fluxos de negócios. Aqui entram os Domain Services do DDD;
* Classes que representam o tal do model do sistema e que supostamente deveriam guardar e operar sobre o estado da aplicação em si;
* Outras classes que executam funções ortogonais a todo o sistema, como enviar email, mandar mensagem para filas, tópicos, fazer requisições http etc. 

### Fluxo padrão uma vez que uma requisição chegou numa aplicação web

Vou deixar em lista de novo, porque deve ser mais fácil de visualizar. Dado que eu não sei desenhar. 

* Dados da requisição entram por um método do controller;
* Os dados são transformados para algo do seu model;
* Pode ser que algum estado seja alterado no seu model em memória;
* Pode ser que a alteração de estado seja refletida no seu banco de dados;
* Pode ser que essa alteração de estado dispare novos fluxos no sistema;
* Uma resposta é gerada e enviada para quem fez a requisição;

O desafio agora é implementar esse fluxo, em sistemas de variados tamanhos, mantendo a carga intrínseca de cada pedaço do sistema dentro do recomendado que é de no máximo 9 pontos. Sendo que 9 pontos já vai ser complicado.

* Como que eu faço um controller que presta?
* Como eu transformo esses dados para o meu model?
* Como eu reflito o estado do model no banco de dados?
* Como eu disparo esses novos fluxos no sistema?
* Como eu gero minha resposta?

### A resposta

Existem algumas premissas em cima da minha resposta:

* O sistema é construído em cima de uma linguagem orientada a objetos;
* Vou assumir que você utiliza algum framework que abstraia o trabalho duro de uma aplicação web. Spring MVC, Nest.js, Djanjo, Rails, Phoenix, .NET Core ou sei lá mais o que;
* Vou utilizar o design orientado a carga cognitiva para sugerir implementações;
* A aplicação web é um misto entre programação orientada a objetos e procedural. Está tudo bem;

Abaixo segue a lista de itens que considero que contam pontos de carga intrínseca de um código:

* Classes necessárias para habilitar a execução de um determinado ponto do nosso código. Aqui eu só levo em consideração classes que foram criadas pelo próprio sistema. Classes do runtime da linguagem, frameworks etc devem ser conhecidas a priori. Não sabe? Precisa estudar. 
* Número de branches no código. Ifs, loops, ifs e/ou loops aninhados. 
* Número de invocações de métodos em classes específicas do sistema para realizar determinado fluxo.

Cada item acima encontrado em uma classe aumenta um ponto na carga intrínseca. 




#### Como devem ser os controllers?

Os controllers representam o ponto de entrada dos dados. Ele deve ser um lugar onde qualquer possa bater o olho e entender o que está rolando para a execução de um determinado fluxo relativo a uma requisição.

A primeira coisa é que eles devem ter TCC=1(zero também seria válido. uma rota que só usa os parâmetros) seguindo a métrica Tight and Loose Class Cohesion(link), ou seja todos os métodos devem usar todos os atributos(caso existam). O que isso significa? Que numa liguagem orientada a objetos, cada uma das rotas definidas em um controller devem usar todos os atributos. O resultado disso é que você vai acabar com muitos controllers com um método só e está tudo bem. Uma rota é uma procedure, ou uma função abstrata seguindo a ideia do artigo Tipos Abstratos de Dados. Ela poderia muito bem ser escrita sem a necessidade de uma classe :). 

// exemplo de código aqui

A segunda coisa é que você deve trabalhar duro para manter a contagem de pontos da carga intrínseca em no máximo 7. Passou de 7, chegou a hora de dividir. Isso significa que provavelmente você deve criar uma nova classe para realiar aquele fluxo. 

//exemplo aqui

A terceira coisa e que talvez você fique mais assustado. Em um cenário de controllers com TCC=1, provavelmente você não precisa mais daquele service que você criaria. Um controller desse, dado o contexto de utilização de frameworks descrito acima, já tende a representar o caso de uso em si. 

//exemplo aqui 

#### E quando a carga intrínseca do controller estoura?

aqui vou falar da criação dessas outras classes que fazem fluxos... vão ser as ramificações do seu service. Elas também deve ter TCC=1 e não estourar os 7 pontos. 

#### E onde eu transformo os dados

aqui eu deixo a porta aberta para converers(adapters) ou form.toModel(dependencias)... Pouco importa pq você manter a carga cognitiva baixa. Com um converter você vai ter 2 pontos a mais e com um form.toModel 1 ponto a mais. Já que eu vou continuar recebendo o form na rota e vou ter a injeção do converter.

#### como eu reflito

fala rápido de repositórios e pronto. TCC=1 também

#### como eu altero o estado do model na memória

aqui aceita a carga intrínseca até 9 e também deixa claro que o lcc >= 0.8 (uam sugestão inicial)

#### geração de respostas 

nem sei vale entrar no mérito aqui.. seria sempre algo assim new Resposta(dominio) e a resposta navega no dominínio. Essa é uma classe que pode ser mais tosca pq ela vai ser super específica. 

ai eu vou ter outro texto falando de smells que considero que podem ser observados. 









