# Sistema para Oficinas Mecânicas

## Índice
1. [Introdução](#introdução)
2. [Público Alvo](#público-alvo)
3. [Desenvolvimento](#desenvolvimento)
4. [Desafios](#desafios)
5. [Facilidade de Desenvolvimento](#facilidade-de-desenvolvimento)
6. [Galeria de Imagens](#galeria-de-imagens)
7. [Relatórios](#relatórios)
8. [Conclusão](#conclusão)

## Introdução
Este projeto foi desenvolvido para atender às necessidades de gestão de oficinas mecânicas. O sistema permite a emissão de orçamentos, emissão de ordens de serviço, gestão de clientes, registro de peças, registro de mão de obra, contas a pagar, contas a receber, agendamento e geração de relatórios.

## Público Alvo
O sistema foi projetado principalmente para:
- Oficinas mecânicas
- Auto elétricas
- Lava-jatos
- Outros estabelecimentos do setor automotivo

## Desenvolvimento
O sistema foi desenvolvido como um aplicativo desktop utilizando as seguintes tecnologias:
- **Java 8**: Linguagem de programação principal.
- **JavaFX**: Para a interface gráfica da aplicação.
- **Scene Builder**: Utilizado para criar os protótipos das telas em FXML.
- **MySQL**: Banco de dados para armazenar todas as informações.
- **Jasper Reports**: Para a geração de relatórios do sistema.

### Funcionalidades Principais
- **Registro de Peças Sem Estoque**: A primeira abordagem foi fazer com que a parte da inserção de peças fosse ágil. Dessa forma, é possível fazer o registro sem ter estoque, apenas digitando a descrição, quantidade e valor unitário. Essa abordagem para os clientes mais simples teve uma adesão enorme.
- **Emissão de Ordens de Serviço**: Facilita a criação e gestão de ordens de serviço para os clientes.
- **Gestão de Clientes**: Mantém um registro completo dos clientes, seus veículos, orçamentos, ordens de serviço e pagamentos.
- **Relatórios**: Geração de relatórios detalhados em PDF para análise e tomada de decisões.

## Desafios
Durante o desenvolvimento do sistema, enfrentei diversos desafios, incluindo:
- **Integração de Componentes**: A integração de JavaFX com Scene Builder foi complexa e exigiu bastante tempo de estudo.
- **Gerenciamento de Estado**: Manter o estado da aplicação consistente entre as diversas telas e operações foi desafiador.
- **Desempenho**: Otimizar o desempenho do sistema para que seja produtivo para o usuário.
- **Preenchimento da Tabela Principal**: O maior desafio, que tomou bastante tempo, foi preencher a tabela principal com informações de 3 classes diferentes. Como só podemos apontar para a propriedade da tabela apenas um tipo, foi necessário usar uma classe específica. Após vários dias trabalhando no desenvolvimento, consegui criar uma classe somente para visualização dos dados e uma SQL formatada para preenchimento desses dados. Essa solução resolveu o problema e funcionou bem.

### Exemplo de SQL para Capturar os Dados

```sql
SELECT o.id AS ordem_id, 
       p.id AS pessoa_id,
       p.nome AS pessoa_nome,
       o.servico AS servico_descricao, 
       o.veiculo_id AS veiculo_id, 
       o.pessoa_id AS pessoa_id, 
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

### Exemplo de Java
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

    // Construtor, getters e setters
}

```
### Exemplo de Preenchimento da tabela principal
```java

public void preencherTabela {
            clnNumero.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, Integer>("id"));
			clnCliente.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, Pessoa>("cliente"));
			clnServico.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("servico"));
			clnVeiculo.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("veiculo"));
			clnChassis.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("chassis"));
			clnPlaca.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("placa"));
			clnDataEntrada.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("data_entrada"));
			clnDataPrometida.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, String>("data_prometida"));

            ObservableList<TableViewProperty> listaAberto = FXCollections
								.observableArrayList(buscarOrcamentoAbertoNumero(txtBuscar));
						tblLista.setItems(listaAberto);

}            

```

## Galeria de Imagens

### Tela Login
![Tela Login](images/tela_login.png)
*Figura 1: Tela de Login para acessar a ferramenta*

### Tela Home
![Tela Home](images/tela_home.png)
*Figura 2: Tela Principal do sistema, contendo uma tabela principal, menus e botões de navegação*

### Tela Home 1
![Tela Home 1](images/tela_home_1.png)
*Figura 3: Tela Principal do sistema, na aba lista, onde mostra as principais informações para a primeira visualização*

### Tela Home 2
![Tela Home 2](images/tela_home_2.png)
*Figura 4: Tela Principal do sistema, na aba Peças/Serviços, onde podemos ver as peças que foram registradas, assim como os serviços*

### Tela Home 3
![Tela Home 3](images/tela_home_3.png)
*Figura 5: Tela Principal do sistema, na aba Cliente podemos visualizar suas informações, sem precisar ir no menu de clientes*

### Tela Home 4
![Tela Home 4](images/tela_home_4.png)
*Figura 6: Tela Principal do sistema, na aba Veículo podemos visualizar suas informações que foram cadastradas no sistema*

### Tela Home 5
![Tela Home 5](images/tela_home_5.png)
*Figura 7: Tela Principal do sistema, na aba Fechamento podemos visualizar um resumo geral da ordem de serviço ou orçamento*

### Tela Nova Ordem de Serviço
![Tela Nova Ordem de Serviço](images/tela_nova_ordem_servico.png)
*Figura 8: Tela responsável por criar uma nova ordem de serviço*

### Tela Nova Ordem de Serviço 1
![Tela Nova Ordem de Serviço 1](images/tela_nova_ordem_servico_1.png)
*Figura 9: Tela responsável por criar uma nova ordem de serviço*

### Tela Relatório de Despesa
![Tela Relatório de Despesa](images/tela_relatorio_despesa.png)
*Figura 10: Tela responsável por possuir todos os registros de despesas registradas*

### Tela Relatório de Receita
![Tela Relatório de Receita](images/tela_relatorio_receita.png)
*Figura 11: Tela responsável por possuir todos os registros de pagamentos registrados para receber*

### Link do Video de Demonstração
![Video](https://drive.google.com/drive/folders/1VfpSla1j0AR7JTGxGFApQl8s6yv4AQVo/apresentacao-sistema-ordem-servico.mp4)


## Conclusão 
Conforme informado, todos os desafios e facilidades. Por ser o primeiro serviço e primeiro sistema a desenvolver, consegui ter noção de como seria um processo de forma geral para produzir um sistema. Este foi o primeiro e serve de base para vários outros.