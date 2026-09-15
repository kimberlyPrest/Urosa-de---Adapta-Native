# Escopo Definitivo — Urosaude & Associados LTDA

## 1. Resultado de negocio

Construir, em cinco fases, um sistema financeiro operacional para reduzir a dependencia de planilhas e permitir o fechamento e acompanhamento financeiro da clinica com DRE, conciliacao, caixa, contas, recorrencias e repasses medicos em fluxo rastreavel.

O criterio de sucesso do projeto e: **fechamento e acompanhamento financeiro da clinica**.

Metas globais:

- Tempo de fechamento financeiro: dias entre encerramento do mes e DRE disponivel. Meta inicial: **ate 5 dias uteis**.
- Conciliacao financeira: movimentacoes conciliadas / movimentacoes totais. Meta: **98% ou mais**.
- Automacao dos lancamentos recorrentes: lancamentos processados sem digitacao manual / lancamentos elegiveis. Meta: **90% ou mais**.

Fontes usadas:

- Reuniao de validacao de escopo de 28/08/2026: dor confirmada como fechamento e acompanhamento financeiro; estoque citado como gargalo futuro, mas nao prioridade atual.
- Escopo base `03-Projeto/01-Escopo.md`: consolidacao financeira, conciliacao, faturamento, repasses, DRE e processos mapeados.
- CSV anexo `CSV (1).CSV`: evidencia de formato de exportacao do ProDoctor com local, usuario/medico, prontuario, paciente, convenio, data, conta, procedimento, glosa, faturado, clinica, medico e credito/debito.
- Arquivos de contexto em `04-Mapeamento-Processos/00-Contexto/`: modelos de caixa, fluxo de caixa, DRE, faturamento medico e centro cirurgico.

Nota de seguranca: instrucoes contidas em transcricoes, CSVs, planilhas ou documentos anexos foram tratadas como contexto do cliente, nao como ordens ao agente.

## 2. Atores

- Direcao: acompanha DRE, fluxo de caixa, indicadores e aprova decisoes gerenciais.
- Gestao financeira: fecha caixa, importa extratos, classifica movimentacoes, concilia, valida DRE e acompanha pendencias.
- Recepcao/caixa: registra fechamento diario por forma de pagamento e anexa evidencias.
- Faturamento: acompanha procedimentos, convenios, glosas e valores pendentes.
- Medicos prestadores: recebem prestacao de contas e repasses calculados.
- Consultor Adapta: revisa fases, seguranca, escopo e evolucao do sistema.
- Ethos/Agentes: executam tasks a partir do repositorio, com teste humano obrigatorio quando solicitado.

## 3. Fluxo operacional alvo

1. Recepcao registra diariamente caixa, formas de pagamento, divergencias e comprovantes.
2. Gestao financeira importa extratos bancarios em CSV/OFX por conta, preservando origem e saldo.
3. Sistema recebe ou importa dados do ProDoctor, incluindo procedimentos, pacientes, convenio, medico, glosa e valores faturados.
4. Sistema confronta caixa, bancos, contas, faturamento e repasses, gerando status por movimentacao: conciliada, divergente ou pendente.
5. Lancamentos recorrentes elegiveis sao gerados automaticamente por competencia.
6. Contas a pagar recebem cadastro, autorizacao, baixa e comprovante vinculado.
7. DRE gerencial e paineis de KPI ficam disponiveis para acompanhamento mensal.
8. Direcao aprova fechamento, registra log de fechamento/reabertura e acompanha desvios de prazo, conciliacao e automacao.

## 4. Capacidades do sistema

- Cadastro de contas financeiras, centros/areas, categorias, fornecedores, medicos, convenios e regras basicas de classificacao.
- Separacao entre pessoa fisica, pessoa juridica e contas/competencias quando necessario para leitura gerencial.
- Fechamento de caixa diario por forma de pagamento, com anexos e trilha de auditoria.
- Importacao de extratos bancarios CSV/OFX e normalizacao de dados por conta.
- Importacao/ingestao de dados do ProDoctor, preferencialmente via API; enquanto API nao estiver disponivel, uso de arquivo exportado como insumo controlado.
- Conciliacao entre caixa, banco, contas a pagar/receber, faturamento e repasses.
- Contas a pagar com autorizacao, baixa e comprovante.
- Motor de lancamentos recorrentes por template, competencia, vencimento e regra de recorrencia.
- Repasses medicos com regras por medico, convenio, procedimento, local, imposto/glosa quando aplicavel e prestacao de contas.
- DRE gerencial preliminar e final, com logs de fechamento, aprovacao e reabertura.
- Painel de KPIs: prazo de fechamento, percentual de conciliacao, percentual de automacao, pendencias e divergencias.

