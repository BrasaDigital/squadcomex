# Armadilhas conhecidas

- **CNPJ da PORT BRAZIL (07.759.948/0001-74) nas invoices do 26/348:** é a consignatária de encomenda, não a RCL. Cadastrar cliente com esse CNPJ está errado. (09/11/2026)
- **CNPJ com dígitos inválidos em e-mail:** o 49.598.704/0001-06 ("SM COMER") não passa nos dígitos verificadores; provável erro de digitação. Sempre conferir antes de usar. (09/11/2026)
- **Mesmo CNPJ, razão social diferente do nome do cliente:** 19.176.862/0001-06 a Receita mostra como SM MOBILIDADE LTDA (Curitiba), mas o usuário confirmou como o CNPJ da SM ADAPTAÇÃO (09/11/2026). Antes de cadastrar, confirmar a razão social oficial; não usar o nome da Receita para "corrigir" o cliente sem perguntar.
- **Pessoa x empresa:** "Divina Cleia Resende" é a responsável da BELAMANDA; "Camila Sartori" aparece em processo da Marimex e não implica que Sartori seja cliente com o mesmo CNPJ. Cadastrar a empresa, não a pessoa. (09/11/2026)
- **Valor de compra ignorado em silêncio:** no 26/358 o recibo DHL (COMPRA.pdf) foi lido para ETA, peso e incoterm, mas o valor (Fator de Faturação 66,75 USD) não virou taxa nem dúvida. Documento de fornecedor sempre gera proposta de taxa de compra ou dúvida. (09/11/2026)
- **Cadastro sem CNPJ:** gerou retrabalho (4 clientes sem CNPJ em 09/11/2026) porque o conector não editava empresa depois. Procurar o CNPJ ANTES de cadastrar. Atualização 10/11/2026: o conector passou a ter `editar_empresa`; cadastro sem CNPJ agora se completa por proposta no painel (ver `receitas.md`).
- **Duplicar cliente com outro nome comercial:** BRUTOS era a LOJAESA LTDA (confirmado pelo usuário em 09/11/2026) e foi cadastrado como cliente novo; o cadastro BRUTOS sobrou no Aura (desde 10/11/2026 dá para resolver com `mesclar_empresas`, por proposta no painel). Antes de cadastrar cliente novo, procurar por CNPJ no Aura (prévia do `cadastrar_empresa` diz se já existe) e nas pastas antigas do mesmo cliente.
- **Limite de 35 embarques/mês do plano:** `criar_processo` é recusado ao confirmar mesmo com a prévia conferida (26/321 e mais 2 em 06/11/2026). Não insistir; avisar o usuário (upgrade ou embarque bônus com o suporte).
- **FCL não aceita linhas de volume:** peso e cubagem de FCL não gravam por `atualizar_volumes_carga`/`atualizar_carga_fisica` (26/218, 263, 308, 309, 310). Registrar como informação, não insistir (ver MEL-01).

## Melhorias recusadas
- **MEL-10** (Taxas de compra: documento com validade ao conferir + despesa no financeiro do processo): usuário clicou "Discordo" em 09/11/2026 20:30, sem informar motivo. Não propor de novo sem evidência nova.
- **MEL-11** (Despesas por processo: informar a taxa de câmbio usada no pagamento): usuário clicou "Discordo" em 09/11/2026 20:30, sem informar motivo. Não propor de novo sem evidência nova.
- **Parênteses no nome do fornecedor (09/11/2026):** `adicionar_taxas` com `parceiro_compra` "DHL EXPRESS (BRAZIL) LTDA" falhou ("DHL EXPRESS BRAZIL LTDA" não encontrada), embora a empresa exista com parênteses (26/344). O conector normaliza o nome. Item 26/358 ficou `falhou`; não repetir sem decisão do usuário.
- **Cliente por trás do despachante (11/11/2026):** FYNEX é despachante aduaneiro e os documentos trazem o cliente final dele; para a Atlas o cliente do processo é a FYNEX (resposta do usuário, 26/357). Não perguntar de novo o cliente final nesses casos.
