---
task: "Responder E-mail Financeiro"
order: 1
input: |
  - email: remetente, destinatários, assunto, data e corpo
  - triagem: saída de Tiago Triagem
output: |
  - corpo_resposta: texto do corpo do rascunho, ou SEM_RESPOSTA
---

# Responder E-mail Financeiro

Escreve o corpo do rascunho de resposta a um e-mail financeiro, em nome de Marcos Martini.

## Process

1. Identificar o que o e-mail pede: confirmar recebimento, informar vencimento, esclarecer divergência, enviar comprovante, validar nota substituída.
2. Listar mentalmente os documentos e valores citados (número, valor, vencimento). Usar somente o que está escrito.
3. Escrever resposta curta: agradecer, mostrar que entendeu (repetindo número e valor), dizer o próximo passo da Atlas.
4. Onde faltar dado (pagamento feito, data prevista, qual nota vale), escrever `[CONFIRMAR: ...]`.
5. Se o e-mail for apenas informativo e nada pedir, responder `SEM_RESPOSTA`.

## Output Format

Somente o texto do corpo, em parágrafos curtos, sem assunto, sem assinatura, sem explicações.

## Output Example

```
Prezado Denis, bom dia.

Recebemos o seu e-mail informando que a NF 133114 (R$ 8.360,05) foi substituída pela NF 132911 (R$ 17.101,54), referente ao PO497.

Vamos considerar a NF 132911 como a válida. [CONFIRMAR: data prevista de pagamento]. Qualquer ajuste, avisaremos.

Atenciosamente,
```

## Quality Criteria

- Valores, números e datas idênticos aos do e-mail.
- Nenhum compromisso de pagamento sem `[CONFIRMAR: ...]`.

## Veto Conditions

Refazer se:
1. Houver valor, nota ou data que não aparece no e-mail.
2. Houver confirmação de pagamento sem `[CONFIRMAR: ...]`.
3. Houver assunto ou assinatura no texto.
