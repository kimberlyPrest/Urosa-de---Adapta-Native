# Matriz SPECs x Fases — Fase 1

**Projeto:** Urosaude & Associados — sistema financeiro operacional
**Fonte:** `03-Projeto/02-Escopo-Definitivo.md` (Fase 1) e `matriz-de-rastreabilidade.md` (RQ-01..RQ-03, RQ-10 parcial)
**Regra:** toda SPEC declara fase, requisito de origem, criterios de aceite e prova; toda task nasce vinculada a uma SPEC.

| Fase | Requisito | SPEC | Criterios de aceite | Prova (TDD) | Tasks |
|---|---|---|---|---|---|
| 1 | RQ-01, RQ-02 | SPEC-1-001 — Base financeira e perfis iniciais | CA-1-001, CA-1-002, CA-1-003 | GREEN: cadastros aparecem no caixa; REFACTOR: duplicidade bloqueada e item usado inativado | T1.1, T1.2, T1.3 |
| 1 | RQ-02 | SPEC-1-002 — Caixa diario com anexos e divergencias | CA-1-004, CA-1-005, CA-1-006 | GREEN: fechamento com totais por forma; REFACTOR: divergencia sem justificativa bloqueia e reabertura com motivo | T1.4, T1.5, T1.6, T1.7 |
| 1 | RQ-03, RQ-10 (parcial) | SPEC-1-003 — Importacao inicial ProDoctor e dashboard | CA-1-007, CA-1-008, CA-1-009 | GREEN: lote importado e dashboard atualizado; REFACTOR: cabecalho invalido e lote duplicado tratados | T1.8, T1.9, T1.10 |
| 1 | RQ-01 (integracao) | SPEC-1-004 — Validacao integrada da Fase 1 | CA-1-010 (cobre CA-1-001..CA-1-009) | GREEN + REFACTOR/REGRESSAO com dossie e aceite humano | T1.11 |

## Tasks — projeção canônica da matriz

