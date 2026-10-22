---
execution: inline
agent: estela-estimativa
inputFile: squads/inside-sales/output/{processo}/cotacoes.md
outputFile: squads/inside-sales/output/{processo}/estimativa-custos.md
---

# Step 05: Montar Estimativa de Custos

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/pedido.md` — incoterm, modal, valor da mercadoria.
- `squads/inside-sales/output/{processo}/cotacoes.md` — frete recomendado e condições aprovadas.
- `pipeline/data/domain-framework.md` — base legal de valor aduaneiro e tributos (Decreto 6.759/2009, art. 77).

## Instructions

### Process

1. Ler pedido.md e cotacoes.md para reunir os dados confirmados de valor, frete e condições.
2. Listar todos os componentes de custo aplicáveis (mercadoria, frete, seguro, valor aduaneiro, II, IPI, PIS/COFINS, ICMS, despachante, armazenagem), seguindo a task `montar-estimativa.md` do agente Estela Estimativa.
3. Marcar cada item como "Confirmado" (com origem) ou "A confirmar" (com fonte oficial indicada) — nunca estimar uma alíquota sem fonte.

## Output Format

```
# estimativa-custos.md — {processo}

| Item | Valor | Status |
|---|---|---|
| ... | ... | Confirmado/A confirmar |

**Status geral:** ...
```

## Output Example

```
# estimativa-custos.md — ATL-2026-0142

| Item | Valor | Status |
|---|---|---|
| Valor da mercadoria (FOB) | USD 12.000 | Confirmado (invoice do cliente) |
| Frete internacional (recomendado: Agente C) | USD 870 | Confirmado (cotacoes.md) |
| Seguro internacional | USD 60 (0,5% sobre CFR) | Confirmado (tabela Atlas Log) |
| Valor aduaneiro (FOB + frete + seguro) | USD 12.930 | Calculado |
| Imposto de Importação | A confirmar | Pendente — alíquota depende da classificação NCM final (fonte: Receita Federal, Tabela TEC) |
| IPI | A confirmar | Pendente — depende do NCM final |
| PIS/COFINS-Importação | A confirmar | Pendente — alíquota varia conforme NCM (2,1% a 9,65%) |
| ICMS | A confirmar | Pendente — depende do estado de destino |
| Honorários despachante | R$ 1.800 | Confirmado (tabela Atlas Log) |
| Armazenagem (estimativa 5 dias) | R$ 950 | Confirmado (tabela porto Santos) |

**Status geral:** Estimativa parcial. Itens tributários dependem da confirmação do NCM final — ver pendencias.md.
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. Alguma alíquota tributária aparece com número sem fonte confirmada.
2. O total é apresentado como definitivo mesmo havendo itens "a confirmar".

## Quality Criteria

- [ ] Todo item tributário está marcado "Confirmado" (com fonte) ou "A confirmar" (com fonte oficial).
- [ ] O status geral reflete corretamente se a estimativa está completa ou parcial.
