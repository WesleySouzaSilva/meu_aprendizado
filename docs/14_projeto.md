# Sistema para Hotel/Petshop (sistema_hotel_pet)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Aplicação desktop para gestão de um hotel/petshop que oferece hospedagem de pets (para clientes que viajam e precisam deixar seus animais em um local seguro e responsável), banho, tosa e venda de produtos. O sistema cobre o ciclo completo do negócio: cadastro de clientes (pessoa física e jurídica), cadastro de pets e suas categorias, agenda de hospedagem e serviços, controle financeiro com contas a pagar e receber (inclusive parcelamento), emissão de relatórios gerenciais via JasperReports e fluxos auxiliares de usuários e licença.

## Contexto do Projeto
O cliente atua no segmento pet com três frentes de receita que precisam coexistir na mesma operação: hospedagem, banho/tosa e loja de produtos. A hospedagem é o diferencial — cada pet agendado representa um período de entrada e saída, valor da diária, situação (reservado, hospedado, finalizado) e observações próprias — e precisa ser visualizada e conciliada com os serviços eventuais de banho e tosa aplicados durante a estadia. A motivação foi unificar essas frentes em um único sistema, evitando o controle paralelo em planilhas e cadernos que gerava divergências entre agenda, cobrança e estoque de produtos.

A solução foi dimensionada para uso local, em um ou dois pontos do estabelecimento, sem dependência de servidor de aplicação: o banco MySQL fica em uma hospedagem externa e o app é instalado nas máquinas dos usuários. A autenticação inicial é feita por uma tela de login que valida usuário e senha contra a tabela `usuario` antes de liberar o `TelaHome` com o menu principal.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal do projeto.
- **JavaFX + FXML**: camada de apresentação com `TelaLogin.fxml` como bootstrap e `TelaHome.fxml` como hub de navegação; controllers nomeados `Tela*.java` em `br.com.sistema.controll`.
- **JDBC + MySQL (mysql-connector-java 5.x)**: persistência, com string JDBC fixa `jdbc:mysql://localhost:3306/sistema_pet?useTimezone=true&serverTimezone=UTC` instanciada a partir de `Principal.main`.
- **JasperReports (`jasperreports-6.x`)**: relatórios `RelatorioDRE`, `RelatorioDadosAgenda`, `RelatorioListaAgenda`, `RelatorioPagamento`, `RelatorioPagamentoDespesa` e `RelatorioPet`.
- **Apache Ant (`build.xml`)**: build/empacotamento da aplicação desktop (sem Maven/Gradle).
- **PowerShell (`converterUTF8.ps1`)**: utilitário para conversão de encoding de arquivos do projeto.

### Organização do Código
- **`br.com.sistema.conexao`** — `Conexao` JDBC e `AbstractGenericDAO<G>` com operações genéricas `inserir(G)`, `apagar(G)`, `atualizar(G)`, `listarTodos()` e utilitário `ultimoID(String tabela)`, reaproveitados pelos DAOs concretos.
- **`br.com.sistema.controll`** — controllers JavaFX (~30 telas: `TelaLogin`, `TelaHome`, `TelaCadastroClientesPF`, `TelaCadastroClientesPJ`, `TelaEditarAgendaPet`, `TelaEditarCategoriaPet`, `TelaEditarClientePF`, `TelaEditarClientePJ`, `TelaEditarPet`, `TelaEditarServico`, `TelaEditarUsuario`, `TelaFechamentoAgenda`, `TelaListaCategoriaPet`, `TelaListaContasPagar`, `TelaListaContasReceber`, `TelaListaPet`, `TelaNovaCategoriaPet`, `TelaNovoAgendamentoPet`, `TelaNovoAgendaPet`, `TelaNovoClientePet`, `TelaNovoPet`, `TelaNovoServico`, `TelaNovoUsuario`, `TelaPagamentoParcelado`, `TelaPrincipalClientes`, `TelaPrincipalUsuario`, `TelaRelatorioDre`, `TelaVisualizarDisponibilidadeAgenda`).
- **`br.com.sistema.filtros`** — filtros de teclado reaproveitáveis: `Cep`, `Chassis`, `Email`, `Flutuante`, `Fone`, `Inteiro`, `Letras`, `Placa`, que interceptam `KeyEvent.KEY_TYPED` para bloquear caracteres inválidos.
- **`br.com.sistema.icones`**, **`br.com.sistema.listeners`** — recursos de imagem e listeners genéricos (mínimo de caracteres, formatadores de CEP/CNPJ/CPF/Fone/monetário/RG).
- **`br.com.sistema.model`** — entidades de domínio: `Pessoa`, `Pet`, `CategoriaPet`, `Servico`, `Agenda`, `Endereco`, `Cidade`, `Estado`, `Telefone`, `Email`, `ContasPagar`, `ContasReceber`, `DetalhesPagamento`, `Usuario`, `Licenca`.
- **`br.com.sistema.relatorios`** — wrappers Java para os `.jasper` compilados, com parâmetros e JRDataSource populados a partir das listas de domínio.
- **`br.com.sistema.validadores`** — validações reutilizáveis (campos obrigatórios, formatos, regras de negócio).
- **`br.com.sistema.view`** — arquivos FXML e recursos visuais.
- **`br.com.sistema.Principal`** — bootstrap: instancia 18 DAOs (`agendaDAO`, `categoriaPetDAO`, `cidadeDAO`, `contasPagarDAO`, `contasReceberDAO`, `detalhesPagamentoDAO`, `emailDAO`, `enderecoDAO`, `estadoDAO`, `licencaDAO`, `pessoaDAO`, `petDAO`, `servicoDAO`, `telefoneDAO`, `usuarioDAO` etc.), chama `agendaDAO.atualizarTabela()` para sincronização inicial e abre `TelaLogin.fxml`.
- **`sistema_pet.sql`** (170 KB) — script de criação do esquema MySQL com tabelas `agenda, categoria_pet, cidade, contas_pagar, contas_receber, detalhes_pagamento, email, endereco, estado, licenca, pessoa, pet, servico, telefone, usuario`.

