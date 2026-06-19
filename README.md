# Minha Jornada Profissional e de Aprendizado em Desenvolvimento de Software

## Índice
1. [Introdução](#introdução)
2. [Primeiros Passos](#primeiros-passos)
3. [Projetos Documentados](#projetos-documentados)
4. [Contatos e Links](#contatos-e-links)

## Introdução
Olá! Meu nome é Wesley Souza e sou consultor de vendas em uma concessionária da minha cidade. Tenho mais de 10 anos de experiência de trabalho, iniciada em 2014, principalmente no setor de peças automotivas Chevrolet. Minhas atividades incluem:
- Vendas
- Lançamento de notas fiscais de compra
- Emissão de notas fiscais de venda de produtos (com e sem impostos)
- Controle de qualidade do estoque
- Controle de reposição de peças de estoque
- Entendimento de curva ABC (estoque)

Em 2016, comecei a me interessar por software. Em 2017, iniciei um curso de Java na cidade vizinha. Foi um ano de estudo aos sábados. Sempre fiquei entusiasmado quando chegava o dia de estudar. Após a conclusão do curso, comecei a aplicar meus conhecimentos para treinar minhas habilidades. Desenvolvi duas soluções para o meu setor:
- Automatizei o controle de estoque mínimo de itens, criando um programa onde efetuamos a leitura de um arquivo CSV do sistema da empresa e manipulamos os dados na minha ferramenta. Com base no estoque de saídas de itens e na quantidade de estoque restante, conseguia aplicar regras na minha aplicação e definir o estoque mínimo desejado. No final, também gerava um relatório em PDF para poder imprimir se necessário, permitindo a tomada de decisão sobre a reposição de itens.
- Criei um programa para controlar peças fora de estoque (peças velhas ou danificadas), embalando-as e relacionando-as com código e local de armazenamento. No sistema desenvolvido, conseguimos localizar a peça pelo código.

Com essas habilidades adquiridas, comecei a oferecer meus serviços aos meus clientes, que incluem oficinas mecânicas, auto elétricas, lava-jatos, entre outros. Em 2018, desenvolvi um sistema de gestão para oficinas mecânicas, meu primeiro projeto para um cliente, e o mais desafiador até então, pois consegui efetuar uma venda a partir da necessidade do cliente.

## Primeiros Passos
### [Introdução aos Primeiros Passos]
Neste momento (2018), comecei a me interessar por software e busquei opções na minha cidade. Não encontrei, então procurei na cidade vizinha, onde encontrei um curso de Java com duração de um ano. Iniciei o curso em 2017 e, em 2018, comecei a aplicar o que havia aprendido. Assim, criei os programas para minha empresa no meu setor e meu primeiro projeto para um cliente de software, conforme informado na [Introdução](#introdução).

## Projetos Documentados
Abaixo estão os projetos do portfólio já documentados neste repositório. Cada um tem um arquivo próprio com o contexto, a stack, as funcionalidades e os aprendizados do desenvolvimento.

### [Sistema para Oficinas Mecânicas](docs/primeiro_projeto.md)
Meu primeiro projeto foi o sistema de gestão para oficinas mecânicas, no qual tive muito empenho e dedicação, e também enfrentei alguns desafios. O projeto era um sistema desktop, desenvolvido com Java 8, JavaFX (interface gráfica da aplicação), Scene Builder (para efetuar os protótipos das telas em FXML), MySQL como banco de dados e Jasper Reports para gerar os relatórios do sistema.

### [Sistema Oficina — Projeto Base](docs/9_projeto.md)
Sistema desktop em Java 8 + JavaFX + JDBC + MySQL + JasperReports que serviu de molde para todas as variações posteriores voltadas a outros clientes do segmento de oficinas mecânicas. Cobre orçamento, ordem de serviço, controle de peças, mão de obra, clientes, veículos, contas a pagar/receber e relatórios.

### [API de Nutrição (api-mbs)](docs/13_projeto.md)
Primeira API REST profissional do portfólio, desenvolvida com Java 11 + Spring Boot 2.7.2 + Spring Data JPA + Spring Security (JWT) + MySQL. Back-end de um aplicativo de controle alimentar, com autenticação stateless, documentação Swagger e DTOs de inserção/visualização separados.

### [Sistema de Revenda (sistema_revendedora)](docs/6_projeto.md)
Sistema-base da família de sistemas para revendedoras, em Java 8 + JavaFX + JDBC + MySQL + JasperReports. Cobre o ciclo completo da revenda de veículos: clientes (PF e PJ), veículos, fluxo de vendas, financeiro, despesas, funcionários, CRM de clientes interessados e relatórios gerenciais (DRE, comissão de vendedores).

### [Atualizador via Dropbox (download_api_dropbox)](docs/7_projeto.md)
Atualizador automático desktop em Java 8 + Swing + Dropbox Core SDK. Verifica se há versão mais recente do sistema na nuvem, compara a data do arquivo local com a metadata do Dropbox e, quando há atualização, baixa o arquivo em thread separada (SwingWorker) com diálogo modal e barra de progresso. Configuração externa via `config.properties` + `atualizador.xml`, reaproveitada para múltiplos clientes.

### [Sistema de Controle de Frota e Turismo (sistema_controle_frota_dpaula)](docs/8_projeto.md)
Sistema desktop em Java 8 + JavaFX + JDBC + MySQL + JasperReports para gestão integrada de frota e turismo. Cobre cadastro de clientes PF/PJ, veículos com custos específicos (combustível, IPVA, manutenção, pedágio, seguro, vistoria, revisão preventiva, parcelas), agenda de turismo, funcionários, despesas, contas a pagar/receber e conjunto amplo de relatórios operacionais e gerenciais (DRE, agenda, veículo por tipo de custo).

### [Sistema para Funilaria (sistema_funilaria_leandro)](docs/10_projeto.md)
Sistema desktop em Java 8 + JavaFX + JDBC + MySQL + JasperReports para gestão de funilaria, derivado da base consolidada de oficinas mecânicas. Cobre orçamento, ordem de serviço, peças com estoque, materiais avulsos, agenda, funcionários, comissões, despesas, pagamento de funcionários e relatórios operacionais e gerenciais (orçamento, OS, comissão, pagamento, serviço, lista de material).

### [Sistema para Jardinagem (sistema_jardinagem_samuel)](docs/11_projeto.md)
Sistema desktop em Java 8 + JavaFX + JDBC + MySQL + JasperReports para gestão de negócio de jardinagem e paisagismo, derivado da base consolidada de oficinas mecânicas. Cobre orçamento, ordem de serviço, agenda, retorno de clientes, controle de itens, funcionários, despesas, pagamento de funcionários e relatórios operacionais e gerenciais (orçamento, OS, pagamento, retorno, serviço).

## Contatos e Links
- [LinkedIn](https://www.linkedin.com/in/wesley-souza-b79841191/)
- [Portfólio](https://github.com/WesleySouzaSilva)
