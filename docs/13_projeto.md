# API de Nutrição (api-mbs)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Este projeto marcou a primeira API REST profissional do portfólio, e também o primeiro trabalho em que o Spring Boot foi usado como framework principal. O sistema foi encomendado por um cliente da área de nutrição, que precisava de um back-end para um aplicativo de controle alimentar. A entrega foi uma API REST documentada, com autenticação própria e estrutura preparada para crescer como microserviço.

## Contexto do Projeto
O cliente queria um aplicativo que ajudasse os usuários a:
- Controlar a alimentação diária com tabelas de alimentos curadas a partir de fontes confiáveis.
- Receber alertas para desvios na dieta em relação à meta.
- Acompanhar progresso com gráficos de evolução personalizados.
- Seguir planos alimentares adaptados a objetivos e preferências individuais.

Como o cliente não tinha equipe de back-end e o produto seria consumido por outras aplicações (futuro app mobile, dashboard web, integrações), a solução foi entregue como API REST, deployada em servidor.

## Desenvolvimento

### Stack Técnica
- **Java 11**: linguagem principal.
- **Spring Boot 2.7.2**: framework principal.
- **Spring Data JPA**: persistência com Hibernate.
- **Spring Security + JWT (`java-jwt` 4.4.0)**: autenticação stateless, token com validade de 2h.
- **Springfox Swagger 2.9.2 + Swagger UI**: documentação viva da API.
- **ModelMapper 2.3.0**: conversão entre entidades e DTOs.
- **Lombok**: redução de boilerplate nas entidades.
- **MySQL**: banco de dados.
- **Maven**: gerenciador de dependências (com `maven-shade-plugin` para gerar jar único executável).

### Arquitetura em Camadas
O código segue a estrutura clássica em camadas, com pacotes bem definidos:
- **`controller`** — endpoints REST. Subpacotes auxiliares:
  - `documentacao/` — Swagger separado por controller (uma classe `*Swagger.java` por `*Controller.java`).
  - `filtros/` — tipos de filtros aceitos nos GETs de listagem.
- **`domain`** — entidades JPA. Subpacote `dto/` com DTOs de inserção separados dos DTOs de visualização.
- **`repositories`** — interfaces Spring Data JPA.
- **`service`** — regras de negócio e CRUD. Subpacote `exception/` com tratamento de erros e respostas personalizadas.
- **`security`** — autenticação, geração de token JWT, liberação de acesso por endpoint.

### Decisões Arquiteturais
- **Swagger por controller**: cada controller tem um `*Swagger.java` correspondente, mantendo a documentação alinhada com o código.
- **DTOs de inserção vs visualização**: o DTO de entrada tem só validação; o de saída tem só o que deve ser exposto (campos sensíveis, IDs internos, joins).

## Funcionalidades
A API cobre o ciclo de uso do aplicativo de nutrição:
- **Alimentos**: cadastro a partir de 3 fontes confiáveis, com categorias, kcal e macronutrientes.
- **Consumo de alimentos**: registro do que o usuário comeu (refeição, quantidade, alimento).
- **Check-ins diário, semanal e mensal**: ponto de entrada para o usuário registrar seu progresso.
- **Cálculos**: estratégia, indicadores e objetivo do usuário — endpoints que calculam metas calóricas, desvios, sugestões de plano.
- **Plano alimentar e adesão**: geração e acompanhamento do plano.
- **Metas (kcal, passos, macronutrientes)**: definição e progresso.
- **Feedback e checklist inicial**: onboarding e retorno ao usuário.
- **Autenticação (`/v1/auth/login`)**: gera token JWT com 2h de validade.

## Desafios
- **Configurar Spring Security + JWT**: fluxo de `Filter`, `UserDetailsService`, geração/validação de token, expiração, e como liberar endpoint público (`/auth/login`, `/usuarios` para cadastro) sem autenticação.
- **Decidir granularidade do DTO**: separar o que vem do cliente do que vai para o cliente, evitando vazar campos internos pela API.
- **Manter Swagger sincronizado com o código**: solução adotada foi ter um arquivo `*Swagger.java` por controller.
- **Maven Shade Plugin**: gerar um jar "uber" (fat jar) com todas as dependências para deploy em servidor sem servidor de aplicação.

## Aprendizados
- **Primeira API REST do portfólio**: marcou a transição de sistemas desktop para APIs.
- **Spring Boot + JPA + REST do zero**: controllers, services, repositories, DTOs, validação, exception handler e Swagger.
- **Primeiro deploy em servidor**: saiu do desktop para rodar em nuvem (EC2).
- **Swagger separado por controller** e **DTOs de inserção vs visualização separados**: passaram a ser aplicados como padrão nos projetos seguintes.

## Conclusão
A api-mbs foi o primeiro microserviço, o primeiro projeto Spring Boot completo, o primeiro deploy em nuvem (AWS EC2) e o primeiro trabalho freelance de API. As decisões arquiteturais tomadas aqui (Swagger separado por controller, DTOs de inserção vs visualização, arquitetura em camadas) foram aplicadas nos projetos seguintes de API do portfólio.