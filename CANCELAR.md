# Rota Cancelar (NFe/NFCe)

**POST** /nfe/cancelar

**POST** /nfce/cancelar

As chamadas para essa rota solicitam o cancelamento do docuemento fiscal (lembre-se quem cancela é a SEFAZ).

*NOTA: somente podem ser cancelados documentos que estão autorizados e presentes na base de dados da API, caso tente cancelar um documento não autorizado (denegado/cancelado/indefinido) um erro será retornado.*

*NOTA: ATENÇÃO aos prazos de cancelamento: NFe(55) até 24hs, NFCe(65) até 10min, por padrão.*


## Envio

```json
{
    "ambiente": "homologacao",
    "contingencia": {
        "datahora": "2021-02-17T15:55:20-03:00",
        "motivo": "Sefaz autorizadora fora do ar"
    },
    "chave": "12345678901234567890123456789012345678901234",
    "motivo": "Erro na emissão da nota fiscal, falha de digitação"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|
|chave|string|Chave de identificação do documento|OBRIGATORIO 44 digitos numericos|
|motivo|string|Motivo do cancelamento|OBRIGATORIO de 15 a 255 caracteres|


## Retorno

```json
{
    "sucesso": true,
    "codigo": "101",
    "mensagem": "Homologado o cancelamento da NF-e",
    "data_hora_evento": "2019-09-16T17:18:48-03:00",
    "protocolo": "141190000844226",
    "xml": "PGVudjpFbnZlbG9wZSB4bWxuczplbnY9J2h...cGU+",
    "pdf": "JVBERi0xLjMKMyAwIG9iago8PC9UeXBlIC9QYA+...GCg=="
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|data_hora_evento|string|Data e hora do evento de cancelamento autorizado||
|protocolo|string|Numero do protocolo de cancelamento||
|xml|string|XLM do Evento de cancelamento protocolado|em base64|
|pdf|string|PDF do Evento de cancelamneto|em base64|