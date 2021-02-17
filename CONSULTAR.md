# Rota Consultar (NFe/NFCe)

**POST** /nfe/consultar

**POST** /nfce/consultar

Esta rota permite que o chamador (ERP), consulte os dados de uma NFe ou NFCe


## Envio

Os payloads podem ser criados usando arrays, exemplo:

```php
$post = [
    "ambiente" => "homologacao",
    "chave" => "12345678901234567890123456789012345678901234",
    "contingencia" => [
        "datahora" => '2021-02-17T15:55:20-03:00',
        "motivo" => 'Sefaz autorizadora fora do ar'
    ]
];
```


```json
{
    "ambiente": "homologacao",
    "chave": "12345678901234567890123456789012345678901234",
    "contingencia": {
        "datahora": "2021-02-17T15:55:20-03:00",
        "motivo": "Sefaz autorizadora fora do ar"
    }
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|
|chave|string|chave de 44 digitos da NFe ou da NFCe|


*NOTA: No caso de NFe, caso a mesma ainda não tenha sido autorizada, será feita a consulta pelo recibo (se existir) e pela chave (caso não tenhamos o recibo), em caso de resposta do protocolo as mesam será protocolada e registrada na base de dados.*

*NOTA: NFCe emitidas OFFLINE apenas retornarão em sua situação atual.*

## Retorno

```json
{
    "sucesso": true,
    "codigo": 100,
    "mensagem": "Autorizado o uso do NF-e",
    "chave": "50190813188739000110550010000012001581978549",
    "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZ...VByb2M+",
    "pdf": "JVBERi0xLjMKMyAwIG9iago8PC9UeXBlIC9QYA+...GCg==",
    "data_hora_evento": "2019-08-30T11:25:22-04:00",
    "protocolo": "150190003925457",
    "status": "autorizado",
    "numero": "1200",
    "serie": "1"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|chave|string|Chave do documento fiscal|44 digitos numericos|
|xml|string|XML protocolado do documento fiscal||
|pdf|string|PDF representação grafica do documento fiscal||
|data_hora_evento|string|data e hora do evento de autorização||
|protocolo|string|protocolo de autorizaçao||
|status|string|situação do documento|offline ou autorizado ou denegado ou cancelado ou indefinido|
|numero|string|numero do docuemento fiscal||
|serie|string|seire do documento fiscal||

*NOTA: um doumento pode estar na condição de indefinido apenas em caso de falhas de comunicaçao com a SEFAZ autorizadora (ex. timeouts recorrentes).*

*NOTA: a condição de **INDEFINIÇÃO** pode acarretar falhas no controle especialmente de NFCe, pois sua emissão não pode esperar. É recomendável um tratamento critérioso para emissão em contingência OFFLINE nesses casos."