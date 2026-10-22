# Output Examples — Inside Sales Comex

## Exemplo 1 — Dossiê completo, sem pendências críticas

Ver `agents/pedro-pedido/tasks/extrair-pedido.md` (exemplo "Pedido completo de importação marítima") para o formato de `pedido.md`, e os exemplos equivalentes em `agents/caio-cotacao/tasks/comparar-ofertas.md`, `agents/estela-estimativa/tasks/montar-estimativa.md` e `agents/rafael-redacao/tasks/redigir-proposta.md` para `cotacoes.md`, `estimativa-custos.md` e `proposta.md` do mesmo processo (ATL-2026-0142).

## Exemplo 2 — Dossiê com pendências abertas

Ver o exemplo "Pedido incompleto — faltam dados" em `agents/pedro-pedido/tasks/extrair-pedido.md` (processo ATL-2026-0143), que mostra como `pendencias.md` é gerado e como as etapas seguintes devem sinalizar os valores "a confirmar" em vez de assumir dados.

## Exemplo 3 — Revisão reprovada e devolução

Ver o exemplo "Veredito reprovado" em `agents/vitor-veredito/tasks/revisar-dossie.md`, que mostra como uma inconsistência entre `proposta.md` e `cotacoes.md`/`estimativa-custos.md` é apontada com arquivo e trecho exatos antes de devolver ao Rafael Redação.
