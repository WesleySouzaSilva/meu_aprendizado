# Sistema de Controle de Frota e Turismo (sistema_controle_frota_dpaula)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Este projeto é um sistema desktop de **gestão de frota e turismo** desenvolvido para um cliente do segmento de transporte. Cobre, em um único aplicativo, o cadastro de clientes, veículos, agenda de turismo, funcionários, controle financeiro (contas a pagar/receber, despesas) e um conjunto amplo de relatórios operacionais e gerenciais.

## Contexto do Projeto
O cliente operava com **frota de veículos e agenda de turismo** e precisava centralizar a gestão completa em um sistema único, substituindo controles paralelos em planilhas e cadernos. A motivação foi cobrir, em um só lugar, todas as operações do negócio: veículos e seus custos (combustível, IPVA, manutenção, pedágio, seguro, vistoria, revisão preventiva), agenda de turismo (adição, listagem e cancelamento), clientes PF/PJ, funcionários e o financeiro (despesas da empresa, veículo, vale, salário, diversas, contas a pagar/receber).

O sistema foi pensado para ser operado por um ou dois usuários na empresa, com o dono acessando relatórios para acompanhar o movimento.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal.
- **JavaFX + FXML + Scene Builder**: camada de apresentação, com separação entre view e controller.
- **JDBC puro**: camada de persistência, com `AbstractGenericDAO` aplicado em todas as entidades.
- **MySQL**: banco de dados (scripts `sistema_frota dados.sql` e `sistema_frota_limpo.sql` na raiz do projeto).
- **JasperReports**: geração de relatórios em PDF, com 19 `.jrxml` cobrindo frota, turismo, financeiro e funcionários.

### Organização do Código
- **`view/`** — telas em FXML.
- **`controll/`** — controllers JavaFX (com classes específicas para cada tela: `TelaAdicionarAgendaTurismo`, `TelaCadastroClientesPF`, `TelaCombustivelVeiculos`, etc.).
- **`model/`** — entidades e `model/dao/` com DAOs por entidade.
- **`relatorios/`** — arquivos `.jrxml` do JasperReports.
- **`conexao/`** — `Conexao` (JDBC) e `AbstractGenericDAO` (DAO genérico).
- **`filtros/`** — utilitários para entrada de dados.
- **`listeners/`** — listeners para componentes JavaFX (máscaras, formatação).
- **`validadores/`** — validação de campos (CPF, CNPJ, placas, etc.).

## Funcionalidades
- **Cadastro de clientes PF e PJ**, com endereço, telefones e e-mails.
- **Cadastro de veículos** com operações dedicadas por tipo de custo:
  - Combustível
  - IPVA
  - Manutenção
  - Pedágio
  - Seguro
  - Vistoria
  - Revisão preventiva
  - Parcelas e financiamentos
- **Agenda de turismo**: adicionar, listar e cancelar agendamentos (`TelaAdicionarAgendaTurismo`, `TelaListaAgenda`, `TelaCancelarTurismo`, `TelaEditarTurismo`).
- **Funcionários**: cadastro, salário, vale, férias.
- **Despesas** divididas por origem: empresa, veículo, salário, vale e diversas.
- **Contas a pagar e a receber** com fluxo de edição dedicado.
- **Relatórios operacionais e gerenciais** (JasperReports): DRE, despesas, funcionários, agenda de turismo, contas a receber, veículo (geral, consumo, IPVA, manutenção, pedágio, seguro, vistoria, revisão preventiva, parcelas).

## Desafios
- **Volume de módulos / telas**: o principal desafio foi a quantidade de módulos e telas interligadas (frota, turismo, financeiro, funcionários, agenda). Cada área tinha CRUD próprio, com regras específicas e várias telas de edição dedicadas (`TelaEditar*`). Manter a navegação, a validação e a consistência entre as áreas exigiu disciplina na camada DAO e nos controllers.
- **Modelagem dos custos por veículo**: cada tipo de custo do veículo (combustível, IPVA, manutenção, pedágio, seguro, vistoria, preventiva, parcelas) é uma entidade própria com relatórios específicos, o que multiplicou o número de tabelas, DAOs e `.jrxml`.
- **Consistência entre módulos**: alterações em veículo impactavam agenda, financeiro e relatórios. Garantir que essas relações ficassem íntegras ao longo do sistema foi um cuidado permanente.

## Aprendizados
- **Relatórios complexos por veículo**: o destaque técnico foi a construção de um conjunto amplo de relatórios JasperReports focados no veículo — cada tipo de custo com seu `.jrxml` próprio (consumo, IPVA, manutenção, pedágio, seguro, vistoria, revisão preventiva, parcelas), além dos relatórios gerenciais clássicos (DRE, funcionários, despesas, agenda).
- **Reuso da arquitetura desktop em outro domínio**: reaproveitamento integral do padrão JavaFX + JDBC + Jasper + DAO genérico, agora aplicado a um domínio de frota + turismo, confirmando a portabilidade da arquitetura.
- **Disciplina na camada DAO**: o `AbstractGenericDAO` e a replicação do padrão DAO por entidade foram essenciais para absorver o volume de entidades sem perder consistência.

## Conclusão
O `sistema_controle_frota_dpaula` consolidou o uso da arquitetura desktop padrão do portfólio em um domínio de **frota e turismo**, com um conjunto robusto de relatórios JasperReports focados nos custos do veículo e no movimento da agenda. Foi deployado em produção no cliente de transporte e demonstrou que a arquitetura JavaFX + JDBC + Jasper escala bem quando o volume de módulos e relatórios cresce.