A tabela abaixo repete os mesmos campos da tabela operacional e das seções `## Tasks vinculadas` das SPECs.

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T1.1 | Implementar login e perfis iniciais (Direcao leitura; Gestao Financeira cria/edita e revisa; Recepcao opera caixa) com permissoes minimas da RN-03 | Ethos | SPEC-1-001 | Cada perfil loga e ve somente o que a RN-03 define; Recepcao nao acessa configuracoes criticas | GREEN da SPEC-1-001 (parte perfis) + caminho de erro da RN-03 | Captura do login dos 3 perfis e do menu visivel de cada um | Projeto Skip existente (satisfeita) | a fazer |
| T1.2 | Criar CRUD dos cadastros financeiros essenciais (contas, categorias, formas de pagamento, medicos, convenios, centros/areas) com obrigatorios e bloqueio de duplicidade ativa | Ethos | SPEC-1-001 | CA-1-001: Gestao Financeira cria e edita os 6 tipos de cadastro; duplicidade ativa bloqueada | GREEN da SPEC-1-001 (parte cadastros) + fixture (Caixa Recepcao, Dinheiro, Cartao, Consulta, Particular, Dr. Edson) | Captura das telas com registros salvos e erro de duplicidade | T1.1 concluida | a fazer |
| T1.3 | Implementar inativacao (RN-01) e visibilidade somente de ativos em novos lancamentos (RN-02) | Ethos | SPEC-1-001 | CA-1-003: registro usado em lancamento nao e apagado, fica inativo; inativo some de novos lancamentos | REFACTOR/REGRESSAO da SPEC-1-001 | Captura/log da exclusao bloqueada e do item inativo oculto no caixa | T1.2 concluida | a fazer |
| T1.4 | Criar modelo do caixa diario com abertura/fechamento por data e estados aberto/fechado/reaberto, sem duplicar fechamento da mesma data/caixa | Ethos | SPEC-1-002 | Caixa do dia abre e fecha com status rastreavel; segundo fechamento da mesma data bloqueado | GREEN da SPEC-1-002 (parte estados) + idempotencia da tabela de caixa | Captura do caixa aberto/fechado e do erro de duplicidade | T1.1 concluida | a fazer |
| T1.5 | Implementar lancamentos de caixa por forma de pagamento/categoria com campos obrigatorios, valor > 0 (RN-06) e anexo com fallback pendente | Ethos | SPEC-1-002 | Lancamento valido salva; valor <= 0 bloqueia; anexo ausente marca pendente e permite anexar depois | GREEN da SPEC-1-002 (parte lancamentos) + caminhos de erro obrigatorios | Captura dos lancamentos 600/200/180 e da tentativa de valor invalido | T1.4 e T1.2 concluidas | a fazer |
| T1.6 | Implementar divergencias com justificativa obrigatoria (RN-05) bloqueando fechamento sem justificativa | Ethos | SPEC-1-002 | CA-1-005: caixa com divergencia nao fecha sem justificativa | REFACTOR/REGRESSAO da SPEC-1-002 (parte divergencia) | Captura do bloqueio e do fechamento apos justificar | T1.5 concluida | a fazer |
| T1.7 | Implementar resumo do dia por forma de pagamento no fechamento e reabertura pela Gestao Financeira com motivo rastreavel | Ethos | SPEC-1-002 | CA-1-004: totais corretos com ao menos 3 formas; CA-1-006: reabertura com motivo registrada | GREEN (totais) + REFACTOR (reabertura) da SPEC-1-002 | Captura do resumo do caixa fechado e do historico de reabertura | T1.6 concluida | a fazer |
| T1.8 | Implementar upload + validacao de cabecalho + parser CSV sanitizado ProDoctor com normalizacao monetaria (RN-07) e datas (RN-08), separando linhas validas de invalidas e quarentenando identificadores diretos | Ethos | SPEC-1-003 | CA-1-007: fixture CSV sintetica importa sem digitacao; linha invalida nao quebra o lote; coluna paciente/prontuario nao e persistida | GREEN da SPEC-1-003 (parser) + prova negativa RN-10 | Log sanitizado de importacao + uma linha invalida rejeitada + prova negativa sem valor PII | T1.1 concluida (usuario Gestao Financeira para upload) | a fazer |
| T1.9 | Implementar armazenamento do lote com identidade (hash/nome/data) para duplicidade (RN-09), relatorio de linhas rejeitadas e remocao de lote de teste | Ethos | SPEC-1-003 | CA-1-008: linhas com erro em relatorio separado; reenvio do mesmo lote bloqueado ou confirmado | REFACTOR/REGRESSAO da SPEC-1-003 | Captura do relatorio de erros e da tentativa de lote duplicado | T1.8 concluida | a fazer |
| T1.10 | Implementar dashboard da Fase 1 com total faturado, quantidade de procedimentos e filtros por data/convenio/medico, sem PII por padrao (RN-10) | Ethos | SPEC-1-003 | CA-1-009: dashboard mostra totais e filtra por data/convenio/medico | GREEN da SPEC-1-003 (parte dashboard) | Captura do dashboard com dados sinteticos e verificacao de ausencia de PII | T1.8 concluida | a fazer |
| T1.11 | Executar prova integrada da Fase 1: logar, cadastrar, abrir caixa, lancar, divergir, fechar, importar CSV e conferir dashboard, coletando evidencias dos CA-1-001..009 | Ethos | SPEC-1-004 | CA-1-010: 9/9 criterios funcionais com evidencia e veredito humano registrado | GREEN + REFACTOR/REGRESSAO da SPEC-1-004 | Dossie de capturas/logs + checklist CA a CA + aceite ou reprovacao | T1.3, T1.7, T1.9 e T1.10 concluidas | a fazer |

## Cobertura

- 4 SPECs (3 funcionais + 1 de validacao), 10 criterios de aceite (CA-1-001..CA-1-010), 11 tasks em 6 levas (A-F).
- Nenhum criterio de aceite sem task dona; nenhuma task sem SPEC de origem.
- Os campos de task (ID, dono, SPEC, criterio, recorte da prova, evidencia, pre-condicoes e status) sao identicos nas tres projecoes canonicas.
- Fases 2-5 permanecem sem SPECs nesta matriz (onda: SPECs da fase N+1 sao geradas durante a fase N via liberar-fase).

## Fora desta fase (confirmado no escopo)

Conciliacao bancaria automatizada, DRE final, repasses completos, contas a pagar, recorrencias, loops/agentes e estoque.
