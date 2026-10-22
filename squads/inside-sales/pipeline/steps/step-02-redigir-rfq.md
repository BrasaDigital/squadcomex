---
execution: inline
agent: caio-cotacao
inputFile: squads/inside-sales/output/{processo}/pedido.md
outputFile: squads/inside-sales/output/{processo}/cotacoes.md
---

# Step 02: Redigir Solicitações de Cotação

## Context Loading

Load these files before executing:
- `squads/inside-sales/output/{processo}/pedido.md` — dados confirmados do pedido (modal, origem/destino, peso/volume, incoterm).
- `pipeline/data/anti-patterns.md` — erros a evitar ao redigir RFQ (ex.: recomendar só por preço vem depois, mas RFQ também não deve omitir dados).
- Status dos conectores de e-mail disponíveis nesta sessão (Gmail e/ou Microsoft 365).

## Instructions

### Process

1. Ler pedido.md e confirmar modal, origem/destino, peso/volume e tipo de carga.
2. Verificar qual conector de e-mail está conectado: se só um, usar esse; se mais de um, perguntar ao usuário qual usar; se nenhum, perguntar como proceder.
3. Redigir uma solicitação de cotação (RFQ) por fornecedor/agente/armador relevante ao modal, seguindo a task `redigir-rfq.md` do agente Caio Cotação.
4. Criar o rascunho (draft) real no conector escolhido — nunca enviar — e registrar em cotacoes.md o conteúdo e a referência do rascunho (destinatário, assunto, conector usado).
5. Marcar cada rascunho como "RASCUNHO — aguardando aprovação" — nada será enviado antes do checkpoint seguinte.

## Output Format

```
## Rascunho de solicitação de cotação (RASCUNHO — aguardando aprovação para envio)

**Para:** ...
**Assunto:** ...

[corpo da solicitação com todos os dados necessários para cotação precisa]
```

## Output Example

```
## Rascunho de solicitação de cotação (RASCUNHO — aguardando aprovação para envio)

**Conector usado:** Gmail (único conector de e-mail conectado nesta sessão)
**Para:** [Armador/Agente de carga a definir]
**Assunto:** Solicitação de cotação de frete marítimo — Shanghai → Santos
**Rascunho criado:** sim (draft no Gmail, não enviado)

Olá,

Solicitamos cotação de frete marítimo para o seguinte embarque:
- Origem: Shanghai, China
- Destino: Porto de Santos, SP
- Mercadoria: Peças automotivas (rolamentos de aço), carga geral não perigosa
- Peso/Volume: 2.400 kg / 6 m³ (40 caixas)
- Incoterm: FOB Shanghai
- Modal: Marítimo, LCL (a confirmar conforme consolidação)

Favor informar: valor do frete, prazo de trânsito, validade da cotação e condições de free time no destino.

Atenciosamente,
Atlas Log
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. O rascunho de RFQ não está marcado "RASCUNHO — aguardando aprovação".
2. Falta algum dado essencial do pedido na RFQ (origem, destino, peso/volume, incoterm).
3. Um e-mail foi enviado em vez de criado como rascunho, ou o conector foi escolhido sem checar se havia mais de um conectado.

## Quality Criteria

- [ ] Toda RFQ está marcada como pendente de aprovação.
- [ ] A RFQ contém todos os dados do pedido necessários.
- [ ] O rascunho foi criado no conector correto (único conectado, ou escolhido pelo usuário).
