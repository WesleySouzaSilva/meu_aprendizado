# Robô de Coleta de Telefones via URL (scraping-telefones-url)

## Índice
1. [Introdução](#introdução)
2. [Contexto do Projeto](#contexto-do-projeto)
3. [Desenvolvimento](#desenvolvimento)
4. [Funcionalidades](#funcionalidades)
5. [Desafios](#desafios)
6. [Aprendizados](#aprendizados)
7. [Conclusão](#conclusão)

## Introdução
Robô de web scraping em Python cuja finalidade é apoiar a área de marketing de um cliente na etapa de prospecção comercial. A partir de uma lista de URLs informadas em arquivo de texto, o script acessa cada página, identifica os blocos com dados de lojas/empresas e extrai três campos — nome, endereço e telefone — gravando o resultado consolidado em um único arquivo `.txt`. A solução foi desenhada como peça auxiliar dentro de um fluxo maior: o `.txt` gerado serve como insumo para leitura ou manipulação posterior pelo software principal do cliente, sem persistência própria.

## Contexto do Projeto
O cliente atua com foco em marketing e prospecta empresas para oferta de serviços. A prospecção depende de bases atualizadas com nome, telefone e endereço das empresas-alvo, e alimentá-las manualmente a partir de sites de guia era moroso e sujeito a erro de cópia. A motivação foi criar uma automação enxuta que lesse uma lista de URLs, raspasse os dados de contato e entregasse um arquivo pronto para ser consumido pela ferramenta principal do cliente.

A integração com o sistema do cliente é feita por um arquivo `.txt` simples: ele contém, para cada loja encontrada, a URL de origem, o nome, o endereço e o telefone, separados por linha e com blocos em branco entre registros. Não há banco de dados, API ou interface gráfica no escopo deste robô — apenas a coleta e a escrita. Esse recorte mantém o robô pequeno, fácil de rodar em uma máquina local e fácil de reapontar para qualquer site que siga a mesma estrutura HTML alvo.

## Desenvolvimento

### Stack Técnica
- **Python 3**: linguagem do script.
- **`requests`**: cliente HTTP para baixar o HTML de cada URL.
- **`BeautifulSoup` (`bs4`)**: parser HTML para localizar os blocos de loja e extrair os campos de nome, endereço e telefone.
- **Biblioteca padrão `os`**: composição de caminhos de arquivo portáveis (`os.path.join`).

### Organização do Código
- **`coleta numeros de URLs.py`** — script único, com três blocos:
  1. **Leitura da lista de URLs** a partir de `dados_url_site.txt` (uma URL por linha), com `strip()` para remover `\n` e espaços.
  2. **Loop principal** que, para cada URL, faz `requests.get(url)`, parseia o HTML com `BeautifulSoup(response.text, 'html.parser')` e procura os blocos `<article itemprop="itemListElement">` que representam cada loja na página.
  3. **Extração dos campos** com `find` aninhado — `h2[itemprop="name"]` para nome, `<address>` para endereço e `<p class="phone">` para telefone —, retornando o texto via `get_text().strip()` ou uma marcação padrão (`'Nome não encontrado'`, `'Endereço não encontrado'`, `'Telefone não encontrado'`) quando o elemento está ausente.
- **`dados_url_site.txt`** — entrada: lista de URLs (uma por linha) que será percorrida pelo robô.
- **`lojas_com_telefones.txt`** — saída: arquivo único no qual o robô anexa, a cada loja encontrada, um bloco `URL:`, `Nome da loja:`, `Endereço:`, `Telefone:` com linha em branco entre registros; encoding UTF-8 para preservar acentuação.

## Funcionalidades
- **Coleta em lote a partir de arquivo de URLs** — o robô lê `dados_url_site.txt` linha a linha e processa cada URL sequencialmente.
- **Requisição HTTP e parseamento do HTML** com `requests` + `BeautifulSoup`, identificando blocos de loja pelo padrão `<article itemprop="itemListElement">` da página-alvo.
- **Extração de nome** via `h2[itemprop="name"]`, com fallback textual `'Nome não encontrado'` caso o elemento não exista.
- **Extração de endereço** via `<address>`, com fallback `'Endereço não encontrado'`.
- **Extração de telefone** via `<p class="phone">`, com fallback `'Telefone não encontrado'`.
- **Saída consolidada em `.txt` único** (`lojas_com_telefones.txt`), no qual cada bloco contém URL de origem + nome + endereço + telefone, separados por linha em branco, gravado em modo `append` com `encoding='utf-8'`.
- **Impressão da URL em tempo de execução** via `print('url : ' + linha)`, útil para acompanhar o progresso do lote no console.
- **Tolerância a campos ausentes** — quando a estrutura HTML não traz algum dos campos esperados, o robô segue gravando o registro com a marcação `'X não encontrado'`, em vez de quebrar o loop.

## Desafios
- **Primeiro contato com scraping em Python**: a ausência de experiência anterior com `requests`, `BeautifulSoup` e parsing HTML exigiu entender o ciclo `request → parse → find → get_text`, lidar com `try/except` implícito na ausência de elementos e aprender a iterar blocos em vez de buscar campos isolados.
- **Acoplamento à estrutura HTML da página-alvo**: o robô assume que cada loja está em um `<article itemprop="itemListElement">`, com nome em `h2[itemprop="name"]`, endereço em `<address>` e telefone em `<p class="phone">`. Qualquer mudança de markup no site (renomeação de classe, troca de tag, novo wrapper) exige revisão dos seletores.
- **Ausência de normalização dos dados extraídos**: o texto retornado por `get_text()` reflete literalmente o que está no HTML — espaços extras, quebras de linha e variações de formatação de telefone chegam como estão ao `.txt`, sem padronização posterior (máscara, DDI, separação em DDD/número etc.).
- **Sem retentativa nem controle de erro de rede**: se `requests.get(url)` falhar por timeout, DNS ou status HTTP não-2xx, o script quebra a iteração. Não há `try/except`, retry exponencial nem diferenciação entre erro transitório e erro permanente.
- **Robô bloqueável por proteções anti-bot**: sem `User-Agent` customizado, sem delay entre requisições e sem suporte a JavaScript, o script não passa de páginas com renderização client-side, Cloudflare ou listas de bloqueio por ausência de headers de navegador.
- **Saída `.txt` como única interface**: como o cliente consome o arquivo diretamente em seu software principal, qualquer ruído no conteúdo (encoding inconsistente, linhas em branco duplicadas, marcações `'X não encontrado'`) chega ao sistema downstream — o robô não filtra nem separa sucesso de falha.
- **Caminho de saída e de entrada fixos no código**: `dir_path = 'C:/Projetos Python/Scripts/'` e o `open('C:\Projetos Python\Scripts\dados_url_site.txt')` estão hardcoded, o que exige edição manual do script para mudar de máquina, diretório ou arquivo de entrada.

## Aprendizados
- Construção do primeiro pipeline de web scraping em Python, integrando `requests` para a requisição HTTP e `BeautifulSoup` para a navegação no DOM, e aprendendo o ciclo `response.text → soup → find_all → find → get_text`.
- Leitura de arquivo de entrada em Python com `open(...)` + `for linha in file`, compreendendo o consumo lazy linha-a-linha e a importância do `strip()` para limpar `\n` antes de processar cada URL.
- Reuso de `os.path.join` para composição de caminhos portáveis no lugar de concatenar strings com separador manual, evitando quebras ao mudar de sistema operacional.
- Padrão de **fallback textual** (`'Nome não encontrado'`, etc.) quando o `find` retorna `None`, garantindo que a iteração não quebra e o registro chega ao `.txt` com marcação explícita do que faltou — útil para identificar páginas fora do padrão na revisão posterior.
- Escrita em modo append com `encoding='utf-8'` para preservar acentuação de nomes e endereços brasileiros, evitando caracteres corrompidos (`Ã©` no lugar de `é`) quando o `.txt` é aberto por outro software.
- Mentalidade de **robô como alimentador de outro sistema**: o `.txt` é a "interface" com o software do cliente, e a disciplina de manter o formato simples e previsível vale mais do que adicionar uma camada de persistência própria.
- Noção dos limites do `requests + BeautifulSoup` (páginas estáticas, sem JS) como ponto de partida para evoluções posteriores que exigiriam Selenium WebDriver ou Playwright.

## Conclusão
O robô cumpre o papel de coletar nome, endereço e telefone de empresas a partir de uma lista de URLs e consolidar tudo em um arquivo `.txt` consumido pelo software principal do cliente de marketing, automatizando uma etapa manual de prospecção que antes dependia de cópia a partir de páginas de guia. A principal contribuição foi entregar uma automação enxuta, com escopo bem definido (ler URLs → raspar → escrever), que se integra ao fluxo maior sem acrescentar complexidade de banco, API ou UI.

Pontos que ficariam como evolução: parametrizar diretório de entrada/saída via `argparse` ou `config.json`, adicionar `User-Agent` e delays configuráveis para reduzir bloqueios, registrar logs estruturados de falhas de rede em vez de quebrar o loop, normalizar telefones e endereços antes de gravar, e evoluir para Selenium WebDriver ou Playwright quando o site-alvo passar a depender de JavaScript para renderizar o conteúdo.
