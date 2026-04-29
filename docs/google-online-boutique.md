# Google Online Boutique / microservices-demo

## 1. Identificação Geral

**Nome da aplicação:** Online Boutique  
**Repositório principal:** GoogleCloudPlatform/microservices-demo  
**Organização responsável:** GoogleCloudPlatform  
**Domínio da aplicação:** E-commerce / loja virtual  
**Tipo de projeto:** Aplicação demonstrativa de microsserviços cloud-native  
**Finalidade aparente:** Demonstração tecnológica, estudo de microsserviços, Kubernetes, gRPC, service mesh, observabilidade e modernização de aplicações em nuvem  
**Status geral do projeto:** Ativo e amplamente utilizado como referência educacional e experimental

O projeto Online Boutique, também conhecido como `microservices-demo`, é uma aplicação de e-commerce baseada em microsserviços. A aplicação simula uma loja online na qual usuários podem visualizar produtos, adicionar itens ao carrinho e realizar uma compra simulada.

O projeto é mantido pela organização GoogleCloudPlatform e tem como objetivo demonstrar práticas e tecnologias associadas a aplicações cloud-native, incluindo Kubernetes, Google Kubernetes Engine, Cloud Service Mesh, gRPC e ferramentas de observabilidade.

> https://github.com/GoogleCloudPlatform/microservices-demo

![architecture](architecture-diagram.png)

## 2. Evidências Observáveis no Repositório

A análise foi baseada principalmente nos seguintes artefatos públicos do projeto:

- README principal do repositório;
- diretório `src`, contendo o código-fonte dos serviços;
- diretório `protos`, contendo definições Protocol Buffers;
- diretório `kubernetes-manifests`, contendo manifestos Kubernetes;
- diretório `helm-chart`, contendo chart Helm;
- diretório `istio-manifests`, contendo artefatos relacionados a Istio;
- arquivo `skaffold.yaml`, usado para build e deploy;
- documentação de desenvolvimento no diretório `docs`;
- arquivo `cloudbuild.yaml`, indicando automação de build;
- licença Apache-2.0.

Esses artefatos fornecem evidências suficientes para analisar a arquitetura, a separação de responsabilidades, a comunicação entre serviços, a implantação e a adequação da aplicação para atividades práticas de laboratório.

## 3. Estrutura Arquitetural

O Online Boutique é composto por múltiplos serviços independentes, organizados em torno de uma aplicação de e-commerce. A arquitetura separa responsabilidades como frontend, catálogo de produtos, carrinho de compras, pagamento, envio, recomendação, anúncios, checkout e geração de carga.

Os principais serviços identificados são:

| Serviço | Linguagem | Responsabilidade principal |
|---|---|---|
| `frontend` | Go | Expõe a interface web da loja e recebe as requisições dos usuários |
| `cartservice` | C# | Gerencia os itens do carrinho de compras usando Redis |
| `productcatalogservice` | Go | Fornece a lista de produtos e busca de itens |
| `currencyservice` | Node.js | Realiza conversão de valores entre moedas |
| `paymentservice` | Node.js | Simula cobrança de cartão de crédito |
| `shippingservice` | Go | Calcula custos de envio e simula entrega |
| `emailservice` | Python | Simula envio de e-mail de confirmação |
| `checkoutservice` | Go | Orquestra carrinho, pagamento, envio e e-mail |
| `recommendationservice` | Python | Recomenda produtos com base no carrinho |
| `adservice` | Java | Fornece anúncios contextuais |
| `loadgenerator` | Python/Locust | Gera carga simulando fluxos de usuários |

A comunicação entre os serviços ocorre predominantemente por gRPC. As definições de contrato entre serviços ficam no diretório `protos`, o que facilita a análise das interfaces de comunicação.

A aplicação possui uma estrutura fortemente orientada a serviços, com responsabilidades bem delimitadas. O serviço `frontend` atua como ponto de entrada para o usuário, enquanto serviços internos executam funções específicas do domínio de e-commerce.

