# Uso em contingência

O uso da contigência requer o chaveamento para esse ambiente, quando a SEFAZ autorizadora AOUTORIZE o seu uso e APENAS nesse caso.

Esse chaveamento deve ser controlado pelo ERP, sendo ativado e desativado apenas quando for necessário e AUTORIZADO pela SEFAZ.

Para isso é recomendado que haja um registro (em tabela) na base de dados que armazene os dados de contingência se estiver ativa ou nulifique esse registro caso desative. É desse registro que o ERP deve colher esses dados para passa-los para os payloads da API.


## Ambientes de Contingência da API

### NFe (SVCAN/SVCRS)

As NFe (modelo 55) pode ser emitidas normalmente por esses serviços de contingência.

Quando um documento for emitido em contingência a consulta também deverá ser feita em contingência. Portanto o intervalo de tempo entre o envio da NFe e sua consulta não pode ser longa, senão poderão haver falhas na localização do documento, visto que a sincronização entre a SEFAZ autorizadora e o sistema de contigência não é feito em tempo real.

### NFCe (OFFLINE)

As NFCe poderão ser emitidas em contingência OFFLINE, se a SEFAZ autorizadora assim permitir.

Os documentos emitidos em contingência OFFLINE, devem ser o quanto antes enviados para a SEFAZ autorizadora para terem real validade fiscal. Dito isso, uma rotina interna deve a cada hora processar esses documentos OFFLINE.
