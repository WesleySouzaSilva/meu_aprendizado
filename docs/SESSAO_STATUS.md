# Status da Sessão — Documentação do Portfólio

**Data de fechamento da sessão:** 2026-06-19
**Próxima ação:** retomar a partir do item 11 (docs/15_projeto.md — test-planet-api)

---

## Resumo Executivo

Pipeline de documentação dos repositórios do portfólio Wesley Souza Silva.
Cada projeto vira um arquivo `docs/N_projeto.md` e entra via PR com merge manual.

**Progresso:** 12 de 54 docs/N_projeto.md escritos (22,2%)
**PRs abertas e mergeadas:** 11 (PRs #1 a #11)
**PRs pendentes:** 0
**Projetos pulados:** docs/15_projeto.md (test-planet-api), docs/16_projeto.md (sistema_licitacoes) e docs/17_projeto.md (sw-planet-api) — pulados em 2026-06-19 a pedido do Wesley; são apenas atividade de estudo (sw-planet-api é extensão de estudo do test-planet-api), sem cliente nem produção, não cabem na numeração de portfólio documentado.

---

## Como o fluxo funciona (passo a passo)

1. Wesley pede "pode seguir" → agente escolhe o próximo `N` da fila (ver tabela abaixo).
2. Agente faz **4 perguntas de contexto** via `AskUserQuestion`:
   - Quem foi o cliente
   - Qual a motivação/necessidade
   - Qual o principal desafio técnico
   - Qual o principal aprendizado/destaque
3. Agente **clona o repo** em `/tmp/repos/<repo>` (se ainda não estiver lá).
4. Agente **inspeciona a estrutura** (Java/FXML/DAO/Jasper/etc.) — sem inventar features.
5. Agente cria a branch `MA-<N>/<slug>-projeto` a partir da `main` atualizada.
6. Agente escreve `docs/N_projeto.md` no **template rico** (7 seções):
   - Índice
   - Introdução
   - Contexto do Projeto
   - Desenvolvimento (Stack + Organização do Código)
   - Funcionalidades
   - Desafios
   - Aprendizados
   - Conclusão
7. Agente **commita** com mensagem `feat: N-ésimo projeto — <nome curto>`.
8. Agente **push** da branch e **abre PR** com título `[MA-<N>] feat: ...`.
9. **Wesley revisa e faz merge manual** no GitHub.
10. Após o merge, **Wesley avisa** ("pronto"/"pode seguir"/"merge efetuado").
11. Agente **atualiza README.md direto na `main`** (commit `refactor: inclui <repo> no índice...`, sem PR) adicionando o bullet em "Projetos Documentados".
12. Voltar ao passo 1 com o próximo `N`.

### Regras inegociáveis

- **Sem link** `github.com/WesleySouzaSilva/<repo>` em `.md` ou descrição de PR.
- **Sem nome de cliente** nos `.md` (usar genérico tipo "cliente de funilaria").
- **Sem voz de agente** ("eu fiz", "eu aprendi") — texto impessoal.
- **Sempre perguntar contexto** antes de redigir — não inferir 100% do código.
- **Template rico**, não o simplificado (`Visão Geral / Stack / Funcionalidades / Aprendizados`).
- **PRs não-empilhadas**: abrir uma PR por vez, esperar merge antes da próxima.
- **Atualização do README**: commit direto na `main`, sem PR (padrão confirmado por Wesley).

---

## Padrão de Branch

`MA-<N>/<slug>-projeto` onde `<N>` é o **número sequencial da PR no GitHub** (PR #1 = MA-1, PR #2 = MA-2, ..., PR #8 = MA-8). **Não confundir** com o número do arquivo `docs/N_projeto.md` — eles são independentes.

**Tabela de correspondência PR × doc (valores confirmados):**

| PR | Branch | docs/N_projeto.md | Repo |
|---|---|---|---|
| #1 | MA-1/primeiro-projeto | docs/primeiro_projeto.md | sistema para oficina mecânica (legado) |
| #2 | MA-2/segundo-projeto | docs/9_projeto.md | sistema_oficina |
| #3 | MA-11/api-mbs-projeto | docs/13_projeto.md | api-mbs |
| #4 | MA-4/sistema-revendedora-projeto | docs/6_projeto.md | sistema_revendedora |
| #5 | MA-5/download-api-dropbox-projeto | docs/7_projeto.md | download_api_dropbox |
| #6 | MA-6/sistema-controle-frota-dpaula-projeto | docs/8_projeto.md | sistema_controle_frota_dpaula |
| #7 | MA-7/sistema-funilaria-leandro-projeto | docs/10_projeto.md | sistema_funilaria_leandro |
| #8 | MA-8/sistema-jardinagem-samuel-projeto | docs/11_projeto.md | sistema_jardinagem_samuel |
| #9 | MA-9/controle-pedido-pecas-kugler-projeto | docs/12_projeto.md | controle_pedido_pecas_kugler |
| #10 | MA-10/sistema-hotel-pet-projeto | docs/14_projeto.md | sistema_hotel_pet |

> Obs.: PR #3 usou branch `MA-11/api-mbs-projeto` (não MA-3) — Wesley manteve o padrão antigo. Não tentar renomear.

---

## Já Concluídos (11 docs, 10 PRs mergeadas)

| # | Arquivo | Repo | PR | Commit | Status |
|---|---|---|---|---|---|
| 1 | docs/primeiro_projeto.md | (legado, pré-pipeline) | #1 | (já existia) | merged |
| 2 | docs/segundo_projeto.md | (legado, pré-pipeline, mas não mergeado) | — | — | **não mergeado** |
| 3 | docs/6_projeto.md | sistema_revendedora | #4 | a8e416c | merged |
| 4 | docs/7_projeto.md | download_api_dropbox | #5 | 3a6f79b | merged |
| 5 | docs/8_projeto.md | sistema_controle_frota_dpaula | #6 | c9101cb | merged |
| 6 | docs/9_projeto.md | sistema_oficina | #2 | 62c6c54 | merged |
| 7 | docs/10_projeto.md | sistema_funilaria_leandro | #7 | bd726be | merged |
| 8 | docs/11_projeto.md | sistema_jardinagem_samuel | #8 | cc0b50a | merged |
| 9 | docs/12_projeto.md | controle_pedido_pecas_kugler | #9 | 8407de4 | merged |
| 10 | docs/13_projeto.md | api-mbs | #3 | (na PR #3) | merged |
| 11 | docs/14_projeto.md | sistema_hotel_pet | #10 | ab2ade6 | merged |
| 12 | docs/18_projeto.md | scraping-telefones-url | #11 | 259bdc4 | merged |

> Obs.: `docs/segundo_projeto.md` está no working tree mas não foi mergeado. Tratar como rascunho/preview, não conta como entregue.

---

## Faltam (43 docs)

> Obs.: a numeração dos arquivos `docs/N_projeto.md` segue o plano consolidado — pular números concluídos já entregues. Próximo da fila: `docs/18_projeto.md` (scraping-telefones-url).

| # | Arquivo | Repo | Categoria | Data repo |
|---|---|---|---|---|
| 9 | docs/12_projeto.md | controle_pedido_pecas_kugler | Sistemas Desktop | 2022-02-13 |
| 10 | docs/14_projeto.md | sistema_hotel_pet | Sistemas Desktop | 2022-08-20 |
| 11 | docs/15_projeto.md | test-planet-api | APIs Estudo | 2022-12-06 |
| 12 | docs/16_projeto.md | sistema_licitacoes | Sistemas Desktop | 2022-12-12 |
| 13 | docs/17_projeto.md | sw-planet-api | APIs Estudo | 2023-04-23 |
| 14 | ~~docs/18_projeto.md~~ | ~~scraping-telefones-url~~ | ~~Robôs Scraping~~ | 2023-04-28 | entregue (PR #11) |
| 15 | docs/19_projeto.md | projeto-automacao-bling | Integrações | 2023-05-02 |
| 16 | docs/20_projeto.md | PetronectAutomator | Integrações | 2023-05-02 |
| 17 | docs/21_projeto.md | projeto-automacao-hltv | Integrações | 2023-05-10 |
| 18 | docs/22_projeto.md | project-bank | Robôs Financeiros | 2023-05-13 |
| 19 | docs/23_projeto.md | projeto-automacao-bling-interface | Integrações | 2023-05-16 |
| 20 | docs/24_projeto.md | projeto_robo_binance | Robôs Financeiros | 2023-07-05 |
| 21 | docs/25_projeto.md | projeto-leitura-pdf | Utilitários | 2023-09-06 |
| 22 | docs/26_projeto.md | workidev-api-feature-implementacao_qualidade | APIs Estudo | 2023-10-22 |
| 23 | docs/27_projeto.md | sistema_oficina_teta | Sistemas Desktop | 2023-11-26 |
| 24 | docs/28_projeto.md | sistema_sap_api_robo | Integração SAP | 2024-02-11 |
| 25 | docs/29_projeto.md | robo_scraping_brenfeer | Robôs Scraping | 2024-02-13 |
| 26 | docs/30_projeto.md | sistema_sap_api_interface | Integração SAP | 2024-03-20 |
| 27 | docs/31_projeto.md | sistema_sap_scraping_robo | Integração SAP | 2024-03-25 |
| 28 | docs/32_projeto.md | projeto_robo_operacoes_binance | Robôs Financeiros | 2024-04-14 |
| 29 | docs/33_projeto.md | criptografia_arquivo_sql | Utilitários | 2024-08-27 |
| 30 | docs/34_projeto.md | descriptografia_arquivo_sql | Utilitários | 2024-08-27 |
| 31 | docs/35_projeto.md | robo_scraping_brenfeer_produtos | Robôs Scraping | 2024-09-22 |
| 32 | docs/36_projeto.md | robo_scraping_brenfeer_paginas | Robôs Scraping | 2024-09-24 |
| 33 | docs/37_projeto.md | robo_scraping_brenfeer_geral | Robôs Scraping | 2024-09-28 |
| 34 | docs/38_projeto.md | projeto-scraping-robo-api-parts-description | Robôs Scraping | 2024-10-08 |
| 35 | docs/39_projeto.md | projeto-scraping-robo-api-parts-image | Robôs Scraping | 2024-10-08 |
| 36 | docs/40_projeto.md | projeto-robo-captura-imagens-pdf-parts | Robôs Scraping | 2024-10-10 |
| 37 | docs/41_projeto.md | projeto-robo-editor-imagens-anuncios-parts | Robôs Scraping | 2024-10-10 |
| 38 | docs/42_projeto.md | robo_scraping_ostens_ferramentas | Robôs Scraping | 2024-10-11 |
| 39 | docs/43_projeto.md | projeto-teste-upload-google-drive | Integrações | 2024-10-11 |
| 40 | docs/44_projeto.md | projeto-robo-leitor-planilha-produtos-parts | Robôs Scraping | 2024-10-14 |
| 41 | docs/45_projeto.md | projeto-robo-baixar-planilha-api-parts | Robôs Scraping | 2024-10-15 |
| 42 | docs/46_projeto.md | projeto-gerar-planilha-produtos-parts | Robôs Scraping | 2024-10-15 |
| 43 | docs/47_projeto.md | sistema_revendedora_janeo_loja2 | Sistemas Desktop | 2024-10-21 |
| 44 | docs/48_projeto.md | meu_primeiro_teste | Utilitários | 2025-01-08 |
| 45 | docs/49_projeto.md | sistema_log_master | Utilitários | 2025-02-05 |
| 46 | docs/50_projeto.md | projeto-robo-api-bagy | Integrações | 2025-11-18 |
| 47 | docs/51_projeto.md | sistema_oficina_ezequiel | Sistemas Desktop | 2026-05-04 |
| 48 | docs/52_projeto.md | openclaude | IoT / Open Source | 2026-05-17 |
| 49 | docs/53_projeto.md | sistema_dl_auto_pecas_vendas | Sistemas Desktop | 2026-05-17 |
| 50 | docs/54_projeto.md | sistema_arasuinos | Sistemas Desktop | 2026-05-22 |
| 51 | docs/55_projeto.md | upload_api_dropbox | Integrações | 2026-05-29 |
| 52 | docs/56_projeto.md | divvysport-api | APIs Estudo | 2026-05-30 |

> **Nota sobre numeração:** 3 a 5 (docs/3_projeto.md, docs/4_projeto.md, docs/5_projeto.md) e 12, 17, 19, 20, 21, 22, 23, 24, 25, 26, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 60+ estão na fila. Já temos o 6, 7, 8, 9, 10, 11, 13, 18. Pulados: 15, 16, 17 (estudo). Falta 3, 4, 5 e 12 em diante. A numeração vem do **plano consolidado** em `pipeline-outputs/confirmar-arquitetura-54-projetos.md`.

---

## Repos já clonados em `/tmp/repos/`

- `/tmp/repos/api-mbs/`
- `/tmp/repos/download_api_dropbox/`
- `/tmp/repos/robo_scraping_brenfeer_produtos/`
- `/tmp/repos/sistema_controle_frota_dpaula/`
- `/tmp/repos/sistema_oficina/`
- `/tmp/repos/sistema_revendedora/`
- `/tmp/repos/sistema_funilaria_leandro/`
- `/tmp/repos/sistema_jardinagem_samuel/`
- `/tmp/repos/sistema_hotel_pet/`

Ao retomar, clonar o próximo repo da fila com:
```bash
cd /tmp/repos && git clone https://github.com/WesleySouzaSilva/<repo>.git
```

---

## Memórias Importantes para Relembrar

- `wesley-souza.md` — perfil do Wesley
- `user-wesley-conventions.md` — sem nome de cliente, sem voz de agente, "objetivo e enxuto"
- `feedback-documentacao-portfolio.md` — sempre perguntar contexto antes de escrever
- `feedback-confirmar-arquitetura.md` — confirmar antes de git actions
- `feedback-template-docs-projeto.md` — usar template rico
- `feedback-docs-sem-voz-do-agente.md` — texto impessoal
- `feedback-branch-sequencial-pr.md` — N da branch = número sequencial da PR (#2→MA-2, #3→MA-3...)

---

## Estado Atual do Repositório

- Branch atual: `main`
- Último commit: `d42f8a1 refactor: inclui sistema_jardinagem_samuel no índice de projetos documentados`
- Working tree limpo para `docs/segundo_projeto.md` (não-mergeado, pode ficar) e `pipeline-outputs/` (não commitado).

---

## Comando para Retomar (sugestão)

Ao voltar, Wesley pode simplesmente dizer **"pode seguir"** e o agente pega o próximo da fila (`docs/19_projeto.md` — projeto-automacao-bling), faz as 4 perguntas, e executa o ciclo.