## 4. Comunicação entre Serviços

A comunicação predominante é síncrona, baseada em gRPC. O uso de Protocol Buffers contribui para a definição explícita dos contratos entre serviços.

A arquitetura possui características relevantes para estudo de sistemas distribuídos, como:

- múltiplas chamadas entre serviços;
- dependências em cadeia durante o fluxo de checkout;
- separação entre interface, regras de negócio e serviços auxiliares;
- uso de serviço externo de cache para o carrinho;
- possibilidade de observar latência e falhas entre chamadas;
- boa aderência a experimentos com service mesh.

O serviço `checkoutservice`, por exemplo, depende de outros serviços para concluir uma compra simulada, como `cartservice`, `paymentservice`, `shippingservice` e `emailservice`. Isso torna a aplicação útil para análise de dependências, propagação de falhas e observabilidade distribuída.

## 5. Implementação

O projeto apresenta heterogeneidade tecnológica, pois seus microsserviços são implementados em diferentes linguagens de programação, incluindo Go, C#, Node.js, Python e Java.

Essa diversidade é relevante para o laboratório porque permite observar como uma aplicação de microsserviços pode combinar múltiplas stacks tecnológicas dentro de uma mesma arquitetura.

A organização do código favorece a análise individual de cada serviço. Cada microsserviço possui sua própria implementação e responsabilidade, permitindo avaliar aspectos como:

- separação de código por serviço;
- independência tecnológica;
- uso de contratos compartilhados via Protocol Buffers;
- presença de artefatos de build;
- padronização de empacotamento em contêineres;
- automação de build e deploy com Skaffold.

O projeto também inclui um serviço de geração de carga baseado em Locust, o que é especialmente útil para atividades de teste de desempenho e observabilidade.

## 6. Dados e Persistência

A persistência principal observável no projeto está associada ao serviço de carrinho, que utiliza Redis para armazenar os itens adicionados pelos usuários.

Além disso, o serviço `productcatalogservice` obtém os dados de produtos a partir de um arquivo JSON. Já serviços como pagamento, envio e e-mail são implementados como simulações, sem integração real com sistemas externos.

A aplicação não representa um sistema corporativo completo do ponto de vista de persistência, pois não possui múltiplos bancos de dados transacionais complexos nem um modelo avançado de consistência distribuída. Ainda assim, ela é suficiente para estudar padrões básicos de dependência entre serviços, uso de cache e separação de responsabilidades.

| Aspecto | Avaliação |
|---|---|
| Banco relacional | Não é o foco principal da aplicação |
| Banco NoSQL/cache | Redis usado pelo carrinho |
| Dados estáticos | Catálogo de produtos em JSON |
| Banco por serviço | Parcialmente observável |
| Persistência complexa | Baixa |
| Adequação para estudo de dados | Média |

## 7. Implantação e Operação

O Online Boutique possui forte aderência a ambientes conteinerizados e orquestrados. O repositório contém manifestos Kubernetes, Helm chart, artefatos para Istio, Kustomize, Terraform e configuração Skaffold.

Os principais artefatos de implantação identificados são:

| Artefato | Evidência de uso |
|---|---|
| Docker | Serviços empacotados em contêineres |
| Kubernetes | Diretório `kubernetes-manifests` e manifesto de release |
| Helm | Diretório `helm-chart` |
| Istio | Diretório `istio-manifests` |
| Kustomize | Diretório `kustomize` |
| Skaffold | Arquivo `skaffold.yaml` |
| Terraform | Diretório `terraform` |
| Cloud Build | Arquivo `cloudbuild.yaml` |

A documentação do projeto indica que a aplicação pode ser executada em clusters Kubernetes, incluindo GKE e ambientes locais como Minikube ou Kind. Isso torna o projeto adequado tanto para laboratórios em nuvem quanto para experimentos locais.

A presença de múltiplas opções de implantação também permite discutir complexidade operacional, portabilidade e diferentes formas de empacotamento de aplicações cloud-native.

## 8. Observabilidade, Monitoramento e Testes

