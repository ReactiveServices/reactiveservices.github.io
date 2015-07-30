# Reactive Services

<a href="http://www.reactivemanifesto.org/pt-BR"> <img style="border: 0; position: fixed; right: 0; top:0; z-index: 9000" src="http://d379ifj7s9wntv.cloudfront.net/reactivemanifesto/images/ribbons/we-are-reactive-green-right.png"> </a>

## Introdução

Reactive Services é um framework para desenvolvimento de aplicações distribuídas sobre plataforma [.NET/Mono](http://www.mono-project.com/), aderentes ao [Manifesto Reativo](http://www.reactivemanifesto.org/pt-BR) e prontas para a distribuição em ambientes de [computação na nuvem](http://https://pt.wikipedia.org/wiki/Computa%C3%A7%C3%A3o_em_nuvem).

Aplicações escritas sobre o framework [Reactive Services](http://reactiveservices.github.io/) apresentam as seguintes características:


- **Modularização**
Aplicações desenvolvidas sobre o framework Reactive Services já nascem modularizadas e prontas para serem distribuídas independentemente. Isso se deve ao enfoque na comunicação assincrona e através de barramento de mensagens, tanto entre as aplicações quanto entre os componentes das aplicações, o que serve como guia para o trabalho do desenvolvedor.
Por padrão, a comunicação entre aplicações Reactive Services ocorre através da postagem de mensagens em um ou mais barramentos de mensagem, seguindo os padrões [Publish/Subscribe](http://www.enterpriseintegrationpatterns.com/PublishSubscribeChannel.html), [Request/Response](http://www.enterpriseintegrationpatterns.com/RequestReply.html) ou [Send/Receive,](http://www.enterpriseintegrationpatterns.com/CommandMessage.html) os quais permitem a implementação de praticamente todos os tipos de comunicação necessária entre duas aplicações. O uso desses padrões permite que as aplicações se mantenham mais independentes e desconectadas umas das outras, podendo assim serem modificadas e substituídas mais facilmente quando necessário.
- **Escalabilidade**
Cada aplicação pode ser escalada de diferentes formas. É possível instanciar múltiplas cópias da mesma aplicação, seja no mesmo servidor ou em servidores distintos, e deixar que o Reactive Services cuide da distribuição dos *jobs* a serem executados entre as diferentes instâncias, bastanto para isso que estejam todas conectadas ao mesmo barramento de mensagens. É possível também separar uma única aplicação em diferentes *workers*, os quais podem também ser replicados para acomodar melhor as demandas de trabalho.
- **Resiliência**
Aplicações escritas sobre o Reactive Services apresentam diferentes níveis de supervisão. Antes de tudo, cada aplicação pode ser composta por uma ou mais unidades computacionais. Cada unidade computacional, na prática, será um processo independente do sistema operacional. Dentro de cada unidade computacional podem ser executados um ou mais *workers*. Cada *worker* é responsável por executar um único tipo de trabalho, ou *job*.
Todos os trabalhos executados dentro de uma unidade computacional são supervisonados e caso um desses trabalhos falhe, ele poderá ser repetido antes de ter sua falha considerada definitiva. É possível também que um trabalho que tenha falhado em uma unidade computacional seja retomado por outra unidade computacional, que pode até mesmo estar localizada em outro servidor.
Além disso, as unidades computacionais em si também são supervisionadas. Caso o processo, no qual uma unidade computacional é executada, caia ou deixe de responder, a unidade computacional é relançada automaticamente pelo Reactive Services.
- **Segurança**
Toda comunicação entre as aplicações Reactive Services e seus componentes, unidades computacionais e *workers*, é realizada através de barramentos de mensagens que utilizem o padrão [AMQP](https://www.amqp.org/), os quais são bastante seguros, permitindo que as aplicações se conectem apenas aos recursos aos quais possuem acesso. Além disso, a comunicação com esses barramentos é feita através de conexões [TLS/SSL](https://pt.wikipedia.org/wiki/Transport_Layer_Security), o que além de encriptar os dados, garante às aplicações a autenticidade do barramento utilizado.
- ** Extensibilidade **
Por padrão, o barramento de mensagens usado pelo Reactive Services é o [RabbitMQ](https://www.rabbitmq.com/), mas todo o Reactive Services é escrito de modo extensível, permitindo que diferentes serviços de barramento de mensagens possam ser utilizados.
Para isso, basta que uma nova biblioteca seja criada extendendo os componentes de software necessários. Uma vez criada a nova implementação, basta informar que esta deve ser usada no lugar da implementação padrão, através de um arquivo de configuração, técnica conhecida como [Injeção de Dependências](http://robsoncastilho.com.br/2014/06/07/padroes-de-di-introducao/). É possível, por exemplo, criar uma biblioteca *ReactiveServices.MessageBus.ActiveMQ* para usar o [ActiveMQ](http://activemq.apache.org/) como barramento de mensagens, sem que o código da aplicação em si seja alterado.
- **Instalação em containers**
Devido a sua natureza distribuída, as aplicações Reactive Services podem ser instaladas em [container](https://www.docker.com/) individuais, o que permite que todas os recursos de distribuição e gerenciamento da plataforma de containers possam ser utilizados.
- **Aderência aos Manifesto Reativo**
O [Manifesto Reativo](http://www.reactivemanifesto.org/pt-BR) é um manifesto que descreve critérios para que uma aplicação distribuída seja considerada *reativa*, termo usado para se referir ao comportamento esperado para uma aplicação distibuída, sendo para isso responsíva, resiliente, elástica e orientada por mensagens.

## Principais componentes

Os principais componentes de uma aplicação Reactive Services são:
- **[Application].exe:**	Aplicativo responsável por iniciar uma aplicação Reactive Services. Esse aplicativo deve seguir o modelo do pacote ReactiveServices disponível no nuget.org
Ao ser executado, esse aplicativo lança as unidades computacionais e os *jobs* iniciais configurados no arquivo *bootstrap.config*.
O processo iniciado com este comando se mantem ativo permanentemente e é ele também o responsável por supervisonar as unidades computacionais lançadas.
- **[Application].dll:**	Biblioteca de sofware contendo uma dada aplicação, conforme informado no arquivo *bootstrap.config*.
- **ComputationalUnit.exe:**	Comando responsável por iniciar e hospedar os *workers* que compõem uma unidade computacional.
Ao se executado esse comando inicializa uma nova unidade computacional e a configura conforme o arquivo de configuração recebido como parâmetro.
- **Arquivos de configuração:** os arquivos *Bootstrap.config*, *Settings.config*, *Dependencies.config* e *NLog.config* estarão presentes em toda aplicação Reactive Services, fornecendo às aplicações os parâmetros necessários para sua execução.
- **ReactiveServices.\*.dll e outros arquivos \*.dll:**	Bibliotecas dos Reactive Services, usadas pelos aplicativos listados acima.
- **mono:**	Comando usado para execução dos aplicativos *bootstrap.exe* e *computationalunit.exe* em ambiente Linux. Em ambiente Windows esse comando também pode ser utilizado, no entanto a execução direta dos aplicativos também é possível, sendo até recomendada.

## Estrutura das Aplicações
Cada aplicação Reactive Services deve ser distribuída em uma pasta distinta, contendo os arquivos de configuração e as bibliotecas que compõem a aplicação. Em uma implantação em containers, essa pasta seria o que iria para o container da aplicação. As bibliotecas e executáveis do Reactive Services podem estar juntas com a aplicação ou em algum outro lugar, desde que no PATH do sistema.
Para cada aplicação deve ser criada uma biblioteca (arquivo .dll). Um bom parametro para definir o que vai em uma aplicação e o que vai em outra é a definição usada no estilo arquitetural conhecido como [Arquitetura de Microserviços](http://martinfowler.com/articles/microservices.html). Cada aplicação Reactive Services pode ser vista como um microserviço, neste caso composto por uma ou mais unidades computacionais e seus *workers* (atenção para não confundir esses 3 conceitos). Outro parametro que pode ser usado para definir o que compõe uma aplicação, ou microserviço, é o conceito de *[Bounded Context](http://martinfowler.com/bliki/BoundedContext.html)*, da metodologia [Domain-Driven Design](http://dddcommunity.org/). Cada aplicação Reactive Services pode ser vista como um *Bounded Context*.

## Estrutura dos Projetos e Ambientes de Execução
Cada aplicação Reactive Services é desenvolvida como uma biblioteca .NET, podendo ser executada nos ambientes [.NET Framework 4.5.x](https://msdn.microsoft.com/pt-br/vstudio/aa496123) ou [Mono 4.0.x](http://www.mono-project.com/) e futuramente no [.NET Core 5](https://github.com/dotnet/home), ambiente multiplataforma desenvolvido e suportado pela Microsoft.
A IDE utilizada para desenvolvimento de aplicações Reactive Services é o [Visual Studio 2013/2015](https://www.visualstudio.com/) em ambiente Windows, tendo como *target* o .NET Framework 4.5.x, ainda que as aplicações geradas possam ser executadas em ambientes Linux e Mac OS X, através do Mono.
A estrutura de pastas e *namespaces* deve coincidir, sendo definida com base nos padrões da metodologia [Domain-Driven Design](http://dddcommunity.org/).
Para cada aplicação e para cada *worker* desenvolvidos deve haver testes automatizados formando uma [especificação executável](http://www.specflow.org/getting-started/), segundo os padrões da metodologia [Behavior-Driven Design](http://behaviourdriven.org/).
Os testes devem ser implementados em bibliotecas distindas, normalmente em uma pasta denominada *Tests*, dentro da pasta do projeto da aplicação ou *worker* a ser testado.
O framework Reactive Services se destina ao desenvolvimento de aplicações de *backend*, em especial para processamento de dados, sendo que em muitos casos elas podem nem mesmo ter uma interface gráfica. Para os casos em que houver, essa interface gráfica normalmente será provida através de uma aplicação externa a ser desenvolvida independentemente e integrada às aplicações Reactive Services através dos protocolos [AMQP](https://www.amqp.org/), [HTTP](https://pt.wikipedia.org/wiki/Hypertext_Transfer_Protocol) e [WebSocket](https://www.websocket.org/).

## Estágio Atual do Desenvolvimento
No momento da escrita deste documento, Julho de 2015, o framework Reactive Services encontra-se em fase final de seu período como *beta*, já tendo sido utilizado em alguns projetos de software da empresa em que foi originalmente criado. Sua evolução será contínua, contando inclusive com a participação de outros desenvolvedores não ligados ao seu desenvolvimento original.

## Licença e Propriedade Intelectual
O framework Reactive Services foi idealizado por [Rafael Romão](http://rafaelromao.com) enquanto trabalhava como arquiteto de software para a [Concert Technologies SA](http://www.concert.com.br), tendo sido desenvolvido em conjunto com a equipe de desenvolvedores da empresa.
Em meados de 2015, decidiu-se pela disponibilização do framework como um projeto *[open source](https://www.youtube.com/watch?v=Tyd0FO0tko8)* na plataforma [GitHub](https://github.com/reactiveservices), sendo ainda mantido por seus desenvolvedores originais, ainda que aberto à contribuição da comunidade de desenvolvedores, tanto através de relatos de experiências de uso quanto através do trabalho direto na evolução de seu código fonte, sempre sob o controle de seus mantenedores.
A licença utilizada para controlar o uso e a contribuição ao projeto é a [Licença MIT](http://opensource.org/licenses/MIT), a qual permite o uso gratuíto do framework, desde que mantidos os termos da licença e a mensão aos seus autores originais.

### Propriedade Intelectual das Aplicações

Vale ressaltar que aplicações criadas utilizado o framework Reactive Services não se tornam parte integrante dele, não tendo seu código fonte que ser disponibilizado e podendo ser comercializados como queiram seus desenvolvedores. Além disso, a propriedade intelectual de tais softwares se mantém sob controle dos seus desenvolvedores.
Aos olhos de quem utiliza o Reactive Services, o framework é apenas mais um software utilizado por seus sistemas, como acontece com um sistema de banco de dados, por exemplo.


_ _ _
###### Rafael Romão
Arquiteto de Software
Concert Technologies SA
30 de Julho de 2015, 13:44