---
task: "Revisar Dossiê"
order: 1
input: |
  - pedido_md: pedido.md do processo
  - cotacoes_md: cotacoes.md do processo
  - estimativa_custos_md: estimativa-custos.md do processo
  - proposta_md: proposta.md do processo
  - pendencias_md: pendencias.md do processo
output: |
  - veredito: APROVADO ou REPROVADO, com pontuação por critério e correções obrigatórias se houver
---

# Revisar Dossiê

Confere a consistência numérica e factual entre todos os arquivos do dossiê antes do checkpoint de envio da proposta.

## Process

1. Ler os cinco arquivos do dossiê: pedido.md, cotacoes.md, estimativa-custos.md, proposta.md, pendencias.md.
2. Conferir que os valores citados na proposta batem exatamente com os valores confirmados em cotacoes.md e estimativa-custos.md.
3. Conferir que nenhum item marcado "a confirmar" na estimativa aparece como valor definitivo na proposta.
4. Conferir que toda pendência aberta em pendencias.md está refletida com ressalva na proposta ou foi resolvida antes.
5. Conferir consistência de incoterm, modal e rota entre proposta e pedido.md, e verificar se a proposta tem validade e prazo de retorno definidos.
6. Pontuar cada critério (1–10) com justificativa; aplicar a regra: APROVADO se média ≥ 7 e nenhum critério abaixo de 4; REPROVADO caso contrário.
7. Se REPROVADO, listar correções obrigatórias apontando arquivo e trecho exato, e devolver ao agente responsável.

## Output Format

```yaml
veredito:
  resultado: "APROVADO | REPROVADO"
  criterios:
    - nome: "..."
      nota: 0
      justificativa: "..."
  correcoes_obrigatorias:
    - arquivo: "..."
      trecho: "..."
      correcao: "..."
```

## Output Example

> Use como referência de qualidade, não como template rígido.

```
==============================
 VEREDITO: APROVADO
==============================
Processo: ATL-2026-0142

| Critério | Nota | Justificativa |
|---|---|---|
| Consistência numérica (proposta vs. estimativa/cotação) | 10/10 | Todos os valores batem exatamente |
| Nenhum item pendente apresentado como definitivo | 9/10 | Tributos corretamente sinalizados como "sujeito a confirmação" |
| Pendências refletidas com transparência | 8/10 | Pendência de NCM citada na proposta |
| Consistência de incoterm/modal/rota | 10/10 | Idêntico ao pedido.md |
| Validade e prazo de retorno definidos | 10/10 | 10 dias de validade, 5 dias úteis de retorno |

**Média: 9,4/10 — APROVADO.** Liberado para o checkpoint de envio ao cliente.
```

Exemplo de reprovação:

```
==============================
 VEREDITO: REPROVADO
==============================
Processo: ATL-2026-0143

Correção obrigatória: proposta.md cita "frete: USD 790" mas cotacoes.md registra a recomendação final como Agente C (USD 870). Corrigir o valor na proposta antes de reapresentar.

Correção obrigatória: estimativa-custos.md lista ICMS como "a confirmar", mas proposta.md apresenta o custo total sem ressalva sobre esse item. Adicionar a ressalva de "sujeito a confirmação" na proposta.

**VEREDITO: REPROVADO — devolvido para Rafael Redação.**
```

## Quality Criteria

- [ ] Todo valor citado na proposta foi conferido contra cotacoes.md e estimativa-custos.md.
- [ ] Nenhuma pendência aberta é omitida da proposta sem ressalva.
- [ ] O veredito é sempre explícito, com nota e justificativa por critério.

## Veto Conditions

Reject and redo if ANY are true:
1. Um valor na proposta não corresponde ao valor confirmado em cotacoes.md ou estimativa-custos.md.
2. Uma pendência aberta em pendencias.md não está refletida na proposta.