O projeto é bastante adequado para estudos de observabilidade. Por ter múltiplos serviços que se comunicam via gRPC, a aplicação permite observar métricas, logs e traces distribuídos.

Além disso, o projeto é frequentemente usado em demonstrações com Istio, Cloud Service Mesh e ferramentas de observabilidade. A existência de um serviço `loadgenerator` facilita a criação de tráfego para experimentos.

Aspectos relevantes para laboratório:

- geração de carga com Locust;
- possibilidade de monitorar latência entre serviços;
- possibilidade de analisar erros em chamadas gRPC;
- compatibilidade com service mesh;
- boa estrutura para estudar tracing distribuído;
- boa estrutura para testes de resiliência e injeção de falhas.

## 9. Adequação para Uso em Atividades de Laboratório

O Online Boutique é altamente adequado para atividades práticas envolvendo microsserviços, Kubernetes, observabilidade, desempenho e resiliência.

| Dimensão | Avaliação |
|---|---|
| Implantação em Kubernetes | Alta |
| Observabilidade e monitoramento | Alta |
| Testes de desempenho | Alta |
| Testes de resiliência | Alta |
| Análise de comunicação entre serviços | Alta |
| Análise de persistência | Média |
| Complexidade operacional | Média |
| Valor didático | Alto |

A aplicação é suficientemente complexa para representar problemas reais de sistemas distribuídos, mas ainda controlada o bastante para uso em ambiente didático. Isso a torna uma boa candidata para as próximas etapas práticas da disciplina.

## 10. Vantagens

As principais vantagens do Online Boutique como objeto de estudo são:

- arquitetura explicitamente baseada em microsserviços;
- domínio de aplicação fácil de entender;
- múltiplas linguagens de programação;
- comunicação entre serviços via gRPC;
- suporte direto a Kubernetes;
- presença de Helm chart, Kustomize, Istio e Skaffold;
- serviço de geração de carga incluído;
- boa adequação para observabilidade e experimentos com service mesh;
- documentação técnica razoável;
- projeto conhecido e amplamente utilizado em demonstrações cloud-native.

## 11. Limitações

Apesar de ser uma escolha forte para laboratório, o projeto possui algumas limitações:

- é uma aplicação demonstrativa, não um sistema real de produção;
- alguns serviços simulam comportamentos externos, como pagamento, envio e e-mail;
- a persistência é relativamente simples;
- o domínio de negócio é menos complexo do que em aplicações corporativas reais;
- a implantação completa pode exigir recursos computacionais razoáveis;
- a quantidade de serviços pode aumentar a dificuldade inicial para alunos com pouca experiência em Kubernetes.

Essas limitações não invalidam o uso do projeto. Pelo contrário, ajudam a posicioná-lo como uma aplicação adequada para ensino, experimentação e caracterização técnica, mas não como representação completa de um sistema corporativo real.

## 12. Conclusão

O Google Online Boutique é uma das aplicações mais adequadas para o escopo do laboratório. O projeto apresenta evidências claras de arquitetura de microsserviços, possui repositório público, documentação técnica, múltiplos serviços independentes, comunicação via gRPC, suporte a Kubernetes e artefatos úteis para implantação, observabilidade e testes de desempenho.

Sua principal contribuição para o trabalho é permitir uma análise concreta de uma aplicação cloud-native composta por serviços heterogêneos e implantável em Kubernetes. Além disso, sua estrutura favorece experimentos futuros com monitoramento, tracing distribuído, service mesh, geração de carga e testes de resiliência.

Portanto, o Online Boutique é uma escolha altamente recomendada para compor o conjunto de aplicações analisadas no trabalho.

## Referências

- GoogleCloudPlatform. `microservices-demo`: Sample cloud-first application with microservices showcasing Kubernetes, Istio and gRPC.
- GoogleCloudPlatform. Documentação de desenvolvimento do Online Boutique.
- Google Cloud. Documentação de implantação do Online Boutique com Cloud Service Mesh.