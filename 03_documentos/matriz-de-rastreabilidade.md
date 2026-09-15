# Matriz de Rastreabilidade — Escopo Definitivo

| ID | Decisao/requisito | Fonte | Fase | Evidencia esperada |
|---|---|---|---|---|
| RQ-01 | Fechamento e acompanhamento financeiro como criterio de sucesso | Reuniao de validacao; pedido do usuario | 1-5 | DRE, conciliacao, recorrencias e painel mensal |
| RQ-02 | Caixa diario registrado no sistema | Reuniao de validacao | 1 | Fechamento diario por forma de pagamento com divergencias |
| RQ-03 | Importacao do ProDoctor por API ou CSV | Reuniao; CSV anexo | 1, 3 | Dados importados com procedimento, convenio, medico, valores e glosa |
| RQ-04 | Importacao bancaria CSV/OFX | Reuniao de validacao | 2 | Extratos normalizados por conta |
| RQ-05 | Conciliacao com status por movimentacao | Reuniao; criterio de sucesso | 2-5 | Indicador conciliadas/totais e lista de pendencias |
| RQ-06 | Contas a pagar/receber com comprovantes | Reuniao de validacao | 2 | Baixas vinculadas a comprovantes e autorizacoes |
| RQ-07 | Lancamentos recorrentes automatizados | Reuniao; criterio de sucesso | 3-5 | Percentual automatizado/elegivel |
| RQ-08 | DRE gerencial preliminar e final | Reuniao; modelos de DRE | 3-5 | DRE disponivel em ate 5 dias uteis |
| RQ-09 | Repasses medicos por regra documentada | Reuniao; modelos de faturamento medico | 3, 5 | Demonstrativo conferivel por medico |
| RQ-10 | Painel de KPIs financeiros | Pedido do usuario; reuniao | 4-5 | Indicadores de prazo, conciliacao, automacao e pendencias |
| RQ-11 | Loops/agentes de acompanhamento sem autonomia financeira externa | Contrato das cinco fases | 4-5 | Loops candidatos com meta, fonte, cadencia e veredito humano |
| RQ-12 | Validacao transversal das fases 1-5 | Contrato das cinco fases | 5 | Matriz de validacao e aceite humano |
| FE-01 | Estoque fora do escopo atual | Reuniao de validacao | Fora | Registrado como evolucao futura |
| RK-01 | API ProDoctor pode nao estar disponivel | Reuniao de validacao | 1-3 | Caminho alternativo por CSV |
| RK-02 | DRE gerencial nao substitui contabilidade oficial | Revisao adversarial | 3-5 | Exportacao/conferencia e aviso de natureza gerencial |

## Cobertura

- Toda decisao do escopo definitivo aparece como requisito, fora de escopo, risco ou fase.
- As fases 1 a 5 entregam incrementos de sistema.
- As fases 4 e 5 acrescentam loops/agentes sem substituir os incrementos de sistema.
- A fase 5 valida o conjunto entregue nas fases 1 a 5.