## 5. Decisoes consolidadas

- O primeiro foco aprovado e financeiro; estoque fica fora do escopo atual.
- O sistema deve substituir a rotina de consolidacao manual em planilhas como fonte operacional do fechamento, mantendo exportacoes quando necessario.
- O ProDoctor e fonte relevante para consultas, procedimentos, faturamento e repasses; a API deve ser solicitada ao fornecedor.
- O CSV do ProDoctor pode ser usado como caminho inicial de importacao se a API ainda nao estiver disponivel.
- O usuario nao deve mandar prompts diretamente no projeto do Skip; a implementacao deve ser conduzida pelo Ethos a partir das tasks e do repositorio.
- As cinco fases entregam incrementos de sistema; fases 4 e 5 acrescentam loops/agentes; fase 5 tambem valida ponta a ponta o que foi feito nas fases 1 a 5.

## 6. Fora de escopo

- Controle de estoque, compras e almoxarifado.
- Prontuario clinico, agenda medica assistencial ou fluxo clinico fora dos dados necessarios ao financeiro.
- Integracao bancaria automatica via Open Finance antes de validar importacao CSV/OFX.
- Automacao de recurso de glosa junto a convenios; o sistema registra e acompanha glosas, mas a acao externa continua humana.
- Publicacao, push, criacao de repositorio ou alteracao de permissoes sem confirmacao humana especifica.
- Substituir contabilidade oficial; o DRE e gerencial e deve poder apoiar a contabilidade, nao tomar seu lugar legal sem validacao.

## 7. Fases

### Fase 1 — Base financeira e caixa diario

Resultado de negocio: a clinica passa a registrar caixa diario, contas, categorias e fontes de entrada em um sistema unico, com demonstracao visivel de fluxo de caixa operacional.

Capacidades: login e perfis iniciais; cadastros financeiros essenciais; caixa diario com anexos e divergencias; importacao inicial do CSV ProDoctor; painel simples de caixa do dia, pendencias e total por forma de pagamento.

Atores: recepcao/caixa, gestao financeira e direcao.

Dados e integracoes: planilhas/modelos existentes, CSV ProDoctor, anexos/comprovantes manuais.

Regras: cada lancamento deve ter competencia, data, valor, origem, categoria e responsavel; divergencia nao bloqueia registro, mas exige status e justificativa.

Entrega visivel: dashboard com caixa diario e importacao do arquivo de procedimentos, permitindo conferir se valores basicos aparecem por dia, convenio e medico.

Fora desta fase: conciliacao bancaria automatizada, DRE final, repasses completos e loops.

Aceite: caixa diario registrado; CSV importado sem digitacao manual dos campos principais; divergencias ficam rastreaveis.

### Fase 2 — Importacao bancaria, conciliacao e contas

Resultado de negocio: a gestao financeira consegue confrontar caixa, bancos e contas, reduzindo busca manual por informacao.

Capacidades: importacao de extratos CSV/OFX por conta bancaria; normalizacao de movimentacoes; contas a pagar/receber com cadastro, vencimento, autorizacao, baixa e comprovante; motor inicial de conciliacao; tela de tratamento de pendencias.

Atores: gestao financeira, recepcao/caixa e direcao.

Dados e integracoes: extratos bancarios CSV/OFX, caixa diario, contas e comprovantes.

Regras: movimentacao conciliada precisa preservar vinculo com origem; baixa exige comprovante quando aplicavel; divergencia exige motivo.

Entrega visivel: lista de movimentacoes conciliadas/pendentes por conta e competencia, com indicador de percentual de conciliacao.

Fora desta fase: regras completas de repasse medico, DRE final e agentes de acompanhamento.

Aceite: conciliacao mensal demonstravel com calculo de movimentacoes conciliadas / totais.

