---
task: "Responder E-mail Documental"
order: 1
input: |
  - email: remetente, destinatários, assunto, data e corpo
  - triagem: saída de Tiago Triagem
output: |
  - corpo_resposta: texto do corpo do rascunho, ou SEM_RESPOSTA
---

# Responder E-mail Documental

Escreve o corpo do rascunho de resposta a um e-mail sobre documentos de importação, em nome de Marcos Martini.

## Process

1. Identificar o pedido: enviar documentos, confirmar recebimento, pedir documento faltante, esclarecer divergência.
2. Listar os documentos citados no e-mail (invoice, packing list, CE/BL, certificados, comprovantes) e o que o e-mail diz sobre cada um.
3. Escrever resposta curta: confirmar o que foi recebido, nomear o que falta ou diverge, indicar prazo e próximo passo.
4. Para qualquer dado que falte (prazo de registro, qual documento vale), escrever `[CONFIRMAR: ...]`.
5. Se o e-mail for apenas informativo e nada pedir, responder `SEM_RESPOSTA`.

## Output Format

Somente o texto do corpo, em parágrafos curtos (lista curta se houver mais de dois documentos), sem assunto, sem assinatura, sem explicações.

## Output Example

```
Prezados, bom dia.

Recebemos os documentos do processo 26/206 (IMP. PROTECT SHIELD) para o registro da DI: invoice e packing list.

Para seguirmos, ainda precisamos do CE/BL. Poderiam enviar até [CONFIRMAR: prazo para registro]?

Atenciosamente,
```

## Quality Criteria

- Todos os documentos citados aparecem no e-mail.
- Pedidos específicos, com prazo ou `[CONFIRMAR: ...]`.

## Veto Conditions

Refazer se:
1. Houver afirmação sobre conteúdo de anexo que o e-mail não descreve.
2. Houver pedido genérico de "documentação completa".
3. Houver assunto ou assinatura no texto.
