# Tokens

Existem dois tipos de token, cada um com as suas permissões implicitas.

## Token de Softhouse

Gerado para permitir o acesso do ERP para criar/recriar os tokens dos emitentes, e serve apenas para essa finalidade.

Esse token é criado pelo comando artisan "php artisan token:create" e o valor desse token pode ser recuperado da resposta da linha de comando ou pela tabela users.

## Token de Emitente

Gerado pela rota "/token"

## Envio

```json
{
    "ambiente": "homologacao",
    "contingencia": {
        "datahora": "2021-02-17T15:55:20-03:00",
        "motivo": "Sefaz autorizadora fora do ar"
    },
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|

*NOTA: nenhum dos campos do json irá alterar o resultado desta operação, ambos parametros são ignorados.*

## Retorno

```json
{
    "sucesso": true,
    "codigo": 1000,
    "mensagem": "Token gerado com sucesso",
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbXAiOiIxIiwidXNyIjoiOCIsInRwIjoyPRdgYXQiOjE1ODE2MTUzNTJ9.s4-XbF4TFrZWVFrFkMk456Dv7FLQE4ACs98kKspO-ZK"
}

```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|token|string|Token do emitente|gravado na tabela do emitente da API e deve ser usado no header das chamadas a API|

