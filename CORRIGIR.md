# Rota Corrigir (NFe apenas)

**POST** /nfe/corrigir

Esta rota permite que o chamador (ERP), consulte os dados de uma NFe ou NFCe

*NOTA: Este evento so pode ser realizado se o documento fiscal existir no banco de dados e estiver com situação de autorizado*


## Envio

Os payloads podem ser criados usando arrays, exemplo:

```php
$post = [
    "ambiente" => "homologacao",
    "chave" => "50191213188739000110550010000012151581978542",
    "justificativa" => "Teste de carta de correcao",
    "contingencia" => [
        "datahora" => '2021-02-17T15:55:20-03:00',
        "motivo" => 'Sefaz autorizadora fora do ar'
    ]
];
```


```json
{
    "ambiente": "homologacao",
    "chave": "50191213188739000110550010000012151581978542",
    "justificativa": "Teste de carta de correcao",
    "contingencia": {
        "datahora": "2021-02-17T15:55:20-03:00",
        "motivo": "Sefaz autorizadora fora do ar"
    }
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|chave|string|chave de 44 digitos da NFe ou da NFCe|
|justificativa|string|deve conter de 15 a 255 caracteres|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|


## Retorno

```json
{
    "sucesso": true,
    "codigo": "135",
    "mensagem": "Evento registrado e vinculado a NF-e",
    "data_hora_evento": "2019-09-16T17:09:02-03:00",
    "protocolo": "141190000844206",
    "xml_carta_correcao": "PHByb2NFdmVudG9ORmUgeG1sbnM9Imh0d...5GZT4=",
    "pdf_carta_correcao": "JVBERi0xLjMKMyAwIG9iago8PC9UeXBlI...AHCg==",
    "numero_carta_correcao": 2
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|data_hora_evento|string|data e hora do evento||
|protocolo|string|protocolo do evento||
|xml_carta_correcao|string|XML protocolado do documento fiscal||
|pdf_carta_correcao|string|PDF representação grafica da carta de correção||
|numero_carta_correcao|string|numero do evento da carta de cprreção||