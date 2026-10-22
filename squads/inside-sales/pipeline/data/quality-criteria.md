# Quality Criteria — Inside Sales Comex

## Dados do pedido (pedido.md)
- Todos os campos obrigatórios preenchidos ou listados como pendência explícita.
- NCM sempre marcado "a confirmar", nunca definitivo.
- Toda possível exigência de anuência (ANVISA/MAPA/IBAMA) avaliada e documentada.

## Cotações (cotacoes.md)
- Todo rascunho de RFQ está marcado "RASCUNHO — aguardando aprovação" antes do checkpoint (a).
- Comparação usa no mínimo frete, prazo e validade como critérios.
- Recomendação final sempre justificada, nunca "melhor opção" sem explicação.
- Nenhuma oferta inventada; dados ausentes sinalizados, não preenchidos.

## Estimativa de custos (estimativa-custos.md)
- Nenhuma alíquota tributária sem estar "Confirmado" (com origem) ou "A confirmar" (com fonte oficial).
- Total nunca mistura itens confirmados e pendentes sem identificação clara.
- Toda pendência de custo refletida em pendencias.md com área responsável.

## Proposta e follow-up (proposta.md, status-decisao.md)
- Nenhum valor "a confirmar" na estimativa aparece como definitivo na proposta.
- Proposta sempre tem validade e prazo de retorno explícitos.
- status-decisao.md atualizado a cada movimentação (enviada/aguardando/aprovada/em ajuste/recusada).

## Revisão do dossiê (Vitor Veredito)
- Todo valor citado na proposta conferido contra a fonte (cotacoes.md / estimativa-custos.md).
- Nenhuma pendência aberta omitida da proposta sem ressalva.
- Veredito sempre explícito: APROVADO ou REPROVADO.

## Critério geral de aprovação (revisão)
- APROVADO: média das notas ≥ 7 e nenhum critério individual abaixo de 4.
- REPROVADO: média < 7 OU qualquer critério abaixo de 4 (gatilho de reprovação automática).
