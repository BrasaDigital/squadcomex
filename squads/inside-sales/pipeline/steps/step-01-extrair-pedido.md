---
execution: inline
agent: pedro-pedido
outputFile: squads/inside-sales/output/{processo}/pedido.md
---

# Step 01: Extrair Pedido

## Context Loading

Load these files before executing:
- Pedido do cliente (texto/e-mail/mensagem/arquivo) fornecido pelo usuário — conteúdo bruto a ser extraído.
- Pasta do processo informada pelo usuário (cliente + referência) — define onde o dossiê será criado.
- `pipeline/data/domain-framework.md` — base legal/técnica de custos e regra de pendências.

## Instructions

### Process

1. Ler o pedido do cliente e confirmar/criar a pasta do dossiê (cliente + referência do processo).
2. Extrair todos os campos obrigatórios (produto, origem, destino, quantidade/peso/volume, incoterm, modal, tipo de carga), sugerir NCM provável e avaliar exigência de anuências, seguindo exatamente o processo da task `extrair-pedido.md` do agente Pedro Pedido.
3. Identificar o escopo: **cotação simples** (só frete/taxas) ou **com desembaraço** (Atlas cuida do desembaraço, o que habilita a estimativa). Se o cliente não informou, perguntar e registrar como pendência.
4. Para cada lacuna encontrada, redigir uma pergunta objetiva ao cliente e registrar em pendencias.md com responsável e área.

## Output Format

```
# pedido.md

**Cliente:** ...
**Referência do processo:** ...
**Escopo:** cotação simples | com desembaraço
**Data de recebimento:** ...

## Dados do pedido
- **Produto:** ...
- **NCM provável:** ... (a confirmar)
- **Origem:** ...
- **Destino:** ...
- **Quantidade/Peso/Volume:** ...
- **Incoterm:** ...
- **Modal:** ...
- **Tipo de carga:** ...
- **Anuências:** ...

## Status
Pedido completo | Pedido incompleto — ver pendencias.md
```

## Output Example

```
# pedido.md

**Cliente:** Comercial Vitória Imports Ltda
**Referência do processo:** ATL-2026-0142
**Data de recebimento:** 2026-10-20

## Dados do pedido
- **Produto:** Peças de reposição automotivas (rolamentos de aço)
- **NCM provável:** 8482.10.10 (a confirmar com despachante)
- **Origem:** Shanghai, China
- **Destino:** Porto de Santos, SP
- **Quantidade/Peso/Volume:** 2.400 kg / 6 m³ / 40 caixas
- **Incoterm:** FOB Shanghai
- **Modal:** Marítimo (FCL/LCL a definir conforme volume consolidado)
- **Tipo de carga:** Carga geral, não perigosa
- **Anuências:** Nenhuma identificada para este NCM (confirmar na classificação final)

## Status
Pedido completo. Nenhuma pendência crítica. Pronto para cotação.
```

## Veto Conditions

Reject and redo if ANY of these are true:
1. Um campo obrigatório está vazio em pedido.md sem estar listado em pendencias.md.
2. O NCM aparece sem a marcação "a confirmar".

## Quality Criteria

- [ ] Todos os campos obrigatórios estão preenchidos ou listados como pendência explícita.
- [ ] Toda possível exigência de anuência foi avaliada e documentada.
- [ ] pendencias.md está consistente com pedido.md.
