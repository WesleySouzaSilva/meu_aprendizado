# Atualizador via Dropbox (download_api_dropbox)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Este projeto é um **atualizador automático desktop** construído para distribuir novas versões do sistema da revenda sem necessidade de visita presencial ao cliente. O aplicativo consulta o Dropbox, compara a data de modificação do arquivo local com a do servidor e, quando há versão mais recente na nuvem, baixa o arquivo de forma transparente para o usuário.

## Contexto do Projeto
O sistema da revenda roda em máquinas desktops do cliente, e cada atualização exigia, até então, deslocamento até o local para substituir manualmente o `.jar` da aplicação. A motivação foi **distribuir updates sem ir presencialmente** até o cliente, usando o Dropbox como repositório central de versões — uma pasta por cliente na nuvem, com o instalador/jar sempre atualizado.

A solução precisava ser leve, autônoma (sem dependência de servidor de aplicação), e fácil de configurar para o cliente final. O atualizador lê um arquivo `config.properties` (com o caminho do XML de configuração) e um `atualizador.xml` (com os dados do cliente e o arquivo a ser baixado), consulta o Dropbox via SDK oficial e substitui o arquivo local quando detecta versão mais recente.

## Desenvolvimento

### Stack Técnica
- **Java 8**: linguagem principal.
- **Swing (JDialog, JOptionPane, JProgressBar)**: interface gráfica leve do atualizador.
- **Dropbox Core SDK 3.1.3 / 7.0.0**: integração com a API v2 do Dropbox (`DbxClientV2`, `files().getTemporaryLink`, `files().downloadBuilder`).
- **Jackson Core 2.7.4**: dependência transitiva usada pelo SDK do Dropbox.
- **Java XML (javax.xml.parsers + org.w3c.dom)**: leitura do `atualizador.xml` via `DocumentBuilderFactory`.
- **Java Properties**: leitura do `config.properties`.
- **SwingWorker**: execução do download em thread separada, sem travar a UI.

### Organização do Código
- **`controll/Principal.java`**: ponto de entrada (`main`). Orquestra todo o fluxo: inicializa o cliente Dropbox, carrega configurações, dispara a verificação e gerencia o diálogo de download.
- **`model/ClienteXml.java`**: POJO que armazena os dados lidos do XML (`caminhoArquivo`, `nomeArquivo`, `nomeCliente`, `caminhoAtualizadorXml`).
- **`model/LeitorXml.java`**: utilitário genérico que recebe uma lista de tags e devolve os valores extraídos do XML, em grupos.

### Fluxo do Atualizador
1. Carrega `config.properties` (chave `caminhoXml`).
2. Carrega `atualizador.xml` e extrai `caminhoArquivo`, `nomeArquivo`, `nomeCliente`.
3. Inicializa o `DbxClientV2` com o token de acesso.
4. Verifica conectividade com a internet.
5. Compara `lastModified()` do arquivo local com `clientModified` do Dropbox.
6. Se houver versão mais recente, exibe diálogo de atualização com `JProgressBar` indeterminada.
7. `SwingWorker` faz o download via `downloadBuilder().download(out)` e substitui o arquivo local.

## Funcionalidades
- **Verificação automática de versão**: compara a data de modificação local com a do Dropbox a cada execução.
- **Download com interface modal**: barra de progresso indeterminada durante o download, com janela modal que impede interação até a conclusão.
- **Configuração por arquivos externos**: `config.properties` (caminho do XML) + `atualizador.xml` (dados do cliente). O mesmo atualizador serve para qualquer cliente do portfólio, bastando ajustar os arquivos de configuração.
- **Validações e mensagens claras**: diálogo de erro para `config.properties` ausente, chave `caminhoXml` faltando, `atualizador.xml` não encontrado, sem conexão com a internet, ou arquivo da aplicação ausente.
- **Path por cliente**: estrutura `/ATUALIZADOR CLIENTES/<nomeCliente>/<nomeArquivo>` no Dropbox, com um diretório separado para cada cliente.

## Desafios
- **Swing + SwingWorker para download**: o maior desafio foi fazer o download da nova versão sem travar a interface gráfica. A solução foi usar `SwingWorker<Void, Void>` para mover o trabalho pesado para uma thread separada, mantendo o `JDialog` modal com `JProgressBar` indeterminada (`setIndeterminate(true)`) e tratando exceções em `done()` para exibir mensagens de sucesso ou erro.
- **Validação em camadas**: garantir uma sequência correta de validações (config → XML → conexão → arquivo local → Dropbox) para falhar rápido com mensagens úteis ao usuário, em vez de erros genéricos de baixo nível.
- **Comparação de datas confiável**: usar `metadata.getClientModified()` do Dropbox (não o horário do servidor) como referência para decidir se o download é necessário.

## Aprendizados
- **Reuso: app desktop + atualizador**: o padrão "sistema desktop + atualizador via Dropbox" passou a ser replicado em outros sistemas do portfólio que precisavam de atualizações remotas sem infraestrutura de servidor. Bastava trocar o `atualizador.xml` para atender clientes diferentes.
- **Integração com API externa de nuvem**: foi a primeira integração prática com uma API de armazenamento em nuvem usando SDK oficial (Dropbox Core SDK).
- **Threads em Swing**: uso de `SwingWorker` para isolar trabalho de I/O em background, com tratamento de exceções em `done()` e atualização segura da UI na thread de eventos.
- **Configuração externa ao código**: separar parâmetros sensíveis e dados do cliente em arquivos de configuração (`config.properties` + `atualizador.xml`) facilita replicar o mesmo executável para vários clientes.

## Conclusão
O `download_api_dropbox` foi o **atualizador padrão** dos sistemas desktop do portfólio, usado para distribuir novas versões sem deslocamento até o cliente. O padrão Swing + SwingWorker + Dropbox SDK + configuração via XML/properties foi aplicado em outros sistemas da família, consolidando a estratégia de "atualização por Dropbox" como solução leve para deploy em sistemas desktop legados.
