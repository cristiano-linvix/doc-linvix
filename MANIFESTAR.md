# Rota Manifestar (somente para NFe)

**POST** /nfe/manifestar

Esta rota solicita a manifestação de destinatário para as NFe recebidas.


*NOTA: Os eventos de manifestação não usam ambiente em contingẽncia.*

*NOTA: a manifestação está se tornnado OBRIGATÓRIA, leia [CloudDFe Post](https://blog.cloud-dfe.com.br/manifestacao-da-nfe-passou-a-ser-obrigatoria).*

## Envio

```json
{
    "ambiente": "homologacao",
    "contingencia": null,
    "chave": "12345678901234567890123456789012345678901234",
    "tipo_evento": "210240",
    "motivo": "não comprei a mercadoria emissão falsa"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|
|chave|string|Chave da NFe|Obrigatório|
|tito_evento|string|Codigo do tipo do evento de manifestação|Obrigatório|
|motivo|string|Descrição do motivo da manifestação|Opcional|


## Retorno

```json
{
    "sucesso": true,
    "codigo": "135",
    "mensagem": "Evento registrado e vinculado a NF-e",
    "data_hora_evento": "2019-09-16T17:34:03-03:00",
    "protocolo": "891190004733543",
    "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbm...BlPg=="
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|data_hora_evento|string|Data e hora do evento de manifestação autorizado||
|protocolo|string|Numero do protocolo de manifestação||
|xml|string|XLM do Evento de manifestação protocolado|em base64|
