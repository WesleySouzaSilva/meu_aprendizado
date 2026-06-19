# Sistema para Jardinagem (sistema_jardinagem_samuel)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Este projeto é um sistema desktop de gestão para um negócio de jardinagem e paisagismo, desenvolvido como variação da família de sistemas para oficinas mecânicas. Cobre, em um único aplicativo, orçamento, ordem de serviço, cadastro de clientes PF/PJ, veículos, agenda, retorno de clientes, funcionários, despesas, pagamento de funcionários, controle de itens e relatórios operacionais e gerenciais.

## Contexto do Projeto
O cliente operava um negócio de jardinagem e paisagismo e controlava o movimento em planilhas e cadernos. A motivação foi centralizar todas as operações em um sistema único, cobrindo orçamento, ordem de serviço, agenda, retorno de clientes, controle de itens, funcionários, despesas, pagamento de funcionários e relatórios — substituindo controles paralelos por um aplicativo desktop único.

O sistema foi pensado para ser operado por um ou dois usuários no negócio, com o dono acessando relatórios para acompanhar o movimento.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal.
- **JavaFX + FXML + Scene Builder**: camada de apresentação, com separação entre view e controller.
- **JDBC puro**: camada de persistência, com `AbstractGenericDAO` aplicado em todas as entidades.
- **MySQL**: banco de dados.
- **JasperReports**: geração de relatórios em PDF, com 6 `.jrxml` cobrindo orçamento, ordem de serviço, pagamento, pagamento de despesa, retorno e serviço.

### Organização do Código
- **`view/`** — telas em FXML (35 telas).
- **`controll/`** — controllers JavaFX (uma classe por tela).
- **`model/`** — entidades e `model/dao/` com DAOs por entidade (OrcamentoDAO, OrdemServicoDAO, PagamentoDAO, FuncionarioDAO, VeiculoDAO, etc.).
- **`relatorios/`** — arquivos `.jrxml` do JasperReports.
- **`conexao/`** — `Conexao` (JDBC) e `AbstractGenericDAO` (DAO genérico).
- **`filtros/`** — utilitários para entrada de dados.
- **`listeners/`** — listeners para componentes JavaFX (máscaras, formatação).
- **`validadores/`** — validação de campos (CPF, CNPJ, placas, etc.).

## Funcionalidades
- **Cadastro de clientes PF e PJ**, com endereço, telefones e e-mails.
- **Cadastro de veículos** com edição dedicada dos dados do veículo.
- **Ordem de serviço e orçamento**: telas dedicadas (`TelaOrdemServico`, `TelaOrcamento`), com serviços, mão de obra e cálculo automático de total.
- **Agenda** com telas de adicionar, listar e marcar retornos de clientes (`TelaAgenda`, `TelaAdicionarAgenda`, `TelaMarcarRetorno`, `TelaRetorno`).
- **Controle de itens** com cadastro dedicado (`TelaCadastroItem`).
- **Funcionários**: cadastro, edição, pagamento, vale, férias e relatórios.
- **Despesas**: cadastro, edição e relatório de despesas.
- **Pagamento de funcionários** com fluxo dedicado (adição, edição, parcelado).
- **Usuários do sistema** com telas de cadastro, edição e login dedicado (`TelaLogin`, `TelaNovoUsuario`, `TelaEditarUsuario`).
- **Empresas e lista de empresas** para gestão dos dados da empresa.
- **Relatórios operacionais e gerenciais** (JasperReports): orçamento, ordem de serviço, pagamento, pagamento de despesa, retorno e serviço.

## Desafios
- **Adaptação para o segmento de jardinagem**: o principal desafio foi adaptar a base consolidada de oficinas mecânicas para as particularidades de um negócio de jardinagem e paisagismo. Embora o esqueleto seja o mesmo (orçamento, OS, clientes, veículos, financeiro), o sistema precisou incorporar fluxo próprio de retorno de clientes (`TelaMarcarRetorno`, `TelaRetorno`) e controle de itens, mantendo a coerência entre orçamento, ordem de serviço e agenda.
- **Modelagem de retorno de clientes**: o segmento de jardinagem costuma trabalhar com visitas de retorno (manutenção periódica, podas sazonais), o que exigiu telas específicas para marcar e listar retornos, integradas à agenda geral do sistema.
- **Consistência entre orçamento, OS e agenda**: alterações em cliente impactavam orçamento, ordem de serviço e agenda. Garantir que essas relações ficassem íntegras ao longo do sistema foi um cuidado permanente.

## Aprendizados
- **Reuso da arquitetura em outro segmento**: o sistema confirmou a portabilidade do padrão JavaFX + JDBC + Jasper + DAO genérico (`AbstractGenericDAO`) para mais um segmento (jardinagem e paisagismo), aproveitando boa parte da base consolidada de oficinas mecânicas e adicionando apenas o que era específico do segmento.
- **Modelagem de fluxo de retorno**: a separação entre agenda geral e retorno de clientes mostrou como a base consolidada pode ser estendida com novos módulos sem reescrever a arquitetura, apenas com novas telas, entidades e DAOs.
- **Disciplina na camada DAO**: o `AbstractGenericDAO` e a replicação do padrão DAO por entidade foram essenciais para absorver o volume de entidades e manter consistência entre orçamento, ordem de serviço, financeiro e relatórios.

## Conclusão
O `sistema_jardinagem_samuel` consolidou o uso da arquitetura desktop padrão do portfólio em um cliente de jardinagem e paisagismo, com fluxo próprio de retorno de clientes como principal particularidade. Foi deployado em produção no cliente de jardinagem e demonstrou que a arquitetura JavaFX + JDBC + Jasper + DAO genérico é portável entre segmentos do setor de prestação de serviços, bastando customizar os módulos específicos de cada tipo de negócio.
