# Controle de Pedidos de Peças — Autopeças (controle_pedido_pecas_kugler)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Aplicação desktop para uma loja/oficina de autopeças, com o objetivo de substituir o controle manual e em planilhas dos pedidos de peças. A partir de uma planilha Excel exportada pelo sistema de retaguarda, o aplicativo carrega as requisições, permite filtrar peças com estoque abaixo de uma quantidade mínima informada e emite um relatório de saída de peças pronto para impressão.

## Contexto do Projeto
A operação de autopeças recebia requisições de peças que ficavam espalhadas em planilhas e anotações, sem visão consolidada e sem um critério claro para identificar o que precisava ser comprado/reposto. A motivação foi centralizar as requisições em uma única tela, aplicar um filtro por quantidade mínima em estoque e gerar um relatório pronto para o comprador usar como lista de pedido.

O sistema foi dimensionado para uso local, em um ou dois pontos da loja/oficina, sem dependência de servidor de aplicação: o banco MySQL fica em hospedagem externa e o app é instalado nas máquinas dos usuários.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal do projeto.
- **JavaFX + FXML**: camada de apresentação com `TelaPrincipal.fxml` e controller `TelaPrincipal.java`.
- **Apache POI**: leitura da planilha Excel (`.xls`/`.xlsx`) exportada do sistema de retaguarda.
- **MySQL + JDBC (mysql-connector-java)**: persistência, com classe `Conexao` parametrizada por banco/usuário/senha e `useTimezone=true&serverTimezone=UTC`.
- **Impressão via `java.awt.print.PrinterJob`**: emissão do relatório em impressora local, sem JasperReports no fluxo real apesar das libs estarem empacotadas em `lib/`.

### Organização do Código
- **`sistema/controll/`** — `MetodoPrincipal` (bootstrap da aplicação), `TelaPrincipal` (controller da tela única), `ValidationFields` (validação genérica de campos obrigatórios com tooltip vermelho), `TextFieldFormatter` (máscaras de campo usando `MaskFormatter`), `filtros/FiltroInteiro` (bloqueia não-dígitos em `KeyEvent.KEY_TYPED`).
- **`sistema/model/`** — `Relatorio` (entidade de domínio da requisição/peça: código, fornecedor, operação, documento, data, quantidade, quantidadeEstoque, responsável), `Conexao` (JDBC).
- **`sistema/view/`** — `TelaPrincipal.fxml` com duas `TableView` lado a lado (requisições importadas e pedidos filtrados por quantidade mínima), campo `txtQtdeEstoque` e botões *Importar Arquivo*, *Atualizar* e *Imprimir*.
- **`sistema/imagens/`** — ícones da interface.
- **`lib/`** — jars do Apache POI, JasperReports (mantidos mas não usados no fluxo principal), MySQL Connector, Jackson, Groovy e Castor.
- **`bin/`** — saída de compilação do Eclipse (projeto configurado como Eclipse Java Project via `.classpath` e `.project`).

## Funcionalidades
- **Importação de planilha Excel** com a relação de requisições/peças via `FileChooser`, lendo colunas: código, fornecedor, operação, documento, data/hora, quantidade, quantidade em estoque e responsável.
- **Conversão de data serializada do Excel** para `dd/MM/yyyy HH:mm:ss` por fórmula `(valor - 25569) * 86400000`.
- **Listagem completa** das requisições importadas em `tabelaRequisicao`.
- **Filtro por quantidade mínima em estoque** informado pelo usuário: aplica `quantidadeEstoque <= qtdeMinima` e mostra apenas as peças que precisam de reposição em `tabelaPedidos`.
- **Validação de campos obrigatórios** (`ValidationFields`) com borda vermelha e tooltip em TextField/ComboBox/TextArea.
- **Filtro de teclado** (`FiltroInteiro`) para o campo de quantidade mínima aceitar só números.
- **Máscaras de entrada** (`TextFieldFormatter`) reaproveitáveis para CPF, CNPJ, datas etc.
- **Impressão de relatório** "Relatório de Saída de Peças" via `PrinterJob`, listando Código, Fornecedor, Operação, Documento, Data de Requisição, Qtde e Qtde Estoque apenas das peças filtradas.
- **Conexão JDBC** configurável por banco, usuário e senha.

## Desafios
- **Modelagem do "pedido"**: o sistema trata o pedido como o subconjunto de requisições filtradas por quantidade mínima em estoque, não como uma entidade persistida com ciclo de vida. Isso simplifica o app (não há CRUD de pedido, basta atualizar o Excel e reimportar), mas exige que o operador compreenda que a "lista de pedido" é uma visão derivada dos dados importados.
- **Regras de borda**: validação genérica para TextField, ComboBox e TextArea coexistir sem repetir lógica — centralizada em `ValidationFields.checkEmptyFields(...)` com listener que limpa borda/tooltip ao digitar.
- **Máscara de campo reaproveitável**: `TextFieldFormatter` usa `MaskFormatter` do Swing dentro de um `TextField` do JavaFX, o que cria um acoplamento incomum entre APIs (a estratégia foi limitar caracteres válidos e remover o último caractere quando a máscara falha até o texto casar).
- **Conversão de data Excel → Java**: a planilha exportada chega com datas em serial numérico; a fórmula `(valor - 25569) * 86400000` foi aplicada inline no controller, sem extrair para utilitário.
- **Impressão manual**: a versão atual desenha o relatório direto na `Graphics` do `PrinterJob`, controlando fonte, posição X/Y e espaçamento manualmente — o que evita dependência de JasperReports mas entrega um relatório de uma página só.

## Aprendizados
- Aprofundamento em **modelagem de domínio leve**: traduzir "lista de pedido" como filtro em memória sobre uma coleção importada, em vez de criar uma entidade `Pedido` persistida.
- Prática com **JavaFX + FXML** num cenário de tela única com dois `TableView` populados a partir de `PropertyValueFactory<>` apontando para os getters de `Relatorio`.
- Integração de **Apache POI** em fluxo desktop para ler planilha gerada por outro sistema e alimentar a UI sem persistência intermediária.
- Reuso de utilitários Swing (`MaskFormatter`) dentro de um projeto JavaFX, lidando com as peculiaridades de `TextField.setText` + `forward()`.
- Construção de **relatório de impressão** usando a API `java.awt.print` em uma única página, sem engine de relatórios.

## Conclusão
O sistema cumpre o papel de consolidar requisições de autopeças e gerar uma lista de pedido baseada em estoque mínimo, em um aplicativo desktop leve, com dependências externas (planilha) e persistência via MySQL quando necessário. A principal contribuição foi manter o app enxuto: tela única, sem CRUD de pedido, com o "pedido" emergindo como filtro sobre os dados importados — adequado para o porte da operação.

Pontos que ficariam como evolução: extrair a conversão de data do Excel para utilitário, mover a impressão manual para JasperReports (já presente em `lib/`), e persistir histórico de pedidos gerados para consulta posterior.