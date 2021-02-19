# Doc Linvix API

Este documento contẽm a documentação básica da API para geração de documentos fiscais (NFe e NFCe) desenvolvido para a Linvix.

## Infra-estrutura
- Lumen Framework (última versão estável)
- Possibilidade de instalação em ambiente de vps (nuvem)
- Segurança de acesso por token (JWT)
- Banco de dados Postgres ou MySql ou MSSQL (UTF8)
- Arquivamento dos documentos fiscias em bucktes S3 (AWS ou Digital Ocean)
- Padrão HTTP Restful (json UTF8)
- Linguagem de programação PHP 7.4/8.0
- Ambiente Linux (Debian 10)
- Base de dados própria da API (acesso compartilhado com o ERP)
- Modelagem da base de dados usando o padrão “Eloquent” (Lumen)

## Integração

É recomendável usar o [Guzzle](https://docs.guzzlephp.org/en/stable/) para realizar a integração caso seu aplicativo seja escrito em PHP, alternativamente pode usar diretamente chamadas cURL do PHP.

## Instalação e configuração

- Desempacotar o arquivo ZIP em uma pasta em sua maquina local (para testes locais antes de subir aos servidores)
- Recomendável subir o pacote para um repositório GIT para manutenção, instalação e atualização dos servidores
- Criar o arquivo .env na raiz do projeto, com as suas credenciais para o banco de dados
- Instalar as dependências com o composer (composer install --no-dev)
- Executar os comandos artisan:

```

php artisan migrate --seed

php artisan token:create

```


## Estrutura de ROTAS de acesso a API

### Controle dos Emitentes

- Os emitentes deverão ser criados pelo sistema ERP, e seus dados lançados nessa tabela da base de dados da API.

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
|numero|string(60)|||Número do endereço do emitente (conforme registro na receita)|
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
|validade|date||NULL|Data de validade do certificado (será gerado automaticamente, com a geração do token)|


|Rota|Método|Descrição|
|:---:|:---:|:---|
|[/token/{id}](TOKEN.md)|GET|Rota para criação e registro do TOKEN do emitente|

- Esta rota é protegida por um token unico de controle da SOFTHOUSE, que é criado durante o processo de configuração, usando a linha de comando artisan. Caso esse comando artisan seja usado novamente, um NOVO token de softhouse será gerado e o anterior perderá a validade.

- Este token (de softhouse) será usado exclusivamente para criar ou recriar os tokens dos emitente e será passado no header da chamada HTTP.

- Esta rota token, espera que todos os dados do emitente estejam disponíveis na tabela, inclusive o certificado A1.

- O id indicado na rota, refere-se ao ID da tabela 'emitentes'.

- O token de emitente, gerado por essa rota, será passado no header das chamadas HTTP e identifica o emitente na API.

- Novas chamadas a esta rota irão RECRIAR o token e dasativar o token gerado anteriormente.

*NOTA: Caso não exista o certificado ou o mesmo esteja vencido o token NÃO SERÁ criado !!, e portanto o emitente não terá acesso as rotas.*

### Rotas da NFe

Todas as operações com NFe são processadas pelas chamadas aqui descritas.

|Rota|Método|Descrição|
|:---|:---:|:---|
|[/nfe/situacao](SITUACAO.md)|POST|Busca o status da Sefaz autorizadora|
|[/nfe/consultar](CONSULTAR.md)|POST|Consulta o registo na base de dados e se necessário o recibo ou chave da NFe na SEFAZ|
|[/nfe/pdf](PDF.md)|POST|Cria o PDF da DANFE|
|[/nfe/emitir](EMITIR.md)|POST|Cria a NFe ASSINCRONO, requer consulta posterior|
|[/nfe/cancelar](CANCELAR.md)|POST|Solicita o cancelamento da NFe|
|[/nfe/inutilizar](INUTILIZAR.md)|POST|Solicita a inutilização de faixa de Números|
|[/nfe/corrigir](CORRIGIR.md)|POST|Solicita a Carta de Correção|
|[/nfe/manfestar](MANIFESTAR.md)|POST|Solicita a manifestação de Destinatário|


### Rotas da NFCe

Todas as operações com NFCe são processadas pelas chamadas aqui descritas.

|Rota|Método|Descrição|
|:---|:---:|:---|
|[/nfce/situacao](SITUACAO.md)|POST|Busca o status da Sefaz autorizadora|
|[/nfce/consultar](CONSULTAR.md)|POST|Consulta o registo na base de dados e se necessário o recibo ou chave da NFCe na SEFAZ|
|[/nfce/pdf](PDF.md)|POST|Cria o PDF da DANFCE|
|[/nfce/offline](OFFLINE.md)|POST|Processa as NFCe emitidas em contingência OFFLINE, enviando todas as NFCe pendentes para  a SFEAZ autorizadora|
|[/nfce/emitir](EMITIR.md)|POST|Cria a NFCe SINCRONO, requer apenas esta chamada geralmente|
|[/nfce/cancelar](CANCELAR.md)|POST|Solicita o cancelamento da NFCe|
|[/nfce/inutilizar](INUTILIZAR.md)|POST|Solicita a inutilização de faixa de Números|


### Critérios basicos para as operações com NFe NFCe

- Autenticação por TOKEN JWT do EMITENTE, criado pela rota "/token/{id}".
- Os campos dos json, tanto no envio como nos retornos estarão em português brasileiro, para efeito de melhor compreensão.
- Todas as chamadas usam apenas o método POST, e todas devem conter um json com:

```
{
    "ambiente": "homologacao",
    ...
}
```

- Todos os retornos das chamas da API conterão:

```
{
    "sucesso": true,
    "codigo": "99999",
    "mensagem": "mensagem relativa a operação",
    "????": "aqui podem ser retornados varias informações, em diferentes formatos, dependendo da rota"
}
```
