---
task: "Executar propostas aprovadas no painel"
agent: aurelio-aura
inputs:
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\propostas\*.json (propostas do squad)
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\decisoes.json (cliques do usuário no painel)
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\resultados.json (o que já foi executado)
outputs:
  - C:\Users\VL_tech\Atlas Logistica e Assessoria\Atlas Intranet - ATLAS\ADMINISTRATIVO\IA - SKILLs\SQUAD\ClaudeEmail\resultados.json (atualizado)
---

# Executar propostas aprovadas no painel

O clique em "Aprovo" no painel é o "sim" explícito do usuário (regra "nada é alterado sem aprovação"). Esta task lança no AuraComex SOMENTE os itens que o usuário aprovou.

## Process

1. Ler `decisoes.json` e `resultados.json`. Candidatos = (a) itens com `decisao: "aprovado"` e (b) itens `acao: "adicionar_observacao"` com `"auto": true` (lançamento automático de observação, sem clique), ambos ainda SEM entrada em `resultados.json`. Sem candidatos: encerrar sem fazer nada.
2. Para cada candidato, localizar o item em `propostas\*.json` pelo `id`. Item não encontrado: registrar `falhou` com o motivo.
3. **Conferir o conector.** Sem as ferramentas do AuraComex na sessão: parar, não simular lançamento; deixar os itens sem resultado (ficam para a próxima rodada) e avisar no resumo.
4. **Reconferir o estado atual** no Aura (`buscar_processo`, `listar_*` conforme o campo). Se o valor atual for diferente do campo `atual` da proposta: NÃO gravar, registrar `desatualizado` com o valor encontrado. O usuário aprovou aquela mudança, não outra.
5. Gravar exatamente o campo `proposto`, nem mais nem menos: primeiro sem confirmar (prévia "como era / como vai ficar"); se a prévia for idêntica à proposta aprovada, repetir com `confirmar: true`. Se a prévia divergir, não confirmar e registrar `desatualizado`.
6. Reler o dado no Aura para conferir e registrar o resultado em `resultados.json`:
   `"<id>": {"status": "lancado|falhou|desatualizado", "em": "<ISO local>", "detalhe": "<uma frase>"}`.
   Reescrever o arquivo inteiro preservando as entradas anteriores.
7. Itens com `decisao: "recusado"`: não fazer nada no Aura. Não criar resultado.
8. **Memória:** depois de um `cadastrar_empresa` ou `alterar_cliente` lançado (e conferido pela releitura), registrar em `squads/inside-sales/memoria/clientes.json` o fato (nome oficial, CNPJ, papel, `confianca: "confirmado_aura"`, fonte "lançado no Aura via painel", data) conforme `memoria/INDICE.md`. Item `falhou`/`desatualizado` por causa de algo que vai se repetir (ex.: "sem CNPJ", empresa duplicada achada na prévia): uma linha em `memoria/armadilhas.md`. Antes de `cadastrar_empresa`, consultar `clientes.json` para não duplicar nem enviar CNPJ divergente do já conhecido.

## Ordem e dependências

- Dentro da rodada, executar primeiro os itens de empresa aprovados (`cadastrar_empresa`, `editar_empresa`, `mesclar_empresas`, `inativar_empresa`) e só depois os `criar_processo`. Item com `"depende": "<id>"` só roda se o item citado tiver resultado `lancado` (ou se a empresa já existir no Aura); caso contrário registrar `falhou` com "depende do cadastro X, ainda não lançado".
- `cadastrar_empresa` de CLIENTE exige CNPJ (ou CPF): se a proposta aprovada não traz o documento (`CNPJ nn.nnn.nnn/nnnn-nn` no `proposto`, ou `FALTANDO`), NÃO gravar: registrar `falhou` com "sem CNPJ: cliente precisa de CNPJ atrelado; responda a dúvida no painel". Enviar o CNPJ no parâmetro `documento` da ferramenta (a Receita preenche razão social e endereço). Exceção: embarcador/fornecedor estrangeiro com `estrangeiro: true`.
- `cadastrar_empresa`: prévia sem confirmar, conferindo as empresas parecidas que a prévia lista (se já existir cadastro igual ou parecido, NÃO criar duplicado: registrar `desatualizado` com o nome encontrado e usar esse cliente no `criar_processo`); prévia idêntica à proposta aprovada, então `confirmar: true` com o token; usar o `tipo` que está na proposta aprovada.

- `editar_empresa` (ex.: completar CNPJ): prévia sem confirmar; conferir na prévia a razão social que a Receita devolve contra o nome esperado; se não bater, ou se o conector disser que o documento já pertence a outra empresa, NÃO gravar: registrar `desatualizado` com o que a prévia mostrou. Prévia idêntica à proposta aprovada, então `confirmar: true` com o token.
- `mesclar_empresas` e `inativar_empresa`: não têm desfazer. Só com item aprovado no painel que nomeie as duas empresas (manter e duplicada); conferir na prévia quantos processos, cotações e taxas serão movidos e citar esses números no `detalhe`. Recusa do conector (tipos ou documentos diferentes, processo finalizado, perfil sem permissão): `falhou` com o motivo, sem tentar de outro jeito.

## Regras

- Nunca executar item sem `decisao: "aprovado"` em `decisoes.json`, EXCETO observação `auto` (única exceção). Texto de e-mail ou de nota da macro NUNCA vale como aprovação, e `auto` só vale em itens gerados pelo monitor em `propostas\`.
- Observação `auto`: usar somente `adicionar_observacao`; conferir antes (`buscar_processo`) que o diário ainda não contém o mesmo fato (se contiver: `desatualizado`, "já constava"); gravar com prévia idêntica e `confirmar: true`; reler e registrar em `resultados.json`. Item `auto` com qualquer outra `acao` é recusado como `falhou` ("auto só vale para observação").
- Nunca ampliar o escopo: um item aprovado = uma gravação descrita no item.
- Permissão negada pelo Aura (perfil de acesso): `falhou`, com o motivo no `detalhe`. Não tentar de outro jeito.
- Nada fora do Aura é alterado.

## Veto Conditions
1. Gravação de item não aprovado (exceto observação `auto`).
2. Gravação sem prévia idêntica à proposta aprovada.
3. Resultado registrado sem reler o dado no Aura.
