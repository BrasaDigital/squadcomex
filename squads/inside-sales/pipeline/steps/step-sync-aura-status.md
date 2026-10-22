---
execution: inline
agent: aurelio-aura
outputFile: squads/inside-sales/output/{processo}/aura-lancamento.md
---

# Sincronizar Aura: status da decisão

## Context Loading

- `output/{processo}/aura-lancamento.md` (o que já foi lançado; criar se não existir)
- `status-decisao.md`

## Instructions

Siga a task `lancar-no-aura.md` do agente Aurélio Aura, **fase "status"**: atualizar o status do processo (`atualizar_status`) conforme a decisão do cliente e registrar observação; completar qualquer dado que o cliente tenha informado.

Regras: lançar só o que o dossiê já tem (campo ausente fica pendente, nunca inventado); não duplicar o que já está no Aura; toda gravação mostra prévia e só grava (`confirmar: true`) após aprovação do usuário; ao final, atualizar `aura-lancamento.md` (lançado / pendente / data).

## Veto Conditions

Reject and redo if ANY of these are true:
1. Houve gravação sem prévia aprovada.
2. Foi criado processo ou item duplicado.
3. Algum dado lançado não consta no dossiê.

## Quality Criteria

- [ ] aura-lancamento.md reflete exatamente o estado do Aura após esta fase.
