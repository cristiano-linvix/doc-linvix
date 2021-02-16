# Rota Consultar (NFe/NFCe)

Esta rota permite que o chamador (ERP), consulte os dados de uma NFe ou NFCe


## Envio

Os payloads podem ser criados usando arrays, exemplo:

```php
$post = [
    "ambiente" => "homologacao",
    "contingencia" => [
        "datahora" => '2021-02-17T15:55:20-03:00',
        "motivo" => 'Sefaz autorizadora fora do ar'
    ]
];
```


```json
{
    "ambiente": "homologacao",
    "contingencia": {
        "datahora": "2021-02-17T15:55:20-03:00",
        "motivo": "Sefaz autorizadora fora do ar"
    },
    "chave": "12345678901234567890123456789012345678901234"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|
|chave|string|chave de 44 digitos da NFe ou da NFCe|


*NOTA:

## Retorno
