---
task: "Montar Estimativa"
order: 1
input: |
  - pedido_md: Dados confirmados do pedido (incoterm, modal, valor da mercadoria se informado)
  - cotacoes_md: Frete recomendado e condições da oferta aprovada
output: |
  - estimativa_custos_md: Tabela de estimativa de custo total, com status de cada item
  - pendencias_md: Itens tributários/taxas pendentes de confirmação, com fonte oficial indicada
---

# Montar Estimativa

Monta a estimativa de custo total do processo (frete, seguro, impostos, despesas, despachante, armazenagem), usando apenas dados informados e marcando claramente o que depende de confirmação.

## Process

1. Ler pedido.md (incoterm, modal, valor da mercadoria se informado) e cotacoes.md (frete recomendado).
2. Listar todos os componentes de custo aplicáveis: valor da mercadoria, frete internacional, seguro, valor aduaneiro, Imposto de Importação, IPI, PIS/COFINS-Importação, ICMS, taxas portuárias/aeroportuárias, honorários de despachante, armazenagem, transporte interno.
3. Para cada componente, usar apenas valores informados ou tabela já confirmada pela Atlas Log; quando faltar alíquota, indicar a fonte oficial aplicável e registrar pendência em vez de estimar.
4. Somar os itens confirmados em um subtotal "estimativa com base em dados confirmados" — nunca apresentar total único como definitivo se houver pendências.
5. Escrever `estimativa-custos.md` com a tabela completa, status de cada item e pendências associadas.

## Output Format

```yaml
estimativa_custos:
  itens:
    - nome: "..."
      valor: "..."
      status: "Confirmado | A confirmar"
      fonte: "..."
  status_geral: "completa | parcial"
pendencias:
  - item: "..."
    motivo: "..."
    fonte_oficial: "..."
    area: "..."
```

## Output Example

> Use como referência de qualidade, não como template rígido.

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
| PIS/COFINS-Importação | A confirmar | Pendente — alíquota varia conforme NCM (2,1% a 9,65% sobre valor aduaneiro) |
| ICMS | A confirmar | Pendente — depende do estado de destino |
| Honorários despachante | R$ 1.800 | Confirmado (tabela Atlas Log) |
| Armazenagem (estimativa 5 dias) | R$ 950 | Confirmado (tabela porto Santos) |

**Status geral:** Estimativa parcial. Itens tributários dependem da confirmação do NCM final — ver pendencias.md.
```

## Quality Criteria

- [ ] Todo item tributário está marcado "Confirmado" (com fonte) ou "A confirmar" (com fonte oficial indicada).
- [ ] O status geral reflete corretamente se a estimativa está completa ou parcial.
- [ ] Toda pendência de custo aparece em pendencias.md com a área responsável.

## Veto Conditions

Reject and redo if ANY are true:
1. Alguma alíquota tributária aparece com um número sem fonte confirmada.
2. O total é apresentado como definitivo mesmo havendo itens "a confirmar".
