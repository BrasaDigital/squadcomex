---
execution: inline
agent: aurelio-aura
outputFile: squads/inside-sales/output/{processo}/aura-lancamento.md
---

# Sincronizar Aura: proposta enviada

## Context Loading

- `output/{processo}/aura-lancamento.md`, `proposta.md`

## Instructions

Fase "proposta": após o usuário enviar a proposta, registrar no Aura uma observação (`adicionar_observacao`) com data de envio e validade, e ajustar o status do processo (`atualizar_status`) se o usuário pedir. Não redigir nem alterar o documento de proposta, que é do Aura. Atualizar `aura-lancamento.md`.

## Veto Conditions

1. Gravação sem prévia aprovada.
