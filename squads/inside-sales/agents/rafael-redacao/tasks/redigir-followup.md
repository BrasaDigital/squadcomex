---
task: "Redigir Follow-up"
order: 2
input: |
  - proposta_md: Proposta enviada ao cliente
  - status_atual: Situação atual informada pelo usuário (sem resposta, aprovado, pedindo ajuste, recusado)
output: |
  - followup_mensagem: Mensagem de follow-up adequada ao cenário
  - status_decisao_md: Atualização de status-decisao.md com a movimentação e a data
---

# Redigir Follow-up

Redige a mensagem de acompanhamento da decisão do cliente conforme o cenário real, e mantém status-decisao.md atualizado.

## Process

1. Identificar o cenário atual: sem resposta, aprovado, pedindo ajuste ou recusado.
2. Redigir a mensagem de follow-up adequada a esse cenário específico — nunca um texto genérico único.
3. Atualizar status-decisao.md com o status atual (enviada / aguardando / aprovada / em ajuste / recusada) e a data da movimentação.

## Output Format

```yaml
followup:
  cenario: "sem_resposta | aprovado | em_ajuste | recusado"
  mensagem: "..."
status_decisao:
  status: "..."
  data: "YYYY-MM-DD"
```

## Output Example

> Use como referência de qualidade, não como template rígido.

```
Olá [Cliente],

Passando para saber se já teve a oportunidade de avaliar a proposta enviada em [data] para o processo ATL-2026-0142. Nossa validade de 10 dias se aproxima do fim.

Qualquer dúvida ou ajuste necessário, estamos à disposição para conversar.

Atenciosamente,
Equipe Atlas Log

---
status-decisao.md atualizado: status = "aguardando", data = 2026-10-28
```

## Quality Criteria

- [ ] A mensagem de follow-up corresponde exatamente ao cenário real (não é genérica).
- [ ] status-decisao.md é atualizado com status e data a cada movimentação.
- [ ] O tom permanece consultivo, nunca de cobrança agressiva.

## Veto Conditions

Reject and redo if ANY are true:
1. A mensagem de follow-up não corresponde ao status real do processo.
2. status-decisao.md não é atualizado após a movimentação.
