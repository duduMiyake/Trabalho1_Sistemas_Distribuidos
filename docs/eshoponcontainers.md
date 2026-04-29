# eShopOnContainers

[Voltar ao README](../README.md)

## Identificação geral

**Nome da aplicação:** eShopOnContainers  
**Domínio:** aplicação de comércio eletrônico (e-commerce)  
**Repositório:** [github.com/dotnet/eShop](https://github.com/dotnet/eShop)  
**Organização responsável:** Microsoft  
**Finalidade:** demonstração arquitetural de microsserviços corporativos em .NET  
**Status:** ativo como projeto de referência para arquitetura de microsserviços  
**Classificação:** aplicação educacional com foco em cenários corporativos reais

O eShopOnContainers é uma aplicação open source desenvolvida pela Microsoft com o objetivo de demonstrar a construção de um sistema de e-commerce baseado em arquitetura de microsserviços. A aplicação simula uma loja virtual completa, incluindo funcionalidades como catálogo de produtos, carrinho de compras, pedidos, autenticação, pagamento e promoções.

Diferentemente de exemplos mais simplificados, o eShop busca representar um cenário mais próximo de sistemas corporativos reais, incorporando padrões modernos de arquitetura distribuída.

## Estrutura arquitetural

<img src="../assets/eShop.png" alt="Diagrama arquitetural do eShopOnContainers" width="100%">

O eShopOnContainers é composto por diversos microsserviços independentes, cada um responsável por uma funcionalidade específica do sistema.

Principais serviços:

- `Catalog.API`: responsável pelo gerenciamento do catálogo de produtos;
- `Basket.API`: gerencia o carrinho de compras;
- `Ordering.API`: responsável pelo processamento e gerenciamento de pedidos;
- `Identity.API`: responsável por autenticação e autorização;
- `Webhooks.API`: gerencia notificações externas via webhooks.

A arquitetura apresenta:

- separação clara entre frontend e backend;
- uso de API Gateway para centralizar o acesso;
- utilização de BFF (Backend for Frontend) para adaptação de dados a diferentes clientes;
- baixo acoplamento entre serviços.

A comunicação entre os serviços é híbrida:

- síncrona via HTTP/REST e gRPC;
- assíncrona via mensageria (RabbitMQ ou Azure Service Bus).

Essa abordagem permite maior escalabilidade e resiliência, além de facilitar a evolução independente dos serviços.

## Implementação

A aplicação é desenvolvida majoritariamente utilizando tecnologias do ecossistema .NET.

Tecnologias observadas:

- C#;
- ASP.NET Core;
- .NET 9;
- Blazor Web App (frontend).

Características observadas:

- padronização tecnológica entre os serviços;
- organização modular por microsserviço;
- uso de padrões arquiteturais modernos;
- forte integração com ferramentas do ecossistema Microsoft.

## Dados e persistência

O eShopOnContainers adota o padrão **database per service**, no qual cada microsserviço possui seu próprio banco de dados.

Tecnologias utilizadas:

- SQL Server para serviços como catálogo, pedidos e autenticação;
- Redis para cache e gerenciamento do carrinho;
- RabbitMQ ou Azure Service Bus para comunicação assíncrona.

Características:

- isolamento de dados por serviço;
- maior independência entre microsserviços;
- suporte a escalabilidade e resiliência;
- uso combinado de armazenamento relacional e em memória.

## Implantação e operação

A aplicação possui suporte completo a ambientes conteinerizados e orquestrados.

Recursos disponíveis:

- Dockerfiles individuais para cada serviço;
- suporte a docker-compose;
- implantação em Kubernetes;
- integração com Azure Kubernetes Service (AKS).

Recursos de observabilidade:

- OpenTelemetry para coleta de dados;
- logs centralizados;
- tracing distribuído;
- métricas de monitoramento.

Recursos operacionais:

- suporte a escalabilidade horizontal;
- integração com pipelines de CI/CD;
- monitoramento de serviços distribuídos.

A complexidade de implantação é considerada alta, devido à quantidade de serviços e à infraestrutura necessária.

## Adequação para uso em atividades de laboratório

**Adequação para Kubernetes:** excelente  
**Adequação para observabilidade:** excelente  
**Adequação para testes de desempenho:** excelente  
**Adequação para testes de resiliência:** excelente  
**Complexidade operacional:** alta  

Vantagens:

- arquitetura próxima de sistemas reais;
- uso de múltiplos padrões de microsserviços;
- forte integração com ferramentas modernas;
- ideal para estudos avançados de sistemas distribuídos;
- suporte completo a observabilidade e mensageria.

Limitações:

- alta complexidade de configuração;
- forte dependência do ecossistema .NET e Azure;
- curva de aprendizado elevada;
- maior dificuldade para execução em ambientes limitados.

## Conclusão

O eShopOnContainers apresenta uma arquitetura robusta e completa de microsserviços, sendo altamente adequado para estudos avançados em sistemas distribuídos. Sua principal contribuição está na representação de um cenário próximo ao ambiente corporativo real, permitindo explorar aspectos como comunicação híbrida, mensageria, observabilidade e escalabilidade.

Apesar de sua complexidade, o projeto se destaca como uma referência sólida para compreensão de arquiteturas modernas baseadas em microsserviços.

## Referências

- [Repositório oficial do eShop](https://github.com/dotnet/eShop)
