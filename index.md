# Reactive Services

<a href="http://www.reactivemanifesto.org/pt-BR"> <img style="border: 0; position: fixed; right: 0; top:0; z-index: 9000" src="http://d379ifj7s9wntv.cloudfront.net/reactivemanifesto/images/ribbons/we-are-reactive-green-right.png"> </a>

## Introduction

Reactive Services is a framework that support the development of distributed applications over the [.NET/Mono](http://www.mono-project.com/) frameworks, following the principles of the [Reactive Manifesto](http://www.reactivemanifesto.org) and ready for cloud distribution.

[Reactive Services](http://github.com/reactiveservices) applications present the following characteristics:

- **Modularization**
Applications developed over the Reactive Services framework are modular and ready for cloud distribution by default. That happens due to the heavy focus on asynchronous communication, through message buses, which serves as a guide to the developers to build the applications in the right way.
By default, the communication accross Reactive Services applications and its components happens through these message buses using the patterns  [Publish/Subscribe](http://www.enterpriseintegrationpatterns.com/PublishSubscribeChannel.html), [Request/Response](http://www.enterpriseintegrationpatterns.com/RequestReply.html) and [Send/Receive](http://www.enterpriseintegrationpatterns.com/CommandMessage.html), which allow the implementation of most types of integration behaviors.
- **Scalability**
Each application can be scaled in different ways. It is possible to instanciate multiple copies of the same application, on the same or on different servers, and let the Reactive Services framework handle the distribution of jobs between these instances. It is also possible to split a single application in multiple workers, which can also be replicated and distributed to better accomodate the work load.
- **Resilience**
Reactive Services applications presents different levels of supervision. At first, each application can be composed of one or more computational unit. Each computational unit will be a separate process for the operational system. Inside each computational unit one or more workers can be executed. Each worker is responsible for the execution of a single kind of job.
All jobs executed inside a computational unit are supervised and in case one of them fails, it can be have its execution retried before the failure be considered permanent. It is also possible that a job that failed in one computational unit be retried by another computational unit, that can even be running in a different server.
Besides that, the computational units themselves are also supervised. In case its process crashes or stops responding, the computational unit is relaunched by the Reactive Services bootstrap process.
And this can be taken even further by placing the whole application, a bootstrap process and its computational units, inside a container platform that supervise and restart its containers in the case of a failure on the whole thing.
- **Security**
Every communication accross Reactive services applications and its components is meant to be made through a messaging middleware that follows the [AMQP](https://www.amqp.org/) protocol. By default, this middleware is the [RabbitMQ](https://www.rabbitmq.com/), which provides secure channels through [TLS/SSL](https://pt.wikipedia.org/wiki/Transport_Layer_Security) and authentication and authorization rules to protect its communications.
- ** Extensibility **
Most components of the Reactive Services framework are designed to allow them to be replaced by alternative implementations. That allow the framework to be extensible and fit the needs of its users.
It is possible, for example, to replace the ReactiveServices.MessageBus.RabbitMQ.dll  module by a ReactiveServices.MessageBus.ActiveMQ.dll module and then use ActiveMQ as its message bus.
- **Containerization**
Due to its distributed nature, Reactive Services applications can be installed in individual [containers](https://www.docker.com/), which leverage the use of all distribution and management resources of such container platform.
- **Adherence to the Reactive Manifesto**
The [Reactive Manifesto](http://www.reactivemanifesto.org) describes a criteria to define an application as *reactive*. Reactive applications are ready for cloud distribution, presenting responsiveness, resilience, elasticity and message driven characteristics, necessary for this kind of application.

## 

To know more about the framework visit its repository on github at http://github.com/reactiveservices.

## Sponsorship

Sponsored by [Concert Technologies SA](http://www.concert.com.br/en/)

## License

MIT License
