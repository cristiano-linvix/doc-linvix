# Doc Linvix

Este documento contẽm a documentação básica da API para geração de documentos fiscais (NFe e NFCe) desenvolvido para a Linvix.

## Infra-estrutura 
- Lumen Framework
- Instalação em ambiente de vps (nuvem)
- Segurança por token (JWT)
- Banco de dados Postgres ou MySql ou MSSQL (UTF8)
- Padrão HTTP Restful (json UTF8)
- Linguagem de programação PHP 7.4/8.0
- Ambiente Linux (Debian 10)
- Modelagem da base de dados usando o padrão “Eloquent” (Lumen)


## Estrutura de ROTAS de acesso a API


### Controle dos Emitentes

- Os emitentes deverão ser criados pelo sistema ERP, e seus dados lançados nessa tabela da base de dados da API.
- Esta rota é protegida por um token unico de controle da SOFTHOUSE, que é criado durante o processo de configuração, usando a linha de comando artisan. Caso esse comando artisan seja usado novamente, um NOVO token de softhouse será gerado e o anterior perderá a validade.
- Este token (de softhouse) será usado exclusivamente para criar ou recriar os tokens dos emitente e será passado no header da chamada HTTP.

**Tabela Emitentes**

|Campo|Tipo|Index|Null|Descrição|
|:---|:---:|:---:|:---:|:---|
|id|bigInteger|Primary||Id da tabela|
|razao|string(255)|||Razão Social do emitente|
|fantasia|string(255)||NULL|Nome fantasia do emitente|
|cnpj|string(14)|Index|NULL|CNPJ do emitente|
|cpf|string(11)|Index|NULL|CPF do emitente|
|ie|string(15)||NULL|Inscrição estadual do emitente|
|iest|string(15)||NULL|Inscrição estadual de Substitutivo tributário do emitente|
|im|string(15)||NULL|Inscrição Municipal do emitente|
|crt|tinyInteger|||Código de Regime Treibutário do emitente|
|csc|string(150)||NULL|Código de Segurança do Contribuinte|
|cscid|string(6)||NULL|Número de controle do CSC|
|endereco|string(60)|||Logradoudo do endereço do emitente (conforme registro na receita)|
|Número|string(60)|||Número do endereço do emitente (conforme registro na receita)|
|complemento|string(60)||NULL|Complemento do endereço do emitente (conforme registro na receita)|
|bairro|string(60)|||Bairro do endereço do emitente  (conforme registro na receita)|
|municipio|string(60)|||Nome do Municipio do endereço do emitente  (conforme registro na receita)|
|cmun|string(7)|||Código IBGE do endereço do emitente|
|uf|string(2)|||Sigla do estado do endereço do emitente|
|pais|string(60)|||Nome do pais do enderço do emitente|
|cpais|string(8)|||Código do pais (BACEN)|
|cep|string(8)|||Código Postal do endereço do emitente|
|fone|string(14)||NULL|Telegone do endereço do emitente|
|token|string(150)||NULL|Token de acesso gerado posteriormente pela chamada da rota /token|
|certificado|longText||NULL|Certificado A1 pfx, codificado em base64|
|senha|string(25)||NULL|Senha do certificado (não usar caracteres acentuados e/ou de controle|
|validade|date|||NULL|Data de validade do certificado (será gerado automaticamente, com a geração do Token)|


|Rota|Método|Descrição|
|:---:|:---:|:---|
|/token/{id}|GET|Rota para criação e registro do TOKEN do emitente|

Esta rota token, espera que todos os dados do emitente estejam disponíveis na tabela, inclusive o certificado A1.

O id indicado na rota, refere-se ao ID da tabela 'emitentes'.

*NOTA: Caso não exista o certificado ou o mesmo esteja vencido o token NÃO SERÁ criado !!*

Esse token, será passado no header das chamadas HTTP e identifica o emitente na API.

Novas chamadas a esta rota irão RECRIAR o token e dasativar o token gerado anteriormente.


### Rotas da NFe

Todas as operações com NFe são processadas pelas chamadas aqui descritas.

|Rota|Método|Descrição|
|:---|:---:|:---|
|/nfe/situacao|POST|Busca o status da Sefaz autorizadora|
|/nfe/consultar|POST|Consulta o registo na base de dados e se necessário o recibo ou chave da NFe na SEFAZ|
|/nfe/pdf|POST|Cria o PDF da DANFE|
|/nfe/emitir|POST|Cria a NFe ASSINCRONO, requer consulta posterior|
|/nfe/cancelar|POST|Solicita o cancelamento da NFe|
|/nfe/inutilizar|POST|Solicita a inutilização de faixa de Números|
|/nfe/corrigir|POST|Solicita a Carta de Correção|
|/nfe/manfestar|POST|Solicita a manifestação de Destinatário|


### Rotas da NFCe

Todas as operações com NFCe são processadas pelas chamadas aqui descritas.

|Rota|Método|Descrição|
|:---|:---:|:---|
|/nfce/situacao|POST|Busca o status da Sefaz autorizadora|
|/nfce/consultar|POST|Consulta o registo na base de dados e se necessário o recibo ou chave da NFCe na SEFAZ|
|/nfce/pdf|POST|Cria o PDF da DANFCE|
|/nfce/offline|POST|Processa as NFCe emitidas em contingência OFFLINE, enviando todas as NFCe pendentes para  a SFEAZ autorizadora|
|/nfce/emitir|POST|Cria a NFCe SINCRONO, requer apenas esta chamada geralmente|
|/nfce/cancelar|POST|Solicita o cancelamento da NFCe|
|/nfce/inutilizar|POST|Solicita a inutilização de faixa de Números|
|/nfce/substituir|POST|Solicita a Substituição de uma NFCe por outra|


### Critérios basicos para as operações com NFe NFCe

- Autenticação por TOKEN JWT do EMITENTE, criado pela rota "/token/{id}".
- Os campos dos json, tanto no envio como nos retornos estarão em portugues brasileiro, para efeito de compreensão.
- Todas as chamadas usam apenas o método POST, e todas devem conter um json com:

```
{
    "ambiente": "homologacao", //pissiveis homologacao ou producao
    ...
}
```

## Payload das Rotas

### Situação do Webservice da SEFAZ (Nfe/Nfce)

#### Envio

```json
{
    "ambiente": "homologacao",
    "contingencia": false
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|homologacao ou producao|
|contingencia|boolean|Opcional indica se queremos o status da SEFAZ autorizadora (false) ou do ambiente de contingência (true)|false ou true|

*NOTA: no caso das NFCe o parametro contringência, não tem efeito e é ignorado.*

#### Retorno

```json
{
    "sucesso": true,
    "codigo": 107,
    "mensagem": "Serviço em operação"
}

```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
