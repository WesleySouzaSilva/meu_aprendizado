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
Este foi o **primeiro microserviço** que desenvolvi profissionalmente, e também o primeiro projeto em que usei **Spring Boot** de verdade. Foi um trabalho freelance — um cliente da área de nutrição me procurou precisando de uma API back-end para um aplicativo de controle alimentar. Antes disso, meus sistemas eram todos desktop em JavaFX; aqui foi a virada para o mundo de APIs REST, banco na nuvem e deploy em servidor.

## Contexto do Projeto
O cliente queria um aplicativo que ajudasse os usuários a:
- Controlar a alimentação diária com tabelas de alimentos curadas a partir de fontes confiáveis
- Receber **alertas inteligentes** para desvios na dieta em relação à meta
- Acompanhar progresso com **gráficos de evolução** personalizados
- Seguir **planos alimentares** adaptados a objetivos e preferências individuais

Como o cliente não tinha equipe de back-end e o produto era uma API consumida por outras aplicações (futuro app mobile, dashboard web, integrações), entreguei a solução como **API REST documentada**, com autenticação própria e estrutura preparada para crescer como microserviço.

## Desenvolvimento

### Stack Técnica
- **Java 11**: linguagem principal (migrei do Java 8 do desktop para o Java 11 exigido pelo Spring Boot 2.7.x)
- **Spring Boot 2.7.2**: framework principal
- **Spring Data JPA**: persistência com Hibernate
- **Spring Security + JWT (`java-jwt` 4.4.0)**: autenticação stateless, token com validade de 2h
- **Springfox Swagger 2.9.2** + **Swagger UI**: documentação viva da API
- **ModelMapper 2.3.0**: conversão entre entidades e DTOs
- **Lombok**: redução de boilerplate nas entidades
- **MySQL**: banco de dados
- **Maven**: gerenciador de dependências (com `maven-shade-plugin` para gerar jar único executável)

### Arquitetura em Camadas
O código segue a estrutura clássica em camadas, com pacotes bem definidos:
- **`controller`** — endpoints REST. Subpacotes auxiliares:
  - `documentacao/` — Swagger separado por controller (uma classe `*Swagger.java` por `*Controller.java`)
  - `filtros/` — tipos de filtros aceitos nos GETs de listagem
- **`domain`** — entidades JPA. Subpacote `dto/` com **DTOs de inserção separados dos DTOs de visualização**
- **`repositories`** — interfaces Spring Data JPA
- **`service`** — regras de negócio e CRUD. Subpacote `exception/` com tratamento de erros e respostas personalizadas
- **`security`** — autenticação, geração de token JWT, liberação de acesso por endpoint

### Por que Swagger e DTOs separados?
Foram duas decisões arquiteturais que valeram a pena e viraram padrão nos meus projetos de API depois:

- **Swagger por controller**: facilita manter a documentação alinhada com o código (cada classe Swagger documenta apenas o controller correspondente, sem virar um arquivo gigante).
- **DTOs de inserção vs visualização**: nunca misturar o que vem **do** cliente com o que vai **para** o cliente. O DTO de entrada tem só validação; o de saída tem só o que deve ser exposto (campos sensíveis, IDs internos, joins).

## Funcionalidades
A API cobre todo o ciclo de uso do aplicativo de nutrição:
- **Alimentos**: cadastro de alimentos a partir de 3 fontes confiáveis, com categorias, kcal e macronutrientes.
- **Consumo de alimentos**: registro do que o usuário comeu (refeição, quantidade, alimento).
- **Check-ins diário, semanal e mensal**: ponto de entrada para o usuário registrar seu progresso.
- **Cálculos**: estratégia, indicadores e objetivo do usuário — endpoints que calculam metas calóricas, desvios, sugestões de plano.
- **Plano alimentar e adesão**: geração e acompanhamento do plano.
- **Metas (kcal, passos, macronutrientes)**: definição e progresso.
- **Feedback e checklist inicial**: onboarding e retorno ao usuário.
- **Autenticação (`/v1/auth/login`)**: gera token JWT com 2h de validade.

## Desafios
Por ter sido meu primeiro contato sério com Spring Boot, vários pontos exigiram estudo dedicado:
- **Configurar Spring Security + JWT do zero**: entender o fluxo de `Filter`, `UserDetailsService`, geração/validação de token, expiração, e como liberar endpoint público (`/auth/login`, `/usuarios` para cadastro) sem autenticação. Foi a parte que mais exigiu leitura de documentação e tentativa/erro.
- **Decidir granularidade do DTO**: a tentação inicial era usar a própria entidade JPA como retorno, mas isso vaza campos internos e dificulta manutenção. Separar DTOs de inserção/visualização foi a virada para um código mais limpo.
- **Manter Swagger sincronizado com o código**: a solução foi ter um arquivo `*Swagger.java` por controller. Sem essa disciplina, a documentação defasa rapidamente.
- **Maven Shade Plugin**: gerar um jar "uber" (fat jar) com todas as dependências para deploy em EC2 sem servidor de aplicação foi novo — antes só rodava desktop.

## Aprendizados
Este projeto consolidou três aprendizados que carrego até hoje:
1. **Spring Boot + JPA + REST** do zero. Foi aqui que aprendi controllers, services, repositories, DTOs, validação, exception handler e Swagger. Tudo que veio depois em outros projetos de API partiu daqui.
2. **Documentação Swagger separada por controller** virou padrão nos meus projetos — facilita manutenção e revisão.
3. **DTOs de inserção vs visualização separados** virou regra: nunca misturar o que entra com o que sai.

Também foi aqui que ficou claro que **conhecer bem o domínio** (no caso, regras de nutrição, cálculo de meta calórica, alertas de desvio) é tão importante quanto dominar o framework.

## Conclusão
A api-mbs foi o divisor de águas na minha carreira: a transição de **desenvolvedor desktop** para **desenvolvedor de APIs**. Foi o primeiro projeto Spring Boot completo, o primeiro microserviço, o primeiro deploy em EC2, e o primeiro trabalho freelance de API. Foi deployado em produção na AWS e rodou para o cliente. As decisões arquiteturais que tomei aqui (Swagger separado, DTOs split, camadas claras) viraram o padrão dos meus projetos seguintes de API.