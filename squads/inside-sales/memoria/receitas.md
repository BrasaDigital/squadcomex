# Receitas (o que já funcionou)

## Achar o CNPJ de um cliente
1. Memória: Grep em `clientes.json` pelo nome/apelido. Achou nível 1 a 3: use.
2. Documentos do processo e do e-mail: invoice/packing/proforma (campos "Buyer", "Co.", "TAX ID", "CNPJ"), CE/BL, DUIMP/DI (adquirente/importador), NF-e de saída (destinatário), certidões.
3. Pastas antigas do mesmo cliente em `PROCESSOS\FINALIZADOS\2024|2025|2026` (NF-e de saída, extrato da DI, averbação costumam ter o CNPJ do adquirente). Pesquisar pelo nome e por apelidos.
4. Conferir os DOIS dígitos verificadores do CNPJ e a razão social (ver "Conferir CNPJ na Receita").
5. Vários candidatos, divergência de nome ou nenhum achado: dúvida no painel (uma pergunta, candidatos com a origem de cada um). Nunca chutar.

## Conferir CNPJ na Receita sem gravar nada
`cadastrar_empresa` com só `documento` e sem confirmar devolve a razão social da Receita, ou "Já existe empresa com este documento: X" (e então a empresa já está no Aura). Prévia é somente leitura; não confirmar.

## Cliente cadastrado errado ou duplicado
- Trocar o cliente de um processo: `alterar_cliente` (prévia, mostrar, esperar aprovação, confirmar com token).
- Completar CNPJ, razão social ou contato de empresa já cadastrada: `editar_empresa` (completar documento vazio é sempre permitido; trocar documento já preenchido é recusado se a empresa tem processo ou cotação; com CNPJ novo a prévia mostra a razão social da Receita).
- Cadastro duplicado: `mesclar_empresas` (move processos, cotações e taxas para a empresa mantida e inativa a duplicada; NÃO tem desfazer; recusa se tipos ou documentos forem diferentes ou se a duplicada está em processo finalizado). Só tirar dos seletores: `inativar_empresa`. Mesclar e inativar exigem perfil admin, diretor, gerente ou superadmin.
- Tudo com prévia, proposta no painel e aprovação do usuário. (fonte: descrição das ferramentas do conector AC, lida em 10/11/2026; ainda sem caso lançado pelo squad)

## Cliente real x consignatária de encomenda
Em importação por encomenda o consignee dos documentos (ex.: PORT BRAZIL) não é o cliente. O cliente é o encomendante. Se não estiver claro, perguntar.

## Documento de fornecedor na pasta = taxa de compra (usuário, 09/11/2026)
Recibo, fatura, boleto ou COMPRA.pdf de um fornecedor registrado no processo vira proposta de `adicionar_taxa` (compra), com confiança média e a ambiguidade do rótulo apontada. Padrão de courier: 26/344 (DHL EXPRESS (BRAZIL) LTDA, freight, fixed, USD, prepaid). Caso que originou a regra: 26/358, recibo DHL com "Fator de Faturação 66,75 USD" que passou sem proposta de taxa.

## Anexar documento ao processo no Aura
`anexar_documento` aceita link https público de até 10 MB ou arquivo em base64 de cerca de 300 KB; recusa duplicado (mesmo nome e tamanho) e processo finalizado. PDF maior sem link público continua no fluxo manual. (fonte: descrição da ferramenta do conector AC, lida em 10/11/2026)

## Registrar o que aprendeu
Fato confirmado: acrescentar em `clientes.json` (fonte + data + confiança). Palpite: `hipoteses.md`. Erro que pode se repetir: `armadilhas.md`.

## CNPJ já visto em outro processo do mesmo cliente (usuário, 11/11/2026)
Antes de abrir dúvida de CNPJ, procurar o nome em `PROCESSOS\FINALIZADOS\` (NF-e de saída, extrato da DI). Caso VENTURA: NF-e de saída do 26-128 trouxe o CNPJ. `pdftotext -layout` (Git Bash) lê os PDFs.
