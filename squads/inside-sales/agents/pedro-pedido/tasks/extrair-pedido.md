---
task: "Extrair Pedido"
order: 1
input: |
  - pedido_cliente: Texto, e-mail, mensagem ou arquivo com o pedido do cliente
  - pasta_processo: Caminho/nome da pasta do processo informado pelo usuário (cliente + referência)
output: |
  - pedido_md: Arquivo pedido.md com todos os dados extraídos e confirmados
  - pendencias_md: Arquivo pendencias.md atualizado, se houver dados faltantes
---

# Extrair Pedido

Lê o pedido bruto do cliente e a pasta do processo informada, extrai os dados operacionais obrigatórios, sugere NCM provável, avalia exigência de anuências e lista toda pendência encontrada.

## Process

1. Ler o pedido do cliente e localizar/criar a pasta do dossiê (cliente + referência do processo).
2. Extrair campos obrigatórios: produto/mercadoria, origem e destino, quantidade/peso/volume, incoterm, modal (marítimo/aéreo/rodoviário), tipo de carga (geral, perigosa, perecível, granel, etc.).
3. Sugerir NCM provável com base na descrição da mercadoria, sempre marcado como "a confirmar".
4. Verificar se o produto pode exigir anuência de órgão (ANVISA, MAPA, IBAMA, ANP, Exército, etc.); em caso de dúvida, listar como pendência, nunca assumir que não precisa.
5. Listar todos os campos obrigatórios ausentes ou ambíguos e redigir uma pergunta objetiva por item para o cliente.
6. Escrever `pedido.md` com todos os dados extraídos e confirmados.
7. Se houver qualquer pendência, escrever/atualizar `pendencias.md` (o que falta, quem pode responder, para qual área) e sinalizar que o avanço daquele item fica bloqueado até a resposta.

## Output Format

```yaml
pedido:
  cliente: "..."
  referencia_processo: "..."
  data_recebimento: "YYYY-MM-DD"
  produto: "..."
  ncm_provavel: "... (a confirmar)"
  origem: "..."
  destino: "..."
  quantidade_peso_volume: "..."
  incoterm: "..."
  modal: "maritimo | aereo | rodoviario"
  tipo_carga: "..."
  anuencias: "..."
  status: "completo | incompleto"
pendencias:
  - item: "..."
    falta: "..."
    responde: "cliente | fornecedor | area_interna"
    area: "..."
```

## Output Example

> Use como referência de qualidade, não como template rígido.

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

Para um pedido incompleto, a saída inclui a mesma estrutura com os campos faltantes explícitos e a tabela correspondente em `pendencias.md` (item, o que falta, quem responde, área).

## Quality Criteria

- [ ] Todos os campos obrigatórios aparecem preenchidos ou explicitamente marcados como pendência.
- [ ] NCM está sempre marcado "a confirmar".
- [ ] Toda exigência de anuência potencial foi avaliada (confirmada, descartada com justificativa, ou pendente).
- [ ] pendencias.md está consistente com os campos vazios de pedido.md.

## Veto Conditions

Reject and redo if ANY are true:
1. Um campo obrigatório (produto, origem, destino, quantidade/peso/volume, incoterm, modal) está vazio em pedido.md sem estar listado em pendencias.md.
2. O NCM aparece sem a marcação "a confirmar".
