# Fase 1 — Base financeira e caixa diario

**Resultado:** a clinica registra caixa diario e importa uma fixture CSV sanitizada do ProDoctor em um fluxo demonstravel, com dashboard simples.

**SPECs canonicas:** SPEC-1-001, SPEC-1-002, SPEC-1-003 e SPEC-1-004.

**Fora desta fase:** conciliacao bancaria automatizada, DRE final, repasses completos, contas a pagar, recorrencias, loops/agentes e estoque.

## Tasks

| ID | Task | Dono | SPEC | Criterio | Recorte da prova | Evidencia esperada | Pre-condicoes | Status |
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

## Levas de execucao

| Leva | Tasks | Observacao |
|---|---|---|
| A | T1.1 | Login e perfis iniciais. |
| B | T1.2, T1.4 | Cadastros financeiros e modelo do caixa. |
| C | T1.3, T1.5, T1.8 | Inativacao, lancamentos e parser CSV sanitizado. |
| D | T1.6, T1.9, T1.10 | Divergencias, idempotencia do lote e dashboard. |
| E | T1.7 | Resumo e reabertura do caixa. |
| F | T1.11 | Prova integrada da Fase 1. |

## Limites operacionais

- Use somente `03_documentos/fixtures/prodoctor-fase1-sintetica.csv` ou outra fixture sem PII real.
- Nao solicitar nem incorporar API/credenciais do ProDoctor nesta fase.
- Nao implementar conciliacao bancaria, DRE final, repasses completos, contas a pagar, recorrencias, loops/agentes ou estoque.
- Nao marcar task concluida sem evidencia objetiva e teste humano quando solicitado.
