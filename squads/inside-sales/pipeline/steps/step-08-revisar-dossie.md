---
execution: inline
agent: vitor-veredito
inputFile: squads/inside-sales/output/{processo}/proposta.md
---

# Step 08: Revisar Consistência do Dossiê

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/pedido.md`
- `squads/inside-sales/output/{processo}/cotacoes.md`
- `squads/inside-sales/output/{processo}/estimativa-custos.md`
- `squads/inside-sales/output/{processo}/proposta.md`
- `squads/inside-sales/output/{processo}/pendencias.md`
- `pipeline/data/quality-criteria.md` — critérios de aprovação/reprovação

## Instructions

### Process

1. Ler os cinco arquivos do dossiê e conferir que os valores da proposta batem exatamente com cotacoes.md e estimativa-custos.md.
2. Conferir que nenhum item "a confirmar" aparece como definitivo na proposta e que toda pendência aberta está refletida com ressalva, seguindo a task `revisar-dossie.md` do agente Vitor Veredito.
3. Pontuar cada critério (1–10) com justificativa e aplicar a regra de decisão (APROVADO se média ≥ 7 e nenhum critério < 4; caso contrário, REPROVADO).

## Output Format

```
VEREDITO: APROVADO | REPROVADO
Processo: {processo}

| Critério | Nota | Justificativa |
|---|---|---|
| ... | .../10 | ... |

[Se REPROVADO: lista de "Correção obrigatória:" com arquivo e trecho exatos]
```

## Output Example

```
==============================
 VEREDITO: REPROVADO
==============================
Processo: ATL-2026-0143

Correção obrigatória: proposta.md cita "frete: USD 790" mas cotacoes.md registra a recomendação final como Agente C (USD 870). Corrigir o valor na proposta antes de reapresentar.

Correção obrigatória: estimativa-custos.md lista ICMS como "a confirmar", mas proposta.md apresenta o custo total sem ressalva sobre esse item. Adicionar a ressalva de "sujeito a confirmação" na proposta.

**VEREDITO: REPROVADO — devolvido para Rafael Redação.**
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. Um valor na proposta não corresponde ao valor confirmado em cotacoes.md ou estimativa-custos.md.
2. Uma pendência aberta não está refletida na proposta.

## Quality Criteria

- [ ] Todo valor citado na proposta foi conferido contra a fonte.
- [ ] O veredito é sempre explícito, com nota e justificativa por critério.

Se REPROVADO, volte ao Passo 7 (Rafael Redação) com as correções obrigatórias listadas, conforme `on_reject: 7` definido em pipeline.yaml.
