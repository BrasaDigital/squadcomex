# Passo: auditoria completa dos processos ATIVOS ("pente fino")

Objetivo: deixar o AuraComex completo e correto para cada processo ATIVO, cruzando os 3 pilares (E-mail, Pastas, AuraComex). O Aura é a base de dados.

## Escopo (decidido pelo usuário em 05/11/2026)
- SOMENTE processos ATIVOS de 2026: os que estão no Aura com status em andamento (não finalizado/cancelado) e os que têm pasta ativa em PROCESSOS\ (fora de FINALIZADOS) ou e-mail recente tratando como em andamento.
- NÃO tocar em FINALIZADOS: não ler, não propor nada, não criar processo para eles. Se um e-mail antigo tratar de processo finalizado, ignore.
- E-mails: desde 01/01/2026, apenas os ligados aos processos ativos (busque por referência 26/NNN, 26-NNN, HBL, MBL, container, DI/DUIMP). Anexos via read_resource.

## Regras
1. Fase 1 é SOMENTE LEITURA. Nenhuma gravação no Aura, nas pastas, em e-mails ou dossiês. Escritas permitidas: painel_auditoria.js, auditoria\ (checkpoint e relatório), propostas\, saida\ (perguntas internas) e output/_inbox/.
2. Regra de ouro do monitor-email vale aqui (leia step-monitor-email.md): propostas em JSON válido (barras invertidas dobradas), `origem` e `confianca` em cada item, `depende` para cadastrar_empresa antes de criar_processo, nunca propor criação sem modal.
3. Conflito entre fontes: NÃO sobrescreva. Proponha o valor mais confiável (documento oficial > e-mail do cliente > e-mail interno), cite as duas origens e marque confianca "baixa" quando não houver certeza.
4. Dado que não existe em nenhuma fonte: não invente. Se bloqueia o preenchimento, vira pergunta interna (checklist dos 3 pilares do step-monitor-email.md, consolidada por pessoa, sem repetir).
5. Observações factuais do diário podem ser `"auto": true`. Todo o resto exige clique do usuário.
6. Proposta agrupada por processo: todos os itens de um processo no mesmo arquivo, com o campo `processo` idêntico ("26/202"). O painel mostra um cartão por processo com "Aprovar tudo".

## Checklist de completude por processo
cliente; modal; direção (importação/exportação); referência; portos/aeroportos origem e destino; embarcador/consignatário; armador ou cia aérea; BL/AWB (MBL e HBL); containers; navio/voo; ETD/ETA e datas-chave; mercadoria, NCM e valor da invoice; volumes, peso, cubagem; DI/DUIMP; status coerente com a realidade; taxas/cotação e estimativa; documentos anexados no Aura (invoice, packing, BL) versus os da pasta.

## Execução
1. Inventário (Tiago): lista os processos ativos a partir do Aura (listar_processos), das pastas ativas e dos e-mails. Grave a lista em `ClaudeEmail\auditoria\inventario.json` e o total no progresso.
2. Para cada processo, em lotes de até 3 em paralelo (subagentes via Task/Agent), um subagente faz: Davi lê documentos da pasta e anexos; Olívia lê os e-mails; Fabio confere valores, taxas e estimativa; Aurélio compara com o Aura (buscar_processo, listar_carga, listar_taxas, listar_documentos, ver_estimativa) e monta as propostas.
3. Checkpoint: ao fechar cada processo, acrescente-o a `ClaudeEmail\auditoria\feitos.json` (lista). Se a rodada for reiniciada, pule os já feitos.
4. Relatório final em `output/_inbox/AAAA-MM-DD-auditoria.md` com a matriz processo x item (ok / faltando / conflito) e o que depende do usuário.

## Progresso para o painel (obrigatório)
Reescreva `C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\painel_auditoria.js` ao iniciar, ao começar e ao terminar cada processo (e a cada ~2 min em trabalho longo), com UMA linha:
`window.PAINEL_AUDIT={"ativo":true,"total":N,"feitos":n,"fase":"texto curto","atualizado":"AAAA-MM-DDTHH:MM:SS","atual":[{"agente":"davi","msg":"Lendo a pasta","proc":"26/202"}]};`
- `agente` ∈ tiago, fabio, olivia, davi, aurelio (o que estiver trabalhando agora, máximo 3).
- `atualizado` em hora local; o painel considera parado se passar de 10 min sem atualizar.
- Ao concluir: `"ativo":false` com feitos = total.

### Atualização do progresso a cada processo (reforço)
- O contador `feitos` NUNCA é estimado: recalcule-o contando as linhas de todos os `auditoria\parts\parte-*-feitos.txt` (cada linha = um processo concluído) mais os de `feitos.json`.
- Cada subagente, ao terminar CADA processo (não só o lote): (1) acrescenta o processo ao seu `parte-N-feitos.txt`; (2) relê `painel_auditoria.js`, recalcula `feitos`, troca em `atual` a sua entrada (agente, msg e proc do PRÓXIMO processo) e atualiza `atualizado`; (3) grava o arquivo inteiro de uma vez (uma linha).
- O orquestrador, ao receber o retorno de cada subagente, também recalcula `feitos` e reescreve o arquivo, sem esperar o lote inteiro.
- No início da rodada, se já existirem `parte-*-feitos.txt`, pule esses processos (retomada) e conte-os em `feitos`.
