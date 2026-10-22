---
execution: inline
agent: aurelio-aura
outputFile: squads/inside-sales/output/{processo}/aura-lancamento.md
---

# Sincronizar Aura: valores finais e geração da proposta

## Context Loading

- `output/{processo}/aura-lancamento.md`
- `cotacoes.md` e, se o escopo for com desembaraço, `estimativa-custos.md`

## Instructions

Siga a task `lancar-no-aura.md`, **fase "estimativa"**:

1. **Escopo com desembaraço:** atualizar a estimativa de custo no Aura (`atualizar_estimativa`) e completar carga/volumes/taxas confirmados.
2. **Escopo cotação simples:** não há estimativa. Conferir que as taxas da cotação estão completas no Aura (`listar_taxas`).
3. Em ambos os casos, avisar o usuário que o Aura já tem tudo para **redigir a proposta** (e a estimativa, quando houver) e pedir que ele gere o documento no Aura e avise. Os documentos (proposta, estimativa, numerário, prestação de contas) são redigidos pelo Aura; o squad nunca os redige nem edita.

Regras: lançar só o que o dossiê tem; não duplicar; toda gravação com prévia e aprovação; atualizar `aura-lancamento.md`.

## Veto Conditions

1. Gravação sem prévia aprovada.
2. Estimativa lançada em processo de cotação simples.
3. Dado lançado que não consta no dossiê.

## Quality Criteria

- [ ] aura-lancamento.md reflete o estado do Aura e informa se a proposta já foi gerada no Aura.
