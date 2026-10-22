---
task: "Comparar Ofertas"
order: 2
input: |
  - ofertas_recebidas: Ofertas de frete/condições trazidas pelo usuário após o envio das RFQs aprovadas
  - pedido_md: Dados do pedido, para contexto de modal e volume
output: |
  - cotacoes_md: Tabela comparativa de ofertas e recomendação justificada em cotacoes.md
  - pendencias_md: Pendências, se alguma oferta estiver incompleta para comparação justa
---

# Comparar Ofertas

Registra as ofertas recebidas de fornecedores/agentes/armadores e recomenda a melhor opção com justificativa explícita.

## Process

1. Registrar cada oferta recebida em tabela comparativa: frete, prazo/transit time, validade da cotação, condições (free time, demurrage, seguro incluso ou não).
2. Recomendar a melhor oferta com justificativa explícita, considerando prazo, validade e confiabilidade, não só preço.
3. Se faltar informação em alguma oferta (ex.: validade não informada), registrar em pendencias.md e seguir com o que está disponível, sinalizando a lacuna.
4. Escrever/atualizar `cotacoes.md` com a tabela e a recomendação final.

## Output Format

```yaml
comparacao_ofertas:
  - fornecedor: "..."
    frete: "..."
    transit_time: "..."
    validade: "..."
    condicoes: "..."
recomendacao:
  fornecedor: "..."
  justificativa: "..."
```

## Output Example

> Use como referência de qualidade, não como template rígido.

```
## Comparação de ofertas — ATL-2026-0142

| Armador/Agente | Frete (USD) | Transit time | Validade | Condições |
|---|---|---|---|---|
| Agente A | USD 850 | 32 dias | 10 dias | 7 dias free time |
| Agente B | USD 790 | 38 dias | 5 dias | 5 dias free time |
| Agente C | USD 870 | 30 dias | 15 dias | 10 dias free time |

**Recomendação: Agente C.**
Justificativa: embora não seja o frete mais barato, oferece o menor transit time junto com a maior validade de cotação e o maior free time no destino — reduz risco de demurrage e dá mais margem para fechar a proposta com o cliente antes do vencimento da cotação. O Agente B tem o menor preço, mas a validade de 5 dias é arriscada dado o prazo de resposta do cliente.
```

## Quality Criteria

- [ ] A comparação usa no mínimo frete, prazo e validade como critérios.
- [ ] A recomendação tem justificativa explícita, considerando múltiplos critérios.
- [ ] Todas as ofertas recebidas aparecem na tabela, mesmo as não recomendadas.

## Veto Conditions

Reject and redo if ANY are true:
1. A recomendação é baseada apenas no menor preço sem considerar prazo/validade/condições.
2. Uma oferta recebida é omitida da tabela comparativa.
