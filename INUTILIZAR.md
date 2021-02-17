# Rota Inutilizar (NFe/NFCe)

**POST** /nfe/inutilizar

**POST** /nfce/inutilizar

Esta rota solicita a inutilização de uma faixa de numeros de documentos que foram "pulados" por algum motivo.

*NOTA: Os eventos de inutilização não usam ambiente em contingẽncia.*

## Envio

```json
{
    "ambiente": "homologacao",
    "contingencia": null,
    "numero_incial": "1234",
    "numero_final": "1234",
    "serie": "1",
    "motivo": "falha no controle de numeração do aplicativo"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|ambiente|string|define em qual ambiente o processo ocorrerá|Opcional homologacao(default) ou producao|
|contingencia|objeto|contem a datahora de entrada em contingência e o motivo|Opcional|
|numero_inicial|string|numero inicial a ser inutilizado|OBRIGATORIO numerico|
|numero_final|string|numero final a ser inutilizado|OBRIGATORIO numerico|
|serie|string|serie do documento|OBRIGATORIO numerico|
|motivo|string|Motivo do cancelamento|OBRIGATORIO de 15 a 255 caracteres|

*NOTA: A SEFAZ não autoriza inutilizações em contingência*