
# Rota Situação

Consulta o status do Webservice da SEFAZ (Nfe/Nfce).

**NOTA: é muito importante que não se consuma esse processo de forma aleatória ou indiscrimiada, limite o numero de consultas e garanta um intervalo minimo de pelo menos 5 minutos entre cada consulta. É recomendável que esse tipo de consulta não seja feita pelo usuário, mas por um processo interno do ERP que leve em consideração as limitações impostas pela SEFAZ.**



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

*NOTA: para verificar se o ambiente de contingência está ativo, passe as informações no campo contingência, do contrário a consulta será realizada no ambiente normal.*

*NOTA: para uso em contingência AMBOS os campos "datahora" e "motivo" devem ser passados, para uso em ambiente normal, NÃO passe o parametro "contingencia" ou nulifique.*

*NOTA: no caso das NFCe o parametro contringência, não tem efeito e é ignorado, nesta rota.*

## Retorno

```json
{
    "sucesso": true,
    "codigo": 107,
    "mensagem": "Serviço em operação"
}

```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|


*NOTA: sempre que o retorno indicar "sucesso": false, houve um problema que deve ser analisado e resolvido.*

## Possiveis retornos

|Código|Indicação|Observação|
|:---:|:---|:---|
|107|Serviço em operação|vamos em frente aparentemente tudo está funcionando na SEFAZ|
|108|Serviço Paralisado Momentaneamente (curto prazo)|AGUARDE !!!|
|109|Sefaz autorizadora sem previsão de retorno|Consulte o ambiente de contingência e se estiver ativo use|
|113|Contingência em processo de desativação|retorne ao ambiente normal|
|114|Serviço de contingência não disponivel|AGUARDE, tenha PACIÊNCIA e consulte novamente|
|7???|Erro no acesso ao webservice, como timeout por exemplo|Se ocorrer mais de duas vezes consecutivamente, consulte o ambiente de contingência e se estiver ativo use|