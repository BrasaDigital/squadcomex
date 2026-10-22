---
task: "Redigir RFQ"
order: 1
input: |
  - pedido_md: Dados confirmados do pedido (produto, origem/destino, peso/volume, incoterm, modal)
  - conector_email: Conector de e-mail conectado nesta sessão (Gmail e/ou Microsoft 365), detectado em tempo de execução
output: |
  - cotacoes_md: Seção de rascunhos de solicitação de cotação em cotacoes.md, marcados como pendentes de aprovação, com link/referência do rascunho de e-mail criado
  - email_draft: Um rascunho (draft) criado no conector de e-mail, por fornecedor/agente/armador — nunca enviado
---

# Redigir RFQ

Redige, a partir dos dados confirmados em pedido.md, uma solicitação de cotação (RFQ) por fornecedor/agente/armador relevante ao modal do processo, e cria o rascunho real no conector de e-mail disponível — nunca apenas como texto solto.

## Process

1. Ler pedido.md para confirmar modal, origem/destino, peso/volume e tipo de carga.
2. Verificar qual conector de e-mail está conectado nesta sessão:
   - Se só um estiver conectado (Gmail ou Microsoft 365), usar esse, sem perguntar.
   - Se mais de um estiver conectado, perguntar ao usuário qual usar.
   - Se nenhum estiver conectado, perguntar ao usuário como prefere proceder (conectar um agora, ou seguir apenas com o texto em cotacoes.md, sem criar rascunho real).
3. Redigir uma solicitação de cotação por fornecedor/agente/armador relevante, incluindo todos os dados necessários para uma cotação precisa.
4. Criar o rascunho (draft) no conector escolhido — nunca enviar o e-mail diretamente.
5. Registrar em `cotacoes.md` o conteúdo da RFQ e uma referência ao rascunho criado (destinatário, assunto, conector usado), marcando tudo como pendente de aprovação (checkpoint a) antes de qualquer envio.

## Output Format

```yaml
cotacoes:
  rascunhos_rfq:
    - destinatario: "..."
      assunto: "..."
      corpo: "..."
      conector: "gmail | microsoft365"
      status: "RASCUNHO - aguardando aprovacao"
```

## Output Example

> Use como referência de qualidade, não como template rígido.

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

## Quality Criteria

- [ ] Toda RFQ está marcada "RASCUNHO — aguardando aprovação".
- [ ] A RFQ contém todos os dados do pedido necessários para uma cotação precisa.
- [ ] Nenhuma RFQ é enviada automaticamente — fica em rascunho (draft) no conector até o checkpoint.
- [ ] O conector usado foi determinado corretamente (único conectado, ou escolhido pelo usuário quando há mais de um).

## Veto Conditions

Reject and redo if ANY are true:
1. O rascunho de RFQ não está marcado como pendente de aprovação.
2. Falta algum dado essencial do pedido (origem, destino, peso/volume, incoterm) na RFQ.
3. Um e-mail foi enviado (em vez de criado como rascunho) ou um conector foi escolhido sem verificar antes se havia mais de um conectado.
