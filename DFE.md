# Doc Linvix DFe

Este documento contẽm a documentação básica do serviço para busca das NFe destinadas desenvolvido para a Linvix.

## Infra-estrutura
- Lumen Framework (última versão estável)
- Possibilidade de instalação em ambiente de vps (nuvem)
- Segurança (SEM ACESSO EXTERNO, SEM WEBSERIVCE)
- Operação exclusivamente baseada em SCHEDULE e QUEUE (laravel)
- Banco de dados Postgres ou MySql ou MSSQL (UTF8)
- Arquivamento dos documentos fiscais em S3 (AWS ou Digital Ocean)
- Linguagem de programação PHP 7.4/8.0
- Ambiente Linux (Debian 10)
- Base de dados própria do DFe (acesso compartilhado com o ERP)
- Modelagem da base de dados usando o padrão “Eloquent” (Lumen)


## Principio de Operação


A operação ocorre por comandos agendados do Lumen que são executados periódicamente pelo agendador do Lumem e colocam JOBS na fila para processamento.

Esse agendamento é iniciado em intervalos minimos de 2 horas, ao longo do dia todo exceto aos domingos. Cada agendamento irá criar um JOB para cada CNPJ cadastrado (e com o certificado A1 válido).

O "SUPERVISOR" (programa interno no linux), monitora o processo de busca das filas e inicia um ou mais processsos paralelos (essa configuração depende da capacidade de memoria e numero de processadores do servidor).

Cada JOB buscar as NFe, e os Eventos destinados, com até 20 iterações (podem ser recebidos até 50x20 = 1000 documentos por JOB) em cada loop interno e manifestar a ciencia da operação (para permitir a baixa dos documentos) dos resumos de NFe que foram recebidos. Isto significa que as NFe que foram manifestadas agora, provalmente estarão disponiveis daqui a duas horas, quando o ciclo recomeçar.

**NOTA: o sincronismo entre a Receita Federal e as SEFAZ não é feito em tempo real, portanto uma NFe pode ser obtida em 2 horas ou em até dias, a depender do serviço da Receita !!!**

Todos os documentos recebidos são lançados em tabelas da base de dados e o xml é salvo diretamente no S3.
