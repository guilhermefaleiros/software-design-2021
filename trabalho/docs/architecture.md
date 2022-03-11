# DEFINIÇÃO DE ARQUITETURA E DESIGN DO PROJETO MEDICAMENTOS

## Contexto e Descrição Geral do Projeto

Cada município oferece em sua rede, medicamentos específicos em determinadas quantidades e em pontos geográficos diferentes. O objetivo deste projeto é fornecer uma plataforma que forneça de forma unificada um catálogo dos medicamentos oferecidos por determinado município.

## Pontos importantes para definição da Arquitetura

De forma geral, foi dado como requisito que o software deverá ser escalável, configurável por município e possuir alta disponibilidade. Isso implica em alguns pontos importantes que podem ser  importantes para definição e planejamento da arquitetura:

* **Escalável**: isso implica que o software deve estar aberto à extensão de funcionalidades do software no futuro, bem como a possibilidade de aumentar a implantação e viabilização do software para novos municípios.

* **Disponibilidade de dados**: os dados referentes aos estoque de medicamentos devem ser possíveis de serem consumidos tanto pelo usuário final quanto por sistemas externos.

* **Alta disponibilidade**: por se tratar de um sistema da saúde, é desejável que o software tenha alta tolerância à falhas e uma infraestrutura segura que permita que o sistema minimize ao máximo o número interrupções no tráfego e indisponibilidade.
  
* **Integração com sistemas externos**: o software deve ser capaz de se integrar com sistemas e bases de dados externas a modo de obter informações sobre disponibilidade de remédios de cada município.

## Visões da Arquitetura e Design que serão desenvolvidas

Por meio deste documento, serão definidas algumas visualizações importantes para definição e documentação da arquitetura do software. Sendo eles:

* **Diagrama de Componentes**: este diagrama tem como objetivo explicitar os elementos do software, bem como suas interfaces e fronteiras entre si e com possíveis sistemas ou elementos externos.

* **Diagrama de Infraestrutura/Implantação**: este diagrama visa explicitar especificar a infraestrutura do software a nível de componentes de hardware (redes, máquinas virtuais).

* **Modelagem de banco de dados**: este diagrama tem como objetivo descrever as coleções e atributos que o banco de dados do software deve possuir.

## Elementos do Sistema de Software

Para atender os requisitos levantados, foram pensados alguns elementos para satisfazer as necessidades e complexidades inerentes ao software, ambas são detalhadas com maior profundidade. Vejamos:

* **Back-end Core**: este componente será responsável por fornecer uma API para o consumo da aplicação web que irá atender o cliente final, bem como manter a sincronização de dados na base de dados de leitura.
* **Back-end Admin**: este componente será responsável por fornecer uma API para manipulação de dados referentes a medicamentos e estoque.
* **Front-end Web**: este componente é um sistema front-end web destinado ao usuário final, a fim de disponibilizar o catálogo de medicamentos.
* **Front-end Admin**: este componente se trata de uma interface web para 

## Ferramentas para Desenvolvimento do Software

Descrição de ferramentas, linguagens de programação, frameworks e serviços para implementação dos elementos de software.

Linguagens de programação:
Javascript/Typescript: linguagem moderna utilizada tanto para construção de aplicações server-side e client-side, com uma comunidade ativa e muitas opções de ferramentas disponíveis.

### Frameworks e ferramentas para construção:
* **Next.js**: construção de páginas web baseadas em server-side rendering, utilizando como base a biblioteca ReactJS.
* **Nodejs**: construção de aplicações back-end.
* **Docker e Kubernetes**: conteinerização, scaling e gerenciamento das instâncias da aplicação, bem como o provisionamento de ferramentas e ambiente eficaz para ambiente de desenvolvimento.
* **DynamoDB**: banco utilizado para armazenamento de informações utilizadas para leitura.
* **PostgreSQL**: banco utilizado para armazenamento de informações consolidadas.
* **Redis**: banco utilizado para armazenamento de cache de dados.

### Ferramentas de versionamento:
* **Git e Github**: para versionamento e armazenamento de código fonte.

### Ferramentas para integração e entrega contínua:
* **Drone CI**: ferramenta utilizada para construção de pipelines de integração contínua.
* **Argo CD**: ferramenta para realização de pipelines de entrega contínua em infraestruturas baseadas em Kubernetes.
* **ESLint**: ferramenta para linting de código Javascript/Typescript

### Serviços:

* **AWS VPC**: criação de redes internas.
* **AWS EKS**: administração de cluster Kubernetes.
* **AWS EC2**: disponibilização de máquinas em cloud.
* **ElastiCache**: provisionamento de instâncias de Redis gerenciado na cloud.
* **DynamoDB**: banco NoSQL gerenciado na cloud AWS.
* **AWS S3 e CloudFront**: serviços de distribuição de conteúdo estático web.
* **AWS RDS**: serviço gerenciado para provisionamento de bancos SQL padrões de mercado como MySQL e PostgreSQL.
* **AWS SQS**: serviço para troca de mensagens entre sistemas distribuídos no formato de filas de processamento.
* **AWS API Gateway**: serviço para gerenciamento de APIs.
* **AWS Route 53**: serviço para fornecimento de DNS.

## Decisões Arquiteturais

**Padrões Arquiteturais**: foram escolhidos alguns padrões a serem utilizados de modo a resolver os requisitos arquiteturais levantados:
  * **CQRS**: para deixar os dados em alta disponibilidade e tornar o software escalável e performático foi adotada a utilização do padrão CQRS, de modo que, dados que sejam necessários serem consultados com frequência e em formatos específicos, serão persistidas em coleções desnormalizadas separadas, indexadas justamente para otimização de buscas, aliando isso com estratégias de cache bem desenhadas. De modo que a informação original será persistida em uma base de dados transacional e normalizada, que seria a "fonte original da verdade".
  * **Microsserviços**: serão criados microsserviços específicos para cada funcionalidade, de modo a separar as responsabilidades corretamente e permitir uma escalabilidade horizontal de determinados serviços de acordo com a demanda.
  *  **Pub/Sub**: para sincronização entre as bases de leitura e escrita, será realizada a publicação de eventos assíncronos através de um barramento de mensagens, eventos estes que farão a replicação e atualização de dados entre as bases de dados.


**Amazon Web Services (AWS)**: para adequação de requisitos como escalabilidade e segurança, foi escolhida a utilização de serviços gerenciados na cloud AWS, que se caracteriza por alta resiliência e performance em seus serviços:
  * **AWS VPC**: será utilizada para manter que todas as transações e fluxos de comunicação sejam realizados em uma rede interna fechada para a internet, auxiliando na segurança de informações pertinentes ao software.
  * **AWS EKS**: uma vez decidida a utilização de Kubernetes, o serviço gerenciado permitirá que o cluster seja escalado horizontalmente de maneira fácil, com pouco esforço e bastante segurança.
  * **AWS EC2**: uma vez utilizado o EKS, o EC2 servirá para criação de cada nó do cluster Kubernetes.