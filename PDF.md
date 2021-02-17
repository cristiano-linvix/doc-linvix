# Rota Consultar (NFe/NFCe)

**POST** /nfe/pdf

**POST** /nfce/pdf

Esta rota permite que o chamador (ERP), consulte os dados de uma NFe ou NFCe


## Envio

Os payloads podem ser criados usando arrays, exemplo:

```php
$post = [
    "ambiente" => "homologacao",
    "chave" => "12345678901234567890123456789012345678901234"
];
```


```json
{
    "ambiente": "homologacao",
    "chave": "12345678901234567890123456789012345678901234"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|chave|string|chave de 44 digitos da NFe ou da NFCe|


## Retorno

```json
{
    "sucesso": true,
    "codigo": 1001,
    "mensagem": "Sucesso.",
    "pdf": "JVBERi0xLjMKMyAwIG9iago8PC9UeXBlIC9QYA+...GCg=="
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|pdf|string|PDF representação grafica do documento fiscal|em base64|