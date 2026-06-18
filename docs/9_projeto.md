# Sistema para Oficina Mecânica — Projeto Base

## Índice
1. [Introdução](#introdução)
2. [Necessidade do Cliente](#necessidade-do-cliente)
3. [Desenvolvimento](#desenvolvimento)
4. [Desafios](#desafios)
5. [Facilidade de Desenvolvimento](#facilidade-de-desenvolvimento)
6. [Relatórios](#relatórios)
7. [Variações](#variações)
8. [Conclusão](#conclusão)

## Introdução
Este é o **sistema base** da família de sistemas desktop para oficinas mecânicas. Foi o primeiro projeto completo de gestão que entreguei, e serviu de molde para todas as variações posteriores voltadas a outros clientes do segmento. A ideia foi construir uma plataforma com orçamento, ordem de serviço, controle de peças, mão de obra, clientes, veículos, contas a pagar/receber e relatórios — tudo em um único aplicativo desktop.

## Necessidade do Cliente
Oficinas mecânicas de pequeno e médio porte que ainda faziam controle em papel precisavam de uma ferramenta que centralizasse:
- Controle de ordens de serviço (aberta → em andamento → finalizada)
- Registro de peças e mão de obra por OS, com cálculo automático de total
- Cadastro de clientes e seus veículos
- Contas a pagar (peças compradas, contas da oficina) e a receber (pagamentos dos clientes)
- Relatórios para análise do movimento da oficina

## Desenvolvimento
Sistema desktop construído com o stack que se tornou padrão nos meus projetos de oficina:
- **Java 8**: linguagem principal
- **JavaFX**: interface gráfica, com telas em FXML
- **Scene Builder**: prototipação visual das telas
- **MySQL 8**: banco de dados (driver `com.mysql.cj.jdbc.Driver`)
- **JasperReports 6.6**: geração de relatórios em PDF
- **JDBC puro** (DAO genérico + `Conexao.java`): acesso a dados — sem Hibernate, sem Spring Data

O projeto é estruturado em pacotes por responsabilidade:
- `br.com.sistema.conexao` — gerenciamento da conexão com MySQL
- `br.com.sistema.model` — entidades (Pessoa, Veiculo, OrdemServico, Orcamento, Pecas, Servico, Pagamento)
- `br.com.sistema.model.dao` — DAOs (um por entidade)
- `br.com.sistema.controll` — controllers FXML (TelaHome, TelaLogin, etc.)
- `br.com.sistema.listeners` — listeners utilitários (ex.: `ListenerParaMaiusculas` para forçar CAPS nos campos de texto)

### Funcionalidades Principais
- **Registro de Peças Sem Estoque**: o cliente pode registrar uma peça apenas com descrição, quantidade e valor unitário, sem precisar cadastrar previamente em um estoque. Para oficinas pequenas, esse modelo caiu muito bem e virou padrão nos sistemas seguintes.
- **Emissão de Orçamento e Ordem de Serviço**: fluxo completo, com conversão de orçamento em OS.
- **Gestão de Clientes e Veículos**: cadastro completo com histórico (orçamentos, OSs, pagamentos) por cliente.
- **Contas a Pagar e a Receber**: controle financeiro integrado à movimentação da oficina.
- **Relatórios PDF** (Jasper): DRE, despesas, receitas e relatórios específicos de OS.

## Desafios
Por ter sido um dos primeiros sistemas desktop completos que entreguei, enfrentei vários desafios clássicos:
- **Integração JavaFX + Scene Builder**: entender como o FXML conversa com os controllers e como manipular componentes programaticamente tomou tempo.
- **Gerenciamento de Estado entre Telas**: manter a aplicação consistente ao navegar entre menus, modais e abas exigiu cuidado.
- **Preenchimento da Tabela Principal**: o maior desafio técnico. A `TableView` do JavaFX aceita apenas **um tipo** em `setCellValueFactory`, mas a tabela principal precisava juntar dados de **3 classes diferentes** (OrdemServico, Pessoa, Veiculo). A solução foi criar uma classe de visualização (`VisualizacaoOrdem`) apenas para alimentar a tabela, e uma SQL com `JOIN` que retorna exatamente os campos que ela precisa. Funcionou bem e foi reaproveitada nos sistemas seguintes.

### Exemplo de SQL para a Tabela Principal

```sql
SELECT o.id AS ordem_id,
       p.id AS pessoa_id,
       p.nome AS pessoa_nome,
       o.servico AS servico_descricao,
       v.id AS veiculo_id,
       v.modelo AS veiculo_modelo,
       v.chassis AS veiculo_chassis,
       v.placa AS veiculo_placa,
       o.km AS quilometragem,
       DATE_FORMAT(o.data_entrada, '%d/%m/%Y') AS data_entrada_formatada,
       DATE_FORMAT(o.data_prometida, '%d/%m/%Y') AS data_prometida_formatada
FROM orcamento AS o, pessoa AS p, veiculo AS v
WHERE status = 'Aberto'
  AND p.id = o.pessoa_id
  AND o.veiculo_id = v.id;
```

### Exemplo da Classe de Visualização

```java
public class VisualizacaoOrdem {
    private Integer ordemId;
    private Integer pessoaId;
    private String pessoaNome;
    private String servicoDescricao;
    private Integer veiculoId;
    private String veiculoModelo;
    private String veiculoChassis;
    private String veiculoPlaca;
    private Integer quilometragem;
    private String dataEntradaFormatada;
    private String dataPrometidaFormatada;
    // getters e setters
}
```

### Exemplo de Preenchimento da Tabela

```java
public void preencherTabelaStatus(String status) {
    clnNumero.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, Integer>("ordemId"));
    clnCliente.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("pessoaNome"));
    clnServico.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("servicoDescricao"));
    clnVeiculo.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("veiculoModelo"));
    clnChassis.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("veiculoChassis"));
    clnPlaca.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("veiculoPlaca"));
    clnDataEntrada.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("dataEntradaFormatada"));
    clnDataPrometida.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("dataPrometidaFormatada"));

    ObservableList<VisualizacaoOrdem> listaAberto = FXCollections
            .observableArrayList(visualizacaoOrdemDAO.buscarOrdemComStatus(status));
    tblLista.setItems(listaAberto);
}
```

## Facilidade de Desenvolvimento
Apesar dos desafios, alguns pontos tornaram o trabalho mais fluido:
- **JDBC puro** com DAO genérico: didático, fácil de depurar, sem "mágica" de ORM.
- **Padrão DAO replicado** entre entidades: cada nova entidade é só criar a classe + um `*DAO.java` com `salvar`, `buscar`, `atualizar`, `deletar`.
- **JasperReports**: uma vez aprendido o ciclo (`.jasper` compilado + `JasperFillManager.fillReport`), os relatórios saem rápido.

## Relatórios
Implementados com **JasperReports**:
- **Despesas**: lista todas as contas a pagar com filtros por período.
- **Receitas**: lista todos os pagamentos recebidos.
- **DRE** (Demonstrativo de Resultado de Exercício): receita − despesa por período, para o dono ter visão simples de lucro/prejuízo.
- **Relatórios específicos de OS**: orçamento vs.OS, peças por OS, etc.

## Variações
Este projeto originou **diversas derivações** para outros clientes do segmento de oficinas mecânicas. Cada variação mantém a mesma arquitetura base (JavaFX + JDBC + Jasper + MySQL) e o mesmo fluxo principal, mas recebeu adaptações pontuais conforme a necessidade específica de cada cliente — regras de negócio, relatórios extras, ajustes no schema ou no formulário de OS. Para evitar repetição, as variações não são documentadas individualmente; este doc serve como **referência da arquitetura comum** da família.

## Conclusão
Esse sistema foi o **projeto base** da família de sistemas para oficinas. Foi a partir dele que se consolidou o padrão de stack (JavaFX + JDBC + Jasper), a arquitetura em pacotes, a solução do `VisualizacaoOrdem` para a tabela principal e o fluxo de registro de peças sem estoque. Foi aqui também que aprendi a lição mais importante: **conhecer bem o fluxo do cliente vale mais que a tecnologia escolhida**.