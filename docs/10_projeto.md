# Sistema para Funilaria (sistema_funilaria_leandro)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Este projeto é um sistema desktop de gestão para uma funilaria, desenvolvido como variação da família de sistemas para oficinas mecânicas. Cobre, em um único aplicativo, orçamento, ordem de serviço, cadastro de clientes PF/PJ, veículos, controle de peças e materiais (com e sem estoque), funcionários, comissões, despesas, pagamento de funcionários, agenda e relatórios operacionais e gerenciais.

## Contexto do Projeto
O cliente operava uma funilaria e controlava o movimento em planilhas e cadernos. A motivação foi centralizar todas as operações em um sistema único, cobrindo orçamento, ordem de serviço, peças (com estoque e avulsas), materiais, agenda, funcionários, comissões, despesas, pagamento de funcionários e relatórios — substituindo controles paralelos por um aplicativo desktop único.

O sistema foi pensado para ser operado por um ou dois usuários na funilaria, com o dono acessando relatórios para acompanhar o movimento do negócio.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal.
- **JavaFX + FXML + Scene Builder**: camada de apresentação, com separação entre view e controller.
- **JDBC puro**: camada de persistência, com `AbstractGenericDAO` aplicado em todas as entidades.
- **MySQL**: banco de dados.
- **JasperReports**: geração de relatórios em PDF, com 9 `.jrxml` cobrindo orçamento, ordem de serviço, comissão, pagamento, serviço, lista de material e lista de compra de material.

### Organização do Código
- **`view/`** — telas em FXML (61 telas).
- **`controll/`** — controllers JavaFX (62 classes, uma por tela).
- **`model/`** — entidades e `model/dao/` com DAOs por entidade (OrcamentoDAO, OrdemServicoDAO, PecasDAO, PagamentoDAO, FuncionarioDAO, VeiculoDAO, etc.).
- **`relatorios/`** — arquivos `.jrxml` do JasperReports.
- **`conexao/`** — `Conexao` (JDBC) e `AbstractGenericDAO` (DAO genérico).
- **`filtros/`** — utilitários para entrada de dados.
- **`listeners/`** — listeners para componentes JavaFX (máscaras, formatação).
- **`validadores/`** — validação de campos (CPF, CNPJ, placas, etc.).

## Funcionalidades
- **Cadastro de clientes PF e PJ**, com endereço, telefones e e-mails.
- **Cadastro de veículos** com edição dedicada dos dados do veículo.
- **Ordem de serviço e orçamento**: telas dedicadas (`TelaOrdemServico`, `TelaOrcamento`), com peças (genéricas, com estoque e avulsas), serviços, mão de obra e cálculo automático de total.
- **Peças e materiais** com dois fluxos distintos: peças com estoque (`TelaPecasComEstoque`, `TelaCadastroItensPecas`, `TelaQuantidadeItensPecas`, `TelaListaEstoquePecas`, `TelaHistoricoItens`) e materiais avulsos (`TelaAdicionarMaterial`, `TelaCompraMaterial`, `TelaListaMaterial`, `TelaListaCompraMaterial`, `TelaExcluirMaterial`).
- **Agenda** com telas de adicionar, listar e editar agendamentos.
- **Funcionários**: cadastro, edição, pagamento, vale, férias e comissão.
- **Despesas**: cadastro, edição e relatório de despesas.
- **Pagamento de funcionários** com fluxo dedicado (adição, edição, parcelado).
- **Usuários do sistema** com telas de cadastro, edição e login dedicado (`TelaLogin`, `TelaNovoUsuario`, `TelaEditarUsuario`).
- **Relatórios operacionais e gerenciais** (JasperReports): orçamento, ordem de serviço, comissão, pagamento, pagamento de despesa, serviço, lista de material e lista de compra de material.

## Desafios
- **Adaptação para o segmento de funilaria**: o principal desafio foi adaptar a base consolidada de oficinas mecânicas para as particularidades de uma funilaria. Embora o esqueleto seja o mesmo (orçamento, OS, peças, clientes, veículos, financeiro), o sistema precisou incorporar dois fluxos distintos de peças/materiais (com estoque e avulsos), além de cobrir pintura, funilaria e os serviços próprios do segmento, mantendo a consistência entre orçamento e ordem de serviço.
- **Modelagem de peças em dois fluxos**: diferenciar peças com controle de estoque (entrada, saída, histórico, quantidade) de materiais avulsos comprados sob demanda exigiu DAOs e telas específicas para cada fluxo, sem misturar as regras de negócio.
- **Manutenção da coerência entre orçamento e OS**: ao converter um orçamento aprovado em ordem de serviço, peças, serviços e valores precisaram ser preservados sem inconsistência, o que exigiu cuidado na camada DAO e nas controllers.

## Aprendizados
- **Reuso da arquitetura em outro segmento**: o sistema confirmou a portabilidade do padrão JavaFX + JDBC + Jasper + DAO genérico (`AbstractGenericDAO`) para um segmento diferente (funilaria), aproveitando boa parte da base consolidada de oficinas mecânicas e adicionando apenas o que era específico do segmento.
- **Modelagem de peças com dois fluxos**: a separação entre peças com estoque e materiais avulsos mostrou como a base consolidada pode ser estendida sem reescrever a arquitetura, apenas com novas entidades, DAOs e telas.
- **Disciplina na camada DAO**: o `AbstractGenericDAO` e a replicação do padrão DAO por entidade foram essenciais para absorver o volume de entidades e manter consistência entre orçamento, ordem de serviço, financeiro e relatórios.

## Conclusão
O `sistema_funilaria_leandro` consolidou o uso da arquitetura desktop padrão do portfólio em um cliente de funilaria, com a separação de peças com estoque e materiais avulsos como principal particularidade. Foi deployado em produção no cliente de funilaria e demonstrou que a arquitetura JavaFX + JDBC + Jasper + DAO genérico é portável entre segmentos do setor automotivo, bastando customizar os módulos específicos de cada tipo de negócio.