## Funcionalidades
- **Login com validação** contra a tabela `usuario` (TelaLogin → TelaHome) e tela principal com menu lateral de navegação por módulo.
- **Cadastro de clientes PF e PJ** com dados pessoais, múltiplos telefones, múltiplos e-mails, endereço vinculado a cidade e estado (cidade e estado mantidos em tabelas próprias para evitar redundância).
- **Cadastro de pets** com nome, raça, peso, observação, dono (Pessoa) e categoria (cachorro, gato, etc.) cadastrável em tela dedicada.
- **Agenda de hospedagem** (`Agenda`) com data/hora de entrada, data/hora de saída, valor, situação (reservado/hospedado/finalizado), observação, tipo de serviço e vínculo com pessoa e pet; consulta de disponibilidade visual em `TelaVisualizarDisponibilidadeAgenda`.
- **Cadastro de serviços** de banho, tosa e outros, com valor e duração, reaproveitados na agenda.
- **Fechamento de agenda** (`TelaFechamentoAgenda`) consolidando o período hospedado e os serviços aplicados, gerando o valor a cobrar.
- **Pagamento parcelado** (`TelaPagamentoParcelado`) gravando em `contas_receber` e `detalhes_pagamento` (uma parcela por registro, com data de vencimento e situação).
- **Contas a pagar e a receber** com listagens dedicadas (`TelaListaContasPagar`, `TelaListaContasReceber`) para controle financeiro do estabelecimento.
- **Relatórios JasperReports**: DRE, dados da agenda, lista da agenda, pagamento (recebimento), pagamento de despesa e listagem de pets — emitidos a partir das telas de relatório correspondentes.
- **Filtros de teclado** (`FiltroInteiro`, `FiltroLetras`, `FiltroCep`, `FiltroFone` etc.) que travam caracteres inválidos em campos específicos, reaproveitados entre todas as telas de cadastro.
- **Listeners de formatação** (CEP, CNPJ, CPF, Fone, monetário, RG) e validador de mínimo de caracteres aplicados nos campos das telas.
- **Cadastro de usuários** com tela própria para perfis e senhas, controlando quem acessa o sistema.
- **Controle de licença** persistido em tabela própria (`licenca`), validado no startup.
- **Conversor UTF-8** (`converterUTF8.ps1`) para normalizar encoding dos arquivos do projeto em rotinas de manutenção.

