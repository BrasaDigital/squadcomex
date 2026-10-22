---
execution: inline
agent: rafael-redacao
outputFile: squads/inside-sales/output/{processo}/status-decisao.md
---

# Step 10: Follow-up e Status da Decisão

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/proposta.md` — proposta enviada ao cliente.
- Status atual informado pelo usuário (sem resposta, aprovado, pedindo ajuste, recusado).

## Instructions

### Process

1. Identificar o cenário real informado pelo usuário (sem resposta / aprovado / em ajuste / recusado).
2. Redigir a mensagem de follow-up adequada a esse cenário específico, seguindo a task `redigir-followup.md` do agente Rafael Redação — nunca um texto genérico único.
3. Atualizar `status-decisao.md` com o status atual e a data da movimentação.

## Output Format

```
[mensagem de follow-up adequada ao cenário]

---
status-decisao.md atualizado: status = "...", data = "YYYY-MM-DD"
```

## Output Example

```
Olá [Cliente],

Passando para saber se já teve a oportunidade de avaliar a proposta enviada em [data] para o processo ATL-2026-0142. Nossa validade de 10 dias se aproxima do fim.

Qualquer dúvida ou ajuste necessário, estamos à disposição para conversar.

Atenciosamente,
Equipe Atlas Log

---
status-decisao.md atualizado: status = "aguardando", data = 2026-10-28
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. A mensagem de follow-up não corresponde ao status real do processo.
2. status-decisao.md não é atualizado após a movimentação.

## Quality Criteria

- [ ] A mensagem de follow-up é específica ao cenário real.
- [ ] status-decisao.md é atualizado com status e data a cada interação.
