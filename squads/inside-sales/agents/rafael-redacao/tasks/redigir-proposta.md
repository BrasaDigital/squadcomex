---
task: "Redigir Proposta"
order: 1
input: |
  - pedido_md: Dados do pedido aprovado
  - cotacoes_md: Oferta de frete recomendada e aprovada
  - estimativa_custos_md: Estimativa de custos aprovada (checkpoint b)
output: |
  - proposta_md: Rascunho da proposta consultiva, marcado como pendente de aprovação
---

# Redigir Proposta

Redige a proposta consultiva ao cliente a partir do dossiê aprovado, sem nunca apresentar como definitivo um valor ainda "a confirmar".

## Process

1. Ler pedido.md, cotacoes.md e estimativa-custos.md já aprovados.
2. Estruturar a proposta: contexto do pedido, escopo do serviço, condições comerciais (valores confirmados, itens pendentes sinalizados como "sujeitos a confirmação"), validade, próximos passos.
3. Definir prazo de validade da proposta e prazo esperado de retorno do cliente.
4. Marcar a proposta como rascunho pendente de aprovação (checkpoint c) antes do envio.

## Output Format

```yaml
proposta:
  cliente: "..."
  escopo: "..."
  condicoes_comerciais:
    - item: "..."
      valor: "..."
      status: "confirmado | sujeito a confirmacao"
  validade: "..."
  prazo_retorno: "..."
  status: "RASCUNHO - aguardando aprovacao"
```

## Output Example

> Use como referência de qualidade, não como template rígido.

```
# proposta.md — ATL-2026-0142 (RASCUNHO — aguardando aprovação para envio)

Olá [Cliente],

Com base no pedido de importação das peças automotivas (Shanghai → Santos), preparamos a proposta abaixo.

**Escopo:** Agenciamento de carga marítima (LCL) e suporte ao desembaraço aduaneiro, incluindo seguro internacional e acompanhamento documental.

**Condições comerciais:**
- Frete internacional + seguro: USD 930 (confirmado)
- Honorários Atlas Log (despacho): R$ 1.800 (confirmado)
- Armazenagem estimada (5 dias): R$ 950 (confirmado)
- Tributos de importação (II, IPI, PIS/COFINS, ICMS): estimativa sujeita à confirmação da classificação fiscal (NCM) final — valor será informado antes do fechamento

**Validade da proposta:** 10 dias corridos a partir do envio.
**Prazo de retorno esperado:** até 5 dias úteis.

Ficamos à disposição para esclarecer qualquer ponto.

Atenciosamente,
Equipe Atlas Log
```

## Quality Criteria

- [ ] Nenhum valor "a confirmar" na estimativa aparece como definitivo.
- [ ] A proposta tem validade e prazo de retorno explícitos.
- [ ] A proposta está marcada como rascunho pendente de aprovação.

## Veto Conditions

Reject and redo if ANY are true:
1. Um item marcado "a confirmar" na estimativa aparece como valor definitivo na proposta.
2. A proposta não tem validade ou prazo de retorno definidos.