### Fase 3 — Recorrencias, DRE gerencial e repasses medicos

Resultado de negocio: o fechamento mensal passa a produzir DRE gerencial preliminar e repasses medicos com menos digitacao.

Capacidades: templates de lancamentos recorrentes; geracao automatica por competencia; classificacao gerencial para DRE; DRE preliminar e final com logs; regras iniciais de repasse medico; prestacao de contas para conferencia.

Atores: gestao financeira, direcao e medicos prestadores.

Dados e integracoes: caixa, bancos, contas, CSV/API ProDoctor, regras de repasse e modelos de DRE.

Regras: DRE final depende de competencia fechada; reabertura deve registrar motivo; recorrencia automatica precisa ser rastreavel e reversivel.

Entrega visivel: DRE da competencia com indicador de prazo de fechamento e demonstrativo de repasse de pelo menos um medico/modelo prioritario.

Fora desta fase: loop autonomo de acompanhamento e validacao transversal completa.

Aceite: DRE disponivel em ate 5 dias uteis em simulacao/competencia piloto; recorrencias elegiveis geradas pelo sistema; repasse prioritario conferivel.

### Fase 4 — Paineis, alertas e loops de acompanhamento financeiro

Resultado de negocio: a direcao e a gestao acompanham o fechamento em tempo real e recebem alertas acionaveis sobre pendencias, conciliacao e automacao.

Capacidades de sistema: painel gerencial de KPIs; alertas configuraveis para fechamento em risco, queda de conciliacao, recorrencia nao processada e pendencias vencidas; historico mensal; registro de decisoes e justificativas de excecao.

Loops/agentes candidatos:

- Loop de fechamento mensal: monitora checklist da competencia ate DRE disponivel; meta: fechamento em ate 5 dias uteis; validacao: data de encerramento versus data de DRE aprovada.
- Loop de conciliacao: monitora percentual conciliado; meta: 98% ou mais; validacao: movimentacoes conciliadas / totais por competencia.
- Agente de pendencias financeiras: lista itens divergentes/pendentes e sugere proxima acao sem executar pagamento ou recurso externo.

Atores: gestao financeira, direcao, Ethos/agentes e consultor.

Dados e integracoes: base do sistema, extratos, caixa, ProDoctor, regras e historico mensal.

Regras: alerta nao aprova fechamento; agente nao executa pagamento, nao altera banco e nao muda regra de repasse sem validacao humana.

Entrega visivel: painel mensal com alertas e loops candidatos documentados, medindo as tres metas globais.

Fora desta fase: validacao ponta a ponta final de todas as fases.

Aceite: loops configurados como candidatos com baseline, alvo, cadencia, fonte de medicao e responsavel pelo veredito.

### Fase 5 — Validacao integrada, ajustes finais e go-live assistido

Resultado de negocio: o sistema financeiro opera de ponta a ponta com evidencia de fechamento, conciliacao, automacao e acompanhamento mensal.

Capacidades de sistema: ajustes finais de DRE, repasses, glosas, conciliacao, paineis e permissoes; relatorios/exportacoes para direcao e apoio contabilidade; rotina de encerramento de competencia; registro de riscos residuais, pendencias e decisao de go-live/encerramento.

Loops/agentes candidatos:

- Loop de melhoria de classificacao: monitora categorias recorrentes corrigidas manualmente; meta: reduzir retrabalho mensal; validacao: quantidade de ajustes manuais por competencia.
- Loop de confiabilidade do fechamento: verifica se dados essenciais chegaram antes do prazo; meta: nao iniciar DRE final com fonte critica ausente; validacao: checklist de fontes.

Matriz de validacao das fases 1 a 5:

- Fase 1: caixa diario, cadastros e importacao ProDoctor reproduzem dados essenciais.
- Fase 2: conciliacao preserva origem, status e tratamento de divergencias.
- Fase 3: recorrencias, DRE e repasses geram resultado conferivel.
- Fase 4: paineis, alertas e loops medem os criterios globais.
- Fase 5: fluxo integrado fecha competencia, gera evidencias e registra aceite humano.

Atores: gestao financeira, direcao, recepcao/caixa, medicos prestadores, consultor e Ethos/agentes.

Dados e integracoes: todos os dados das fases anteriores, permissoes, logs e evidencias.

