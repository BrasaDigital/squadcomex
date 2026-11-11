# Tarefa: propor melhorias para o AuraComex

Disparo: (a) botão "Pedir feedback" da Melina no painel (`pedir_melhorias.txt`); (b) proativo, ao fim de uma rodada completa quando surgiu evidência nova (falhou/desatualizado repetido, dúvida respondida, limitação do Aura ou do conector relatada pelo usuário). Sem evidência nova, não rodar.

## Passos
0. Leia `C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\melhorias_decisoes.json` (cliques do usuário no painel: `aprovado` com `enviado`, ou `discordo` com `motivo`) e `melhorias_cartoes.json` (cartões já mostrados). **Discordo:** não proponha de novo o mesmo item sem evidência nova; registre o motivo em `memoria/armadilhas.md` (seção "Melhorias recusadas") e marque o item como `recusado` no backlog. **Aprovado e enviado:** acompanhe; quando a evidência parar de aparecer, marque `status: "resolvido"` no cartão e no backlog.
1. Leia `output/melhorias-aura.md` (backlog atual; pode estar na versão inicial).
2. Leia também `output/pistas-melina.md` (pistas levantadas pelo squad e pelo usuário, com onde conferir): confira cada uma na fonte e só leve ao usuário o que se confirmar. Leia as evidências desde a última análise: `resultados.json` (status falhou e desatualizado, campo `detalhe`), relatórios de rodada em `ClaudeEmail\LOG\squad\rodadas\`, relatórios em `output/_inbox/` (monitor e auditoria), `ClaudeEmail\auditoria\parts\*-matriz.md` e `*-perguntas.json`.
3. Agrupe por causa raiz. Para cada causa: origem (Aura, conector AC ou processo do squad), nº de casos e ids/processos de exemplo, impacto (trabalho manual gerado, risco de dado errado), proposta e critério de aceite, esforço estimado (P/M/G).
4. Se necessário, confirme o comportamento com as ferramentas de LEITURA do Aura (buscar_processo, listar_carga, listar_taxas, listar_documentos, ver_estimativa). Nunca use ferramenta de escrita.
5. Atualize `output/melhorias-aura.md`: acrescente itens novos, atualize contagens dos existentes, marque como "possivelmente resolvido" o que parou de aparecer, mantenha a tabela de prioridade (frequência x esforço) no topo e a data da análise.
6. **Cartões no painel (o usuário abre clicando no seu nome na sala):** grave/atualize `C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\melhorias_cartoes.json` no formato `{"atualizado":"AAAA-MM-DD","itens":[{id,titulo,origem,prioridade,esforco,status,criado,problema,evidencia,impacto,proposta,criterio,prompt}]}`. Regras: no máximo 5 cartões abertos (sem decisão), por prioridade; nunca duplicar um id; nunca apagar cartão que o usuário já decidiu (só `status: "resolvido"`); sem evidência, sem cartão.
7. **O campo `prompt`** é o texto que o usuário cola no Claude que cuida do desenvolvimento do Aura, e esse Claude NÃO enxerga os arquivos do squad. Por isso deve ser autossuficiente: contexto do sistema (AuraComex da Atlas Log), problema e comportamento atual, comportamento esperado, exemplos reais (nº de processo, ids, valores), critério de aceite verificável, o que NÃO pode ser alterado, e a instrução de primeiro ler o código e apresentar o plano antes de alterar. Texto direto, sem referência a arquivos locais.
8. Seja atenciosa e criteriosa: confira cada evidência na fonte antes de levar um cartão ao usuário; na dúvida, não crie o cartão, registre em `hipoteses.md`.
9. Responda com resumo curto: itens novos, itens atualizados e os 3 de maior prioridade.

## Formato de cada item
### MEL-NN · título
- Origem: Aura | Conector AC | Processo do squad
- Problema:
- Evidência: (casos, ids, processos)
- Impacto:
- Proposta:
- Critério de aceite:
- Esforço: P | M | G · Prioridade: alta | média | baixa · Status: aberto | em andamento | resolvido
