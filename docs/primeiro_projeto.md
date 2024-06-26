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

### Exemplo da Classe Java Preenchimento da Tabela Principal
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

public void preencherTabelaStatus (String status) {
            clnNumero.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, Integer>("ordemId"));
			clnCliente.setCellValueFactory(new PropertyValueFactory<VisualizacaoOrdem, Pessoa>("pessoaNome"));
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

## Galeria de Imagens

### Tela Login
![Tela Login](/imagens/tela_login.PNG)

*Figura 1: Tela de Login para acessar a ferramenta*

### Tela Home
![Tela Home](/imagens/tela_home.PNG)
*Figura 2: Tela Principal do sistema, contendo uma tabela principal, menus e botões de navegação*

### Tela Home 1
![Tela Home 1](/imagens/tela_home_1.PNG)
*Figura 3: Tela Principal do sistema, na aba lista, onde mostra as principais informações para a primeira visualização*

### Tela Home 2
![Tela Home 2](/imagens/tela_home_2.PNG)
*Figura 4: Tela Principal do sistema, na aba Peças/Serviços, onde podemos ver as peças que foram registradas, assim como os serviços*

### Tela Home 3
![Tela Home 3](/imagens/tela_home_3.PNG)
*Figura 5: Tela Principal do sistema, na aba Cliente podemos visualizar suas informações, sem precisar ir no menu de clientes*

### Tela Home 4
![Tela Home 4](/imagens/tela_home_4.PNG)
*Figura 6: Tela Principal do sistema, na aba Veículo podemos visualizar suas informações que foram cadastradas no sistema*

### Tela Home 5
![Tela Home 5](/imagens/tela_home_5.PNG)
*Figura 7: Tela Principal do sistema, na aba Fechamento podemos visualizar um resumo geral da ordem de serviço ou orçamento*

### Tela Nova Ordem de Serviço
![Tela Nova Ordem de Serviço](/imagens/tela_nova_ordem_servico.PNG)

*Figura 8: Tela responsável por criar uma nova ordem de serviço*

### Tela Nova Ordem de Serviço 1
![Tela Nova Ordem de Serviço 1](/imagens/tela_nova_ordem_servico_1.PNG)

*Figura 9: Tela responsável por criar uma nova ordem de serviço*

### Tela Relatório de Despesa
![Tela Relatório de Despesa](/imagens/tela_relatorio_despesa.PNG)

*Figura 10: Tela responsável por possuir todos os registros de despesas registradas*

### Tela Relatório de Receita
![Tela Relatório de Receita](/imagens/tela_relatorio_receita.PNG)
*Figura 11: Tela responsável por possuir todos os registros de pagamentos registrados para receber*

### Link do Video de Demonstração
[![Assistir ao Vídeo de demonstração do Sistema](https://drive.google.com/uc?export=view&id=1x_GjdwsO35NOKbygtu7FJbyxpoLVbL0a)](https://drive.google.com/file/d/1x_GjdwsO35NOKbygtu7FJbyxpoLVbL0a/view?usp=sharing "Clique aqui para assistir ao vídeo")


## Conclusão 
Conforme informado, todos os desafios e facilidades. Por ser o primeiro serviço e primeiro sistema a desenvolver, consegui ter noção de como seria um processo de forma geral para produzir um sistema. Este foi o primeiro e serve de base para vários outros.
