# Rota OFFLINE (NFCe (65) apenas)


**POST** /nfce/offline


Esta rota em principio não será usada, pois haverá um processo em background varrendo a tabela em busca de NFCe emitidas OFFLINE e tentará processa-las de forma automática.

Esse processo usa FILAS e tenta enviar UMA a UMA as NFCe que estiverem nessa condiçao, e será executado a cada 4 horas.


Esta rota dispara o mesmo processo em fila relatado anteriormente.

## Envio

```json
{
    "ambiente": "homologacao",
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

*NOTA: apenas o ambiente é considerado nessa rota, o parametro contingência é ignorado e portanto dispensável*