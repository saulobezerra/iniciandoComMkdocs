# Eventos

São registros de operações que precisão ser executadas ao termino de uma outra operação ou algum tempo depois ou mesmo na inicialização do sistema. Os registro de eventos são gravados na tabela `Evento` durante uma transação, em determinados momentos são realizadas consultas na tabela para verificar se há algum registro e assim efetivar a operação de que se trata o evento.

Os registro de eventos são gravados na tabela pela `procedure` [`adicionaEvento`](../../units/unitVenda/#adicionaevento) e as verificações são feitas pelo [`trataEventos`](../../units/unitVenda/#trataeventos) ambos da [`unitVenda`](../../units/unitVenda). Na `unitVenda` também possui os diversos tipos de `eventos` relacionados à venda. São eles:

.  
.  
.  

## A tabela `Evento`


| Campo | Tipo | Tamanho |  Descrição |
| ----- | ---- | :-----: | --------- |
|COD_CAIXA_ABERTURA_TURNO | Numérico | 10 | Código do caixa em aberto do qual o evento se refere |
|EVENTO | Varchar | 50 | Nome do evento a ser executado |
|DATA_EVENTO | Date | - | Data de registro do evento |
|HORA_EVENTO | Time | - | Hora de registro do evento |
|EXECUTADO | Varchar | 5 | Indica se o evento já foi executado |
|PARAMETROS | Varchar | 200 | Parâmetro(s) a serem utilizados na execução do eventos. Caso haja mais de um parâmetro, eles são concatenados e separados por pipe ( '\|' ) |
|DATA_EXECUTADO | Date | - | Data em que o evento foi executado |
|HORA_EXECUTADO | Time | - |Hora em que o evento foi executado |
|MENSAGEM | Varchar | 100 | Campo que guarda as possíveis mensagens de erro ao executar o evento|
|NFCE | Varchar | 5 | Informa se o evento envolve NFCe |
|COD_VENDA | Numérico | 10 | Código da venda |


