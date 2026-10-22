---
execution: inline
agent: caio-cotacao
inputFile: squads/inside-sales/output/{processo}/cotacoes.md
outputFile: squads/inside-sales/output/{processo}/cotacoes.md
---

# Step 04: Comparar Ofertas e Recomendar

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/cotacoes.md` — rascunhos de RFQ já aprovados e enviados.
- Ofertas recebidas pelo usuário (frete, prazo, validade, condições) — fornecidas na conversa após o envio.

## Instructions

### Process

1. Registrar cada oferta recebida em tabela comparativa: frete, prazo/transit time, validade da cotação, condições (free time, demurrage, seguro incluso ou não).
2. Recomendar a melhor oferta com justificativa explícita, considerando prazo, validade e confiabilidade — não só preço, seguindo a task `comparar-ofertas.md` do agente Caio Cotação.
3. Se faltar informação em alguma oferta, registrar em pendencias.md e sinalizar a lacuna na tabela.

## Output Format

```
## Comparação de ofertas — {processo}

| Armador/Agente | Frete | Transit time | Validade | Condições |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

**Recomendação: [fornecedor].**
Justificativa: ...
```

## Output Example

```
## Comparação de ofertas — ATL-2026-0142

| Armador/Agente | Frete (USD) | Transit time | Validade | Condições |
|---|---|---|---|---|
| Agente A | USD 850 | 32 dias | 10 dias | 7 dias free time |
| Agente B | USD 790 | 38 dias | 5 dias | 5 dias free time |
| Agente C | USD 870 | 30 dias | 15 dias | 10 dias free time |

**Recomendação: Agente C.**
Justificativa: embora não seja o frete mais barato, oferece o menor transit time junto com a maior validade de cotação e o maior free time no destino — reduz risco de demurrage e dá mais margem para fechar a proposta com o cliente antes do vencimento da cotação. O Agente B tem o menor preço, mas a validade de 5 dias é arriscada dado o prazo de resposta do cliente.
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. A recomendação é baseada apenas no menor preço, sem considerar prazo/validade/condições.
2. Uma oferta recebida é omitida da tabela comparativa.

## Quality Criteria

- [ ] A comparação usa no mínimo frete, prazo e validade como critérios.
- [ ] A recomendação tem justificativa explícita.
