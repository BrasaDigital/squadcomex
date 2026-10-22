# Domain Framework — Inside Sales Comex (Atlas Log)

## Fluxo operacional

1. **Recepção do pedido** — cliente envia pedido (texto/e-mail/arquivo); usuário informa a pasta do processo (dossiê).
2. **Extração e checagem de completude** — dados obrigatórios: produto, origem, destino, quantidade/peso/volume, incoterm, modal, tipo de carga, NCM provável, anuências. Qualquer lacuna vira pendência, nunca suposição.
3. **Cotação** — solicitações de frete a fornecedores/agentes/armadores (rascunho → checkpoint de aprovação → envio manual pelo usuário → comparação das ofertas recebidas → recomendação justificada).
4. **Estimativa de custo total** — soma de valor da mercadoria, frete, seguro, valor aduaneiro, tributos (II, IPI, PIS/COFINS-Importação, ICMS), despachante e armazenagem. Toda alíquota sem confirmação vira pendência com a fonte oficial indicada, nunca um número inventado.
5. **Checkpoint de estimativa** — usuário aprova antes de seguir para a proposta.
6. **Proposta consultiva** — redigida com valores confirmados e ressalvas explícitas sobre itens "sujeitos a confirmação", validade e prazo de retorno.
7. **Revisão de consistência** — conferência cruzada entre pedido, cotações, estimativa e proposta antes do envio.
8. **Checkpoint de envio da proposta** — última aprovação humana antes do envio ao cliente.
9. **Acompanhamento da decisão** — follow-up com prazos, atualização de `status-decisao.md`.

## Base legal/técnica de custos (Brasil)

- Valor aduaneiro = FOB + frete internacional + seguro até o porto/aeroporto de descarga (Regulamento Aduaneiro, Decreto 6.759/2009, art. 77).
- Imposto de Importação incide sobre o valor aduaneiro; IPI incide sobre valor aduaneiro + II.
- PIS/COFINS-Importação: alíquota combinada tipicamente entre 2,1% e 9,65% sobre o valor aduaneiro, variando por NCM.
- ICMS: alíquota estadual, varia por UF de destino.
- Em incoterm CIF, frete e seguro entram na base de cálculo dos tributos; em FOB, o frete é custo direto do importador e sua composição na base segue a mesma regra do Regulamento Aduaneiro.
- Nenhuma alíquota deve ser estimada pelo squad sem confirmação — sempre citar a fonte oficial (Receita Federal / legislação estadual) quando pendente.

## Regra de pendências (transversal a todos os agentes)

Se faltar qualquer informação, o agente NUNCA assume nem inventa. Registra em `pendencias.md` (o que falta, quem pode responder, para qual área) e interrompe o avanço daquele item específico até a resposta chegar.

## Estrutura fixa do dossiê

Cada processo tem pasta própria (cliente + referência) com: `pedido.md`, `cotacoes.md`, `estimativa-custos.md`, `proposta.md`, `status-decisao.md`, `pendencias.md`. O formato é fixo porque os squads de Pré-Embarque, Pós-Embarque, Desembaraço e Financeiro leem o mesmo dossiê.
