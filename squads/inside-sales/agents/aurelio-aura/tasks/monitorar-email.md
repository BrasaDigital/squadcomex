---
task: "monitorar-email"
agent: "aurelio-aura"
inputs:
  - _memory/email-cursor.md
  - output/*/aura-lancamento.md, pedido.md, pendencias.md
outputs:
  - output/_inbox/{AAAA-MM-DD}-{HHhMM}.md
  - _memory/email-cursor.md (atualizado)
---

# Monitorar e-mail e propor alterações no Aura

Modo atual: **somente propor** (treinamento). Nunca chamar ferramentas de escrita do AuraComex nesta task. **Regra permanente:** nada é alterado (Aura, pastas, e-mails, dossiês) sem pedir aprovação ao usuário e receber um "sim" explícito.

## Process

1. **Conferir conectores.** Precisa do conector de e-mail (Outlook/Gmail da sessão) e do AuraComex (só leitura). Se algum faltar, registrar no relatório e parar; não simular.
2. **Ler o cursor** em `_memory/email-cursor.md`. Janela = (`ultima_verificacao`, agora]. Sem cursor: início do dia.
3. **Buscar e-mails** da janela, lidos e não lidos (caixa de entrada e, se disponível, enviados, para ver o contexto das threads).
4. **Listar processos abertos** a partir de `output/*/`, `listar_processos` do Aura e das subpastas `NN-NNN - ...` de `C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\PROCESSOS\` (ignorar `FINALIZADOS`, `METROPOLY` e pastas fora do padrão).
4b. **Varrer as pastas de processos** (somente leitura): listar arquivos recursivamente, comparar com `_memory/pastas-snapshot.md` (caminho, tamanho, data de modificação) e separar novos/alterados/removidos. Na primeira rodada (sem snapshot), só gravar o snapshot-base. Ler o conteúdo dos arquivos novos/alterados (PDF, imagem, planilha) com a ferramenta de leitura e extrair dados como no passo 6. Confiança máxima "média" (remetente desconhecido, veio do WhatsApp/equipe).
4c. **Vínculo por documentos:** para cada e-mail/arquivo, cruzar invoice, PL, BL, container, booking, NF/DUIMP com pastas ativas, `PROCESSOS\FINALIZADOS\` (somente leitura) e Aura (incl. finalizados). Referência diferente = processo diferente. Só dizer "não encontrado" listando o que foi buscado e onde.
5. **Classificar** cada e-mail: relacionado a processo X / não relacionado.
6. **Extrair** só o que está escrito no e-mail: status, datas (ETD/ETA), valores e taxas, dados de carga, decisão do cliente, documentos anexados, respostas a pendências.
7. **Comparação tripla E-mail × Pastas × Aura** (Aura só leitura), por processo e por campo/documento. Classificar: consistente / divergente / só em uma fonte / sem informação. Montar a proposta com o que diz cada fonte, valor proposto e confiança (alta/média/baixa). Se as três já concordam, não propor. Em divergência, listar as três versões e perguntar ao usuário qual vale; nunca decidir sozinho.
8. **Escrever o relatório** em `output/_inbox/{AAAA-MM-DD}-{HHhMM}.md`.
9. **Atualizar o cursor** com o horário de início desta rodada (para não perder e-mails que chegarem durante a execução) e **regravar `_memory/pastas-snapshot.md`** com o estado das pastas lido nesta rodada.
10. Resumir ao usuário: quantas propostas, de quais processos, e o que exige decisão.

## Output Format

```
# Rodada {AAAA-MM-DD} {HH:MM} — janela {início} → {fim}

## Resumo
- E-mails analisados: N | Relacionados: N | Propostas: N

## Propostas de alteração no Aura
### Processo {ref}
| # | Campo | E-mail diz | Pasta diz | Aura diz | Status | Proposta (como ficaria) | Confiança |
|---|-------|-----------|-----------|----------|--------|--------------------------|-----------|

## Pendências respondidas
## Precisa da sua decisão
## Não relacionados (remetente — assunto)
```
