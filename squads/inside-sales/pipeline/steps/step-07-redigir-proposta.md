---
execution: inline
agent: rafael-redacao
inputFile: squads/inside-sales/output/{processo}/cotacoes.md
outputFile: squads/inside-sales/output/{processo}/proposta.md
---

# Step 07: Mensagem de Envio da Proposta

## Context Loading

- `pedido.md`, `cotacoes.md` e, se houver, `estimativa-custos.md`.
- O documento de proposta **gerado pelo Aura** (`listar_documentos` → `ler_documento`).
- `_opensquad/_memory/company.md` — tom de voz da Atlas Log.

## Instructions

A proposta em si é redigida pelo Aura. O papel de Rafael é:

1. Ler a proposta gerada no Aura e conferir que reflete pedido e cotações. Se faltar documento, pedir ao usuário que gere no Aura.
2. Redigir a **mensagem/e-mail de envio** consultiva que acompanha o documento do Aura (contexto, resumo das condições, validade, próximos passos), seguindo a task `redigir-proposta.md` e o tom da Atlas Log.
3. Sinalizar qualquer item "a confirmar" como "sujeito a confirmação".
4. Salvar em `proposta.md` com a marca "RASCUNHO — aguardando aprovação" e a referência do documento do Aura.
