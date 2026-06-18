# Sistema de Revenda (sistema_revendedora)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Variações](#variações)
8. [Conclusão](#conclusão)

## Introdução
Este projeto é o **sistema-base da família de sistemas para revendedoras** do portfólio. Foi desenvolvido para uma revenda de veículos que precisava cobrir todo o ciclo de operação em um único sistema desktop: cadastro de clientes (PF e PJ), cadastro e histórico de veículos, fluxo de vendas, controle financeiro, despesas e relatórios para o dono.

## Contexto do Projeto
O cliente operava uma revenda de veículos e precisava de uma ferramenta que substituísse planilhas e controles paralelos. A motivação foi **centralizar a gestão completa**: clientes, veículos, vendas e financeiro em um único sistema. Antes, esses controles ficavam distribuídos em planilhas e cadernos, o que gerava furos de informação e dificultava o fechamento do caixa.

O sistema foi pensado para ser usado por um ou dois operadores na loja, com um dono que precisava de relatórios claros para tomar decisões (vendas por período, comissão de vendedores, estoque parado, DRE).

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal.
- **JavaFX + FXML + Scene Builder**: camada de apresentação, com separação de view (`.fxml`) e controller.
- **JDBC puro**: camada de persistência.
- **DAO genérico**: padrão aplicado em todas as entidades, evitando repetição de CRUD.
- **MySQL**: banco de dados (script `sistema_revendedora.sql` na raiz do projeto).
- **JasperReports 5.5/6.4/6.6**: geração de relatórios em PDF, com sub-relatórios.
- **iText 2.1.7**: manipulação de PDF.
- **Jackson 2.11**: serialização JSON (para uso com JasperReports ou exportação).
- **Spring Beans/Core 5.1**: injeção leve (vindo provavelmente como dependência do JasperReports).

### Organização do Código
O projeto segue a mesma estrutura em camadas da família de sistemas desktop:
- **`view/`** — telas em FXML (separadas da lógica).
- **`controll/`** — controllers JavaFX.
- **`model/`** — entidades e DAOs (com subpacote `dao/`).
- **`relatorios/`** — arquivos `.jrxml` e `.jasper` (JasperReports).
- **`filtros/`** — classes utilitárias para entrada de dados.
- **`listeners/`** — listeners para componentes JavaFX (máscaras, formatação).
- **`validadores/`** — validação de campos (CNPJ, CPF, placas, etc).
- **`conexao/`** — classe `Conexao` para JDBC.

## Funcionalidades
A aplicação cobre o ciclo completo de uma revenda de veículos:
- **Cadastro de clientes PF e PJ** (com endereço, telefones, e-mails).
- **Cadastro de veículos** (com histórico, IPVA, Fipe, prevenções).
- **Rotas e escalas** (entrega de veículos).
- **Fluxo de venda** (orçamento → venda → entrega).
- **Pagamentos** (de cliente e de veículo).
- **Contas a pagar e a receber**.
- **Despesas** (da empresa, veículo, vale, salário, diversas).
- **Funcionários** (cadastro, salário, escala, férias).
- **Relatórios para o dono**: DRE (Demonstrativo de Resultado de Exercício), Vendas por Vendedor (comissão), Vendas, Veículos, Contas a Receber, Despesas, Lista, Funcionários.

## Desafios
- **Relatórios complexos para o dono**: a parte mais trabalhosa foi gerar relatórios completos que o dono realmente usasse para decidir — DRE, comissão de vendedores, vendas por período, estoque parado. A solução foi montar cada relatório em JasperReports com sub-relatórios para cruzar dados de várias tabelas (vendas + vendedor + veículo, por exemplo).
- **Integração entre módulos**: vendas impactam estoque, contas a receber, comissão de vendedor e DRE. Manter essa consistência exigiu cuidado na camada DAO.
- **Histórico de veículos**: registrar todas as ocorrências do veículo (entrada, manutenções, vendas anteriores, IPVA) em uma única tela foi um desafio de modelagem.

## Aprendizados
- **Gestão completa em um sistema só**: o exercício de cobrir clientes + veículos + vendas + financeiro em um sistema único foi importante para entender como módulos se interligam.
- **Relatórios JasperReports com sub-relatórios**: o destaque técnico foi aprender a montar relatórios que cruzam várias tabelas (ex.: vendas por vendedor com totais calculados em sub-relatórios).
- **Padrão DAO genérico**: aplicado em todas as entidades, acelerou o desenvolvimento e se consolidou como padrão da família desktop.

## Variações
Este projeto deu origem a versões para outras revendedoras, com adaptações pontuais para o fluxo de cada cliente. Essas variações não são documentadas individualmente para evitar repetição da arquitetura comum. O sistema `sistema_revendedora_janeo_loja2` (extensão da revenda do cliente original, com uma segunda loja) é a única variação documentada separadamente.

## Conclusão
O `sistema_revendedora` consolidou o padrão de sistema desktop em camadas usado em todo o portfólio, e foi o primeiro projeto em que o foco de relatórios gerenciais complexos (DRE, comissão de vendedor) ganhou peso. Foi deployado em produção na revenda original e deu origem a derivações para outras revendas da região.