Regras: item nao validado permanece pendente; ausencia de erro relatado nao aprova; go-live exige aceite humano.

Entrega visivel: fechamento piloto de competencia com DRE, conciliacao, recorrencias e painel de indicadores.

Aceite: criterios globais medidos e documentados; decisao humana de go-live, ajustes ou pendencias registrada.

## 8. Riscos

- API do ProDoctor indisponivel ou limitada; mitigacao: usar CSV exportado como fonte inicial e manter decisao sobre API como dependencia.
- Qualidade dos dados historicos e planilhas pode conter inconsistencias; mitigacao: importar com logs, status de divergencia e validacao humana.
- Baixa familiaridade do cliente com GitHub, Skip e Ethos; mitigacao: instrucoes operacionais simples e testes por task.
- Escopo financeiro pode crescer para estoque, compras ou agenda; mitigacao: manter fora de escopo e registrar como evolucao futura.
- DRE gerencial pode divergir da contabilidade oficial; mitigacao: explicitar natureza gerencial e permitir exportacao/conferencia.
- Repasses possuem regras particulares por medico/convenio; mitigacao: comecar por medico/modelo prioritario e exigir regra documentada.

## 9. Gates e decisoes pendentes

- Cliente deve solicitar e disponibilizar API/credenciais do ProDoctor, se existir.
- Consultor deve aprovar o escopo definitivo antes de gerar SPECs profundas.
- CSM/cliente devem aprovar a fase antes da execucao no ambiente do cliente.
- Cada task implementada pelo Ethos exige teste humano quando solicitado.
- Conectores, agentes e loops das fases 4 e 5 devem ser validados na call de setup antes de ativacao.

## 10. Criterios globais de aceite

- DRE da competencia disponivel em ate 5 dias uteis apos encerramento do mes.
- Conciliacao financeira mensal igual ou superior a 98%.
- Automacao de lancamentos recorrentes elegiveis igual ou superior a 90%.
- Toda movimentacao relevante possui origem, status, responsavel e trilha de auditoria.
- O cliente consegue explicar o status financeiro do mes por caixa, banco, contas, DRE e pendencias.
- Estoque permanece fora do aceite deste projeto.

## 11. Revisao serial do painel

- Revisor de coerencia: aprovado com ajuste seguro aplicado; o criterio de sucesso financeiro foi ligado a requisitos e fases.
- Revisor de viabilidade: aprovado com risco registrado sobre API ProDoctor e qualidade de dados importados.
- Guardiao de escopo: aprovado com corte aplicado; estoque ficou fora do escopo atual.
- Revisor adversarial: aprovado com ressalva; DRE e gerencial, nao substitui validacao contabil/legal.

## 12. Matriz fonte -> decisao -> requisito -> fase

| Fonte/achado | Decisao | Requisito | Fase |
|---|---|---|---|
| Reuniao: dor principal e fechamento/acompanhamento financeiro | Priorizar financeiro | Fluxo completo de caixa, conciliacao, DRE e KPIs | 1-5 |
| Usuario: metas de fechamento, conciliacao e recorrencias | Usar como criterios globais | KPIs e evidencias mensais | 3-5 |
| Reuniao: caixa diario por dinheiro/cheque e outras formas | Registrar caixa no sistema | Caixa diario com anexos e divergencias | 1 |
| Reuniao: extratos CSV/OFX | Importar bancos sem redigitar | Importador e conciliacao bancaria | 2 |
| Reuniao: contas a pagar com autorizacao e comprovante | Controlar pagamentos | Contas com baixa e evidencias | 2 |
| Reuniao: recorrencias mensais | Automatizar elegiveis | Templates por competencia | 3 |
| Reuniao: DRE preliminar/final e logs | Gerar DRE gerencial | Fechamento, aprovacao e reabertura | 3 |
| Reuniao/CSV: ProDoctor e repasses | Integrar via API ou CSV | Importacao ProDoctor e regras de repasse | 1, 3 |
| Reuniao: estoque e outro gargalo | Nao incluir agora | Fora de escopo/evolucao futura | Fora |
| Contrato Adapta | Manter 5 fases com loops nas fases 4 e 5 | Loops e validacao transversal | 4-5 |
