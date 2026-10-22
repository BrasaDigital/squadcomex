---
execution: inline
agent: rafael-redacao
inputFile: squads/inside-sales/output/{processo}/estimativa-custos.md
outputFile: squads/inside-sales/output/{processo}/proposta.md
---

# Step 07: Redigir Proposta Consultiva

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/pedido.md` — dados do pedido.
- `squads/inside-sales/output/{processo}/cotacoes.md` — oferta recomendada e aprovada.
- `squads/inside-sales/output/{processo}/estimativa-custos.md` — estimativa aprovada no checkpoint anterior.
- `_opensquad/_memory/company.md` — tom de voz e identidade da Atlas Log.

## Instructions

### Process

1. Ler pedido.md, cotacoes.md e estimativa-custos.md já aprovados.
2. Estruturar a proposta (contexto, escopo, condições comerciais, validade, próximos passos), seguindo a task `redigir-proposta.md` do agente Rafael Redação.
3. Sinalizar explicitamente qualquer item "a confirmar" da estimativa como "sujeito a confirmação" na proposta — nunca como valor definitivo.
4. Marcar a proposta como "RASCUNHO — aguardando aprovação" antes do checkpoint final de envio.

## Output Format

```
# proposta.md — {processo} (RASCUNHO — aguardando aprovação para envio)

Olá [Cliente],

[contexto do pedido]

**Escopo:** ...

**Condições comerciais:**
- ... (confirmado/sujeito a confirmação)

**Validade da proposta:** ...
**Prazo de retorno esperado:** ...

Atenciosamente,
Equipe Atlas Log
```

## Output Example

```
# proposta.md — ATL-2026-0142 (RASCUNHO — aguardando aprovação para envio)

Olá [Cliente],

Com base no pedido de importação das peças automotivas (Shanghai → Santos), preparamos a proposta abaixo.

**Escopo:** Agenciamento de carga marítima (LCL) e suporte ao desembaraço aduaneiro, incluindo seguro internacional e acompanhamento documental.

**Condições comerciais:**
- Frete internacional + seguro: USD 930 (confirmado)
- Honorários Atlas Log (despacho): R$ 1.800 (confirmado)
- Armazenagem estimada (5 dias): R$ 950 (confirmado)
- Tributos de importação (II, IPI, PIS/COFINS, ICMS): estimativa sujeita à confirmação da classificação fiscal (NCM) final

**Validade da proposta:** 10 dias corridos a partir do envio.
**Prazo de retorno esperado:** até 5 dias úteis.

Atenciosamente,
Equipe Atlas Log
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. Um item "a confirmar" na estimativa aparece como valor definitivo na proposta.
2. A proposta não tem validade ou prazo de retorno definidos.

## Quality Criteria

- [ ] Nenhum valor "a confirmar" aparece como definitivo.
- [ ] A proposta tem validade e prazo de retorno explícitos.
