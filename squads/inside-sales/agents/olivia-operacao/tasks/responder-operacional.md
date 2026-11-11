---
task: "Responder E-mail Operacional"
order: 1
input: |
  - email: remetente, destinatários, assunto, data e corpo
  - triagem: saída de Tiago Triagem
output: |
  - corpo_resposta: texto do corpo do rascunho, ou SEM_RESPOSTA
---

# Responder E-mail Operacional

Escreve o corpo do rascunho de resposta a um e-mail de operação (embarque, CE, permanência, follow-up), em nome de Marcos Martini.

## Process

0. Consultar o Aura (somente leitura) pelo processo, BL ou container. O Aura é a base de dados: status, ETA, armador, containers e documentos vêm dele.
1. Identificar o pedido: confirmar ciência, informar andamento, cobrar atualização, tratar prazo de permanência ou armazenagem.
2. Anotar os identificadores e datas que aparecem no e-mail (referência, BL, container, booking, ETD/ETA, prazos).
3. Escrever resposta curta que DÊ a informação pedida (status, datas, armador, containers), repetindo identificadores e prazo e indicando o próximo passo. Pedido de status respondido só com "recebemos / vamos verificar" é proibido.
4. Para qualquer dado que falte ou promessa que dependa de decisão do Marcos, escrever `[CONFIRMAR: ...]`.
5. Se o e-mail for apenas informativo e nada pedir, responder `SEM_RESPOSTA`.

## Output Format

Somente o texto do corpo, em parágrafos curtos, sem assunto, sem assinatura, sem explicações.

## Output Example

```
Prezados, bom dia.

Recebemos a confirmação de embarque do processo 26/294 (CHENGDU HUARUI INDUSTRIAL, SHANGHAI - SANTOS). Ficamos no aguardo do BL original assim que emitido.

Pedimos, por favor, que nos informem o ETA atualizado até [CONFIRMAR: data limite desejada], para organizarmos a desunitização.

Atenciosamente,
```

## Quality Criteria

- Identificadores e datas idênticos aos do e-mail.
- Próximo passo e prazo claros, ou `[CONFIRMAR: ...]`.

## Veto Conditions

Refazer se:
1. Houver data, BL, container ou valor que não aparece no Aura nem no e-mail.
2. Houver promessa de prorrogação ou retirada sem `[CONFIRMAR: ...]`.
3. Houver assunto ou assinatura no texto.
4. Pedido de informação respondido sem a informação nem `[CONFIRMAR: ...]`.
5. Faltar a primeira linha `AURA: encontrado|parcial|ausente|indisponivel`.
