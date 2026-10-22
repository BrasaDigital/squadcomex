---
task: "lancar-no-aura"
agent: "aurelio-aura"
inputs:
  - pedido.md, cotacoes.md, estimativa-custos.md, proposta.md, pendencias.md
outputs:
  - aura-lancamento.md
---

# Lançar e manter o processo no AuraComex (incremental)

Os documentos (proposta, estimativa, numerário, prestação de contas) são redigidos pelo próprio Aura a partir do que lançamos; esta task nunca os redige. A estimativa só existe no escopo com desembaraço (numerário e prestação de contas são da fase de desembaraço, fora deste squad). Esta task roda em várias fases (inicial, cotacoes, estimativa, proposta, status), logo após cada etapa que gera dado novo. O Aura é preenchido aos poucos: cada fase lança só o que o dossiê já tem e completa o que estava pendente. `aura-lancamento.md` é o registro do que já está no Aura e evita duplicidade.

## Process

1. **Conferir o conector.** Se as ferramentas do AuraComex não estiverem disponíveis na sessão, parar e avisar o usuário; não simular lançamento.
2. **Buscar duplicidade** pela referência do processo e pelo cliente. Se existir, perguntar se atualiza o existente ou cancela.
3. **Empresas.** Garantir que cliente, fornecedor(es) e embarcador existam; se não, perguntar o tipo e usar `cadastrar_empresa` (com CNPJ quando houver).
4. **Criar o processo** (`criar_processo`, sempre com `referencia_externa` = ID do dossiê, ex.: ATL-2026-0142; se a resposta vier com `ja_existia: true`, não crie outro e siga com a referência oficial do Aura devolvida) com os dados de `pedido.md` (cliente, modal, origem, destino, incoterm, embarcador/consignatário). Mostrar a prévia e aguardar aprovação.
5. **Carga.** `adicionar_carga` por item (mercadoria, NCM marcado como provável, quantidade, valor da invoice por item). Depois `atualizar_volumes_carga` (quantidade, tipo, medidas, peso por unidade) e `atualizar_carga_fisica` (pallets, caixas, tipo de volume, valor total da invoice). Só com dados presentes no dossiê.
6. **Taxas da cotação.** `adicionar_taxas` com as taxas do `cotacoes.md` (frete recomendado e demais) e da proposta aprovada.
7. **Estimativa de custo.** `atualizar_estimativa` com `estimativa-custos.md` (requer perfil de acesso total; se negado, registrar como pendência).
8. **Observação.** `adicionar_observacao` resumindo a origem do lançamento (squad Inside Sales), a data e os itens "A confirmar".
9. **Conferir** relendo (`buscar_processo`, `listar_carga`, `listar_taxas`, `ver_estimativa`) e gerar `aura-lancamento.md`.

Cada gravação: primeiro sem confirmar, mostrar "como era / como vai ficar", e só então `confirmar: true`.

## Output Format

```
# aura-lancamento.md
**Processo Aura:** <nº/ID> | **Referência:** ...
## Lançado
- Processo: ok | Carga: N itens | Volumes: ok | Taxas: N | Estimativa: ok
## Fora do lançamento / pendências
- <item> — motivo
```

## Veto Conditions
1. Gravação sem prévia aprovada.
2. Processo duplicado criado.
3. Dado inventado que não consta no dossiê.
