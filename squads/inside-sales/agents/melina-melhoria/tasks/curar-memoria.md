---
task: "Curar a memória do squad"
agent: melina-melhoria
inputs:
  - squads/inside-sales/memoria/*
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\respostas.json
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\resultados.json
outputs:
  - squads/inside-sales/memoria/* (enxugado e consolidado)
---

# Curar a memória do squad

Disparo: junto do pedido de feedback da Melina (`pedir_melhorias.txt`) ou quando `memoria/INDICE.md` passar de 60 linhas.

## Passos
1. Ler `memoria/INDICE.md`, `clientes.json`, `receitas.md`, `armadilhas.md`, `hipoteses.md`.
2. **Duplicados:** unir registros da mesma empresa (mesmo CNPJ ou apelidos que se cruzam) em um só, mantendo todos os apelidos e a fonte mais forte.
3. **Hipóteses:** para cada uma, procurar em `respostas.json` e `resultados.json` se já foi confirmada ou negada. Confirmada: promover a `clientes.json` com a confiança certa e remover de `hipoteses.md`. Negada: mover para `armadilhas.md`. Mais de 30 dias sem evidência nova: listar para o usuário decidir, sem apagar.
4. **Conflitos:** dado em `clientes.json` que contradiz o Aura atual (conferir com `buscar_processo`, leitura): não corrigir sozinha; listar como dúvida a abrir no painel.
5. **Enxugar:** receitas e armadilhas repetidas viram um item só; manter o `INDICE.md` em até 60 linhas. Nunca apagar fato `confirmado_usuario`.
6. Medir: contar fatos por nível de confiança e quantos registros foram criados desde a última curadoria; registrar em `output/melhorias-aura.md` uma linha "Memória: N fatos (n1/n2/n3), N armadilhas, N hipóteses abertas".
7. Responder com resumo curto: o que unificou, promoveu, enxugou e o que precisa de decisão do usuário.

## Regras
- Critério antes de velocidade: para cada fato que for promover, confira a fonte (documento, dígitos verificadores, razão social, Aura em leitura) e anote como conferiu. Sem conferência completa, o fato fica como hipótese.
- Conservadora: unificar só com certeza (mesmo CNPJ válido). Nome parecido sem CNPJ igual NÃO unifica; vira dúvida.
- Escreve só em `memoria/` e na linha de métrica de `output/melhorias-aura.md`. SOMENTE LEITURA no Aura e nas pastas de processos.