## Desafios
- **Modelagem de agenda de hospedagem**: representar uma estadia com entrada, saída, valor, situação e múltiplos pets/hospedagens em paralelo exigiu entidade própria (`Agenda`) com chave para pessoa e chave para pet, mais uma tabela para serviços aplicados durante o período. A complexidade foi conciliar o ciclo de vida da hospedagem (reservado → hospedado → finalizado) com a geração automática do valor a cobrar no fechamento, considerando extras de banho, tosa ou produtos consumidos durante a estadia.
- **Centralização do CRUD via DAO genérico**: `AbstractGenericDAO<G>` funciona bem para entidades com chave simples e mesmo padrão de colunas, mas exige disciplina — qualquer entidade com chave composta, FKs múltiplas ou colunas com nomes divergentes do padrão precisa de override, o que leva a DAOs concretos cada vez mais "especiais" ao longo do tempo.
- **Persistência de credenciais JDBC hardcoded**: `Principal.main` instancia a conexão com URL, usuário e senha fixos no código-fonte, o que facilita o bootstrap mas impede deploy em ambiente diferente sem recompilação e expõe credenciais no repositório.
- **Múltiplos pontos de inserção para dados relacionados**: pessoa tem FKs para `telefone`, `email`, `endereco`, `cidade`, `estado`. O cadastro precisa persistir cada tabela filha no momento certo para não quebrar a FK, o que torna a transação manual em vários DAOs vulnerável a inconsistência se uma etapa falhar.
- **Parcelamento modelado como múltiplas linhas em `detalhes_pagamento`**: dá flexibilidade para buscar parcelas por `contas_receber`, mas exige que toda a lógica de UI (mostrar parcelas, marcar paga, estornar) seja feita manualmente em SQL — sem objeto de domínio "parcela" no código Java, apenas `DetalhesPagamento` mapeado 1:1 com a linha da tabela.
- **Relatórios JasperReports parametrizados manualmente**: cada tela de relatório (`TelaRelatorioDre`, `TelaListaAgenda`, etc.) popula `Map<String,Object>` e `JRDataSource` à mão, sem reuso de um helper genérico — o que funciona mas duplica código entre relatórios.
- **Filtros de teclado como `KeyListener`**: filtros baseados em `KeyEvent.KEY_TYPED` ignoram colagem via Ctrl+V e digitação por IME, permitindo entrada inválida por essas vias. A correção exigiria validação em `setText`/commit do campo.

## Aprendizados
- Construção de um sistema de agenda com ciclo de vida próprio (reservado → hospedado → finalizado) que coexiste com serviços eventuais (banho, tosa, produtos) cobrados no fechamento, num modelo único de domínio.
- Generalização do CRUD via `AbstractGenericDAO<G>` reaproveitado por 18 entidades, reduzindo boilerplate de JDBC em operações simples e mostrando os limites do padrão quando entidades fogem do "padrão de colunas esperado".
- Modelagem de contato completo de pessoa (telefones, e-mails, endereço, cidade, estado) com tabelas filhas separadas e FKs, evitando campos concatenados em `pessoa`.
- Persistência de parcelamento via linhas individuais em `detalhes_pagamento`, aprendendo a refletir a granularidade do banco na granularidade da UI sem assumir um objeto "parcela" intermediário.
- Emissão de relatórios JasperReports a partir do JavaFX com parâmetros populados manualmente, sem engine intermediária, e amarrando cada `.jasper` a uma `Tela*` específica.
- Reuso intenso de filtros de teclado e listeners de formatação em todas as telas, garantindo padrão visual de borda, máscara e bloqueio de caracteres inválidos em CEP, CNPJ, CPF, Fone e monetário.
- Estruturação de um projeto desktop grande (~30 telas) com pacotes por papel (`conexao`, `controll`, `filtros`, `model`, `relatorios`, `validadores`, `view`), mostrando que a separação por camada permite navegar no código sem IDE robusta.

## Conclusão
O sistema cumpre o papel de centralizar hotel/petshop, banho/tosa e venda de produtos em um único aplicativo desktop, com agenda de hospedagem controlando o ciclo de vida do pet no estabelecimento, financeiro via contas a pagar/receber e relatórios JasperReports para fechamento gerencial. A principal contribuição foi entregar um modelo de domínio coeso que cobre clientes PF/PJ, pets, serviços, agenda, pagamentos e despesas num único sistema, evitando que o cliente mantivesse três controles paralelos (planilha de hospedagem, caderno de banho/tosa e caixa de loja) que não conversavam entre si.

Pontos que ficariam como evolução: mover credenciais JDBC para `config.properties` externo, introduzir camada de serviço entre controllers e DAOs para encapsular transações compostas (cliente + telefones + e-mails + endereço), generalizar a emissão de relatórios com um helper de parâmetros, e revisar os filtros de teclado para cobrir colagem via clipboard e digitação por IME.