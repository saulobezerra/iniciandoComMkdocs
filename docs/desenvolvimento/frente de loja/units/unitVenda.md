# unitVenda  

A unitVenda é de forma geral responsável pelas operações de vendas.  

Essa unit recebe um bloco de informações de dados de uma venda ou bloco de itens de uma venda, formas de pagamentos de uma venda e juntando todas essas informações realiza a transação de venda por completo. Por exemplo, se for uma transação de venda que envolve ligação com impressora fiscal ela vai utilizar dos meios de se comunicar com a impressora, vai registrar os itens; se for uma transação de venda que tá utilizando o tipo de documento de nota fiscal eletrônica, ela vai realizar a transmissão e todos os procedimentos relativos a nota fiscal eletronica. Enfim, essa unit é responsável por registrar fiscalmente a operação de venda do sistema através de um bloco, o que favorece no desacoplamento do sistema, ou seja, cada unit é responsável por determinadas operações.

## Funções e Procedures
Antes de descrever cada função é importante saber que algum dos seus parâmetros possuem tipos que estão declarados em outras units, por exemplo, a função `registraItem` possui o parâmetro `Itens` do tipo `AItensVenda`, que está declarado na unit [`unitTiposVenda`](/desenvolvimento/frente%20de%20loja/units/unitTiposVenda).  

### registraVenda

É uma função que pode realiza uma venda por completo, mas pode também apenas iniciar a abertura do cupom.

    function registraVenda(codCaixaAberturaTurno: Int64; var codVenda: Int64;
                            dadosVenda: TDadosVenda; itensVenda: AItensVenda; 
                            recebimentos: ARecebimento; troco: Atroco; 
                            automatico: boolean; imprime: boolean = true): boolean  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto                  |
|@codVenda            | Integer     | Retorno do código do item                  |
|dadosVenda           | [TDadosVenda](../unitTiposVenda/#tdadosvenda)  | Dados da venda |
|itensVenda           | [AItensVenda](../unitTiposVenda/#aitensvenda)  | Array com os itens de venda que se quer registrar na operação |
|recebimentos         | [ARecebimento](../unitTiposVenda/#arecebimento)| Array com as formas de pagamentos da venda|
|troco                | [ATroco](../unitTiposVenda/#atroco)            | Array com os diferentes tipos de troco |
|automatico           | boolean     | Flag que informa se a operação aconteceu de forma involutária ao usuário.|
|imprime              | boolean     |                                             |

A variável `dadosVenda` carrega as informações relativas a capa da venda.  

Caso o parâmetro `itensVenda` não seja informado, a função registrará apenas uma venda aberta, uma venda sem item, esperando que seja inserido os itens posteriormente.  

Os `recebimentos` só poderão ser informados se `itensVenda` já tiverem sido informados.  

`troco` informa como foi entregue ao cliente os trocos pra os excessos do recebimento em detrimento aos itens. Vale salientar aqui que troco não é apenas em dinheiro, mas pode ser dado também como crédito, cheque e etc.  

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida. Note também que essa função recebe um parâmetro por referência, `codVenda`, assim quando uma venda for registrada ela retorna nesse parâmetro o código da venda em questão.  


### registraItem  
Registra um item na venda, quando esta estiver em aberto.

    function registraItem(codCaixaAberturaTurno: Int64; itens: AItensVenda): Boolean; overload;

Observe que esse método é sobrecarregado (`overload`) e o `registraItem` item abaixo é apenas uma opção que você pode passar no parâmetro um só item.

    function registraItem(codCaixaAberturaTurno: Int64; item: TItemVenda): Boolean; overload; 

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto                  |
|itensVenda           | [AItensVenda](../unitTiposVenda/#aitensvenda) | Array com os itens de venda que se quer registrar na operação|
|item                 | [TItemVenda](../unitTiposVenda/#titemvenda)   | Item de venda a ser registrado na operação.  

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.

### registraFormaDePagamento

Registra os pagamentos feitos na venda e finaliza o cupom fiscal caso a operação envolva impressora fiscal. Essa função é chamada dentro de `registraVenda`

    function registraFormaDePagamento(codCaixaAberturaTurno: Int64; recebimentos: ARecebimento; troco: ATroco; abreGaveta: boolean; titulosBaixados: string = ''): boolean  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto                  |
|recebimentos         | [ARecebimento](../unitTiposVenda/#arecebimento)| Array com as formas de pagamentos da venda|
|troco                | [ATroco](../unitTiposVenda/#atroco)            | Array com os diferentes tipos de troco |
|abreGaveta           | boolean     | informa se deve abrir a gaveta de dinheiro que tiver interligada ao PDV na hora em que finalizar a venda. |
|titulosBaixados      | boolean     | informar os titulos que foram utilizados durante uma operação não fiscal. |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.

### cancelaItem  

Cancela um item de uma venda que está em aberto. Se você estiver usando uma impressora fiscal a função vai realizar um comando para o cancelamento e vai registra na impressoa o item que está sendo cancelado e suas pertinencias fiscais. Se for uma operação de outro tipo ele vai realizar a operação relativa ao tipo de documento que está sendo utilizado. 

    function cancelaItem(codCaixaAberturaTurno: Int64; item: Integer): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                          |
|---------------------|-------------|------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto          |
|item                 | Integer     | código do item que deseja cancelar |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.

### cancelarVendaAberta  

Cancela uma venda em aberto. Uma venda em aberto se dá quando a venda foi registrada mas ainda não foi registrada sua(s) forma(s) de pagamento. 

    function cancelarVendaAberta(codCaixaAberturaTurno: Int64): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                          |
|---------------------|-------------|------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto          |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.

### cancelarVendaAnterior

Cancela uma venda que acabou de ser concluída na tela de vendas. 

    function cancelarVendaAnterior(codCaixaAberturaTurno: Int64): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                          |
|---------------------|-------------|------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto          |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.

### cancelarVenda  

Cancela uma venda qualquer do sistema, porém como uma venda de nota fiscal não pode ser cancelada é feito um cancelamento apenas para efeitos administrativos, para que se possa emitir a nota fiscal de devolução. 

    function cancelarVenda(codCaixaAberturaTurno: Int64; codVenda: Int64; cancelaNoECF: boolean): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                           |
|---------------------|-------------|-------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto           |
|codVenda             | Int64       | Código da venda que deseja cancelar |
|cancelaNoECF         | boolean     |                                     |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.


### cancelarTEF

É uma operação de cancelamento de uma das formas de pagamentos registradas na venda. 

    function cancelarTEF(codCaixaAberturaTurno: Int64; codVendaFechamento: Int64; labelOperador: TLabel): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno| Int64       | Código do caixa em aberto                  |
|codVendaFechamento   | Int64       | Código da venda fechamento a ser cancelada.|
|labelOperador        | TLabel      |                                            |

#### Retorno
A função retorna um valor tipo `boolean` para informar que a operação foi concluida.


### aplicaAcrescimoNoItem  

Possibilita aplicar um acrescimo no item de uma venda que está em aberto e que teve seu item registrado. Ao aplicar um valor para acrescimo a função aplicará as regras de calculo tributário relativo ao acrescimo e atualizará o valor do item.  

    function aplicaAcrescimoNoItem(codCaixaAberturaTurno: Int64; item: Integer; valorOperacao: Real): boolean;  

#### Descrição dos Parâmetros
|Parâmetro | Tipo  | Descrição |
|--------- |-------|-----------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto|
|item | Integer | Código do item a ser aplicado o acréscimo.|
|valorOperacao| Real | Valor do acréscimo a ser aplicado.|

#### Retorno
A função retorna um valor do tipo `boolean` para indicar que a operação foi realizada.


### aplicaDescontoNoItem 
Possibilita aplicar um desconto no item de uma venda que está em aberto e que teve seu item registrado. Ao aplicar um valor para desconto a função aplicará as regras de calculo tributário relativo ao desconto e atualizará o valor do item.  

    function aplicaDescontoNoItem(codCaixaAberturaTurno: Int64; item: Integer; valorOperacao: Real): boolean;  

#### Descrição dos Parâmetros
|Parâmetro | Tipo  | Descrição |
|--------- |-------|-----------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto|
|item | Integer | Código do item a ser aplicado o desconto.|
|valorOperacao| Real | Valor do desconto a ser aplicado.|

#### Retorno
A função retorna um valor do tipo `boolean` para indicar que a operação foi realizada.

### getTotalVendaEmAberto
Essa função pode ser utilizada para informar ao usuário o subtotal da venda que está em aberto.

    function getTotalVendaEmAberto: real  

#### Retorno
A função retorna o valor da venda em aberto.

### getCodVendaEmAberto  

    function getCodVendaEmAberto(codCaixaAberturaTurno: Int64): Int64;  

#### Descrição dos Parâmetros
|Parâmetro | Tipo  | Descrição |
|--------- |-------|-----------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto|

#### Retorno
Retorna o código da venda que está em andamento.


### getTipoNotaVendaEmAberto

    function getTipoNotaVendaEmAberto: Integer;  

#### Retorno 
Retorna um valor inteiro que representa tipo de nota da venda 


### temCupomFiscalAberto  
Verifica se possui um cupom em aberto  

    function temCupomFiscalAberto: boolean;  

#### Retorno  
A função retorna um valor tipo `boolean` indicando ser possui ou não cupom em aberto


### getCodVendaAnterior

    funtion getCodVendaAnterior(codCaixaAberturaTurno: Int64): Int64;  

#### Descrição dos Parâmetros
|Parâmetro | Tipo  | Descrição |
|--------- |-------|-----------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto|

#### Retorno
Retorna o código da venda anterior 


### existeVendaAberta

Verifica se existe venda em aberto, ou seja, um cupom de venda em aberto.

    function existeVendaAberta: boolean;

#### Retorno  
A função retorna um valor tipo `boolean` indicando se existe ou não uma venda em aberto


### validaItemDeVenda
Valida dados do item da venda. Pode ser usada antes de chamar a `registraItem` para verificar se as informações do item estão corretas.

    procedure validaItemDeVenda(item: TItemVenda, tipoNota: integer; codTipoFiscalVenda: integer; ufDestino: string; vendaParaVarejo: boolean; destinatarioContribuinteST: boolean);  

#### Descrição dos Parâmetros  
|Parâmetro                  | Tipo        | Descrição                                  |
|---------------------------|-------------|--------------------------------------------|
|item                       | [TItemVenda](../unitTiposVenda/#titemvenda)  | Item da venda que deseja validar. |
|tipoNota                   | integer     | Valor que represenata o tipo da nota       |
|codTipoFiscalVenda         | integer     |                                            |
|ufDestino                  | string      |                                            |
|vendaParaVarejo            | boolean     |                                            |
|destinatarioContribuinteST | boolean     |                                            |



### atualizaTributacaoItem

Atualiza os campos tributáveis de um item de venda.

    function atualizaTributacaoItem(tipoNota: integer, item: TItemVenda): string;  

#### Descrição dos Parâmetros  
|Parâmetro                  | Tipo        | Descrição                                  |
|---------------------------|-------------|--------------------------------------------|
|tipoNota                   | integer     | Valor que represenata o tipo da nota       |
|item                       | [TItemVenda](../unitTiposVenda/#titemvenda)  | Item da venda que deseja atualizar.        |

#### Retorno


### validaDadosVenda 

Valida um objeto do tipo `TDadosVenda`. Essa função pode ser utilizada antes da função `registraVenda` para verificar se os dados da venda estão corretos.

    procedure validaDadosVenda (dadosVenda: TDadosVenda; automatico: boolean);  

#### Descrição dos Parâmetros  
|Parâmetro         | Tipo        | Descrição                                  |
|------------------|-------------|--------------------------------------------|
|dadosVenda        | [TDadosVenda](../unitTiposVenda/#tdadosvenda) | Dados da venda a ser validada |
|automatico        | boolean     | Flag que informa se a operação aconteceu de forma involutária ao usuário. |



### solicitaDadosDeAberturaDaVenda

Exibe uma tela caso o usuário deseje alterar os dados da abertua de nota de uma venda, ou seja, de um `TDadosVenda`, antes de registrar a venda.

    function solicitaDadosDeAberturaDaVenda(venda: TDadosVenda; permiteEditarFormaPagamento: boolean; exigeInformarCliente: boolean; botaoAbrirSelecionado: boolena; permiteEditarTabelaPreco: boolean): boolean;  

#### Descrição dos Parâmetros  
|Parâmetro         | Tipo        | Descrição                                  |
|------------------|-------------|--------------------------------------------|
|dadosVenda        | [TDadosVenda](../unitTiposVenda/#tdadosvenda) | Dados da venda |
|permiteEditarFormaPagamento  | boolean     |  |
|exigeInformarCliente  | boolean     |  |
|botaoAbrirSelecionado  | boolean     |  |
|permiteEditarTabelaPreco  | boolean     |  |

#### Retorno
A função retorna um valor tipo `boolean` indicando se a operação foi executada corretamen.


### adicionaEvento

Adiciona um evento na tabela `Evento` utilizando a transação que está em andamento ou uma nova transação.

    procedure adicionaEvento(codCaixaAberturaTurno: Int64; tipo: string; parametros: string; query: TMYQuery; nfce: boolena = false; codVenda: Int64 = 0);

#### Descrição dos Parâmetros  
|Parâmetro         | Tipo        | Descrição                                  |
|------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto, se o evento não envolve o caixa, passa-se 0 |
|tipo  | string     | Tipo de evento a ser executada |
|parametros  | string     | Parâmetro(s) a serem utilizados na execução do eventos. Caso haja mais de um parâmetro, eles são concatenados e separados por pipe ( '\|' )  |
|query  | [TMYQuery](../unitQuery/#tmyquery)  | Transação atual que adicionará o evento, no entando essa adição só se realizará ao final da operação atual quando é dado `commit`. Caso seja passado `nil`, uma conexão será aberta, gravado o evento e dado `commit` instantaneamente. |
|nfce  | boolena     | `Flag` que indica que o evento é de NFCe, por padrão o valor desse parâmetro é `false`. |
|codVenda  | Int64   | Código da venda envolvida no evento. |


### trataEventos

Executa os eventos pendentes da tabela `Evento`.

    procedure trataEventos(codCaixaAberturaTurno: Int64; nfce: boolena = false);

#### Descrição dos Parâmetros  
|Parâmetro         | Tipo        | Descrição                                  |
|------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno|Int64| Código do caixa em aberto, se o evento não envolve o caixa, passa-se 0 |
|nfce  | boolena     | `Flag` que indica que o evento é de NFCe, por padrão o valor desse parâmetro é `false`. |  



### inicializaVendas

Inicializa na tela de vendas as variáveis que são necessárias para manter o bom fluxo do sistema.

    procedure inicializaVendas(codCaixaAberturaTurno: Int64)  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codCaixaAberturaTurno|Int64        | Código do caixa em aberto                  |



### transmitirNFEVenda

Envia a nota fiscal eletrônica da venda para a retaguarda. 

    procedure transmitirNFEVenda(codVenda: Int64);  

#### Descrição dos Parâmetros  
|Parâmetro            | Tipo        | Descrição                                  |
|---------------------|-------------|--------------------------------------------|
|codVenda             |Int64        | Código do caixa em aberto                  |



### getTributacaoECFOP

Essa função é utilizada para saber qual a tributação deve-se aplicar para um determinado produto em determinada venda.

    function getTributacaoECFOP (tipoNota: integer; codProduto: integer; codDocumentoTipo: integer; codNaturezaOperacao: integer; ufDestino: string; vendaParaVarejo: boolean; destinatarioContribuinteST: boolena; var codTributacao: int64; var codCFOP: int64; codTipoOperacaoEscolhida: int64): string;  

#### Descrição dos Parâmetros  
|Parâmetro                  | Tipo        | Descrição                                  |
|---------------------------|-------------|--------------------------------------------|
|tipoNota                   |integer      | Código referente ao tipo de nota           |
|codProduto                 |integer      | Código do produto                          |
|codDocumentoTipo           |integer      | Código referente ao tipo de documento      |
|codNaturezaOperacao        |integer      | Código de natureza da operação             |
|ufDestino                  |string       | UF do destinatário                         |
|vendaParaVarejo            |boolean      | Flag indicando se é vanda para varejo      |
|destinatarioContribuinteST |boolena      | Flag indicando se o destinatário é contribuinte substituto   |
|@codTributacao             |Int64        | Retorno do código da tributação            |
|@codCFOP                   |Int64        | Retorno do código do CFOP                  |
|codTipoOperacaoEscolhida   |Int64        | Código do tipo da operação escolhida       |

#### Retorno
Retorna o código da tributação em `codTributacao` e o código do CFOP em `codCFOP`, baseado nos parâmetros enviados. 
<!-- Falta falar do retorno da função em si -->


### getUFeContribuinteST 

    procedure getUFeContribuinteST(codAgente: Int64; var ufDestino: string; var destinatarioContribuinteST: boolean);  

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codAgente                   |Int64        | Código do agente                           |  
|@ufDestino                  |string       | Retorna a UF do destino                    |  
|@destinatarioContribuinteST |boolean      | Flag indicando se o destinatário é contribuinte substituto   |

#### Retorno
Retorna a UF do destinatário em `ufDestino` e se o destinatário é substituto em `destinatarioContribuinteST`.


### getXMLGravadoVenda 

Retorna o XML da venda e na variável `chave` a sua chave de acesso.

    function getXMLGravadoVenda(codVenda: Int64; var chave: string): string;

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da vanda                            |  
|@chave                      |string       | Retorna a chave de acesso da venda         |  

#### Retorno
Retorna uma `string` com o XML da venda. E o parâmetro `chave` recebe o valor da chave de acesso da venda.


### getStrTipoNota

    function getStrTipoNota(codTipoNota: integer): string

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codTipoNota                 |integer      | Código referente ao tipo de nota           |  

#### Retorno
Retorna uma `string` informando o tipo da nota, relativo ao código passado no parâmetro.

### imprimeOPNF
É utilizado quando não se trabalha com impressora fiscal, possibilitando a impressão em uma impressor de bobina de uma operação não fiscal relativa ao código passado no parâmetro `codVenda`.

    procedure imprimeOPNF(codVenda: Int64);  

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da venda                            |  



### trocaTipoVenda
Altera o tipo de uma venda que está em andamento.

    procedure trocaTipoVenda;

### registraSangria
Utilizada na operação de sangria. 

    function registraSangria(cod_caixa_abertura_turno: Integer; cooAnterior: Integer; valorSangria: Real; valorSangriaDinheiro: Real; codAgente: Int64; classificacao: Integer): Integer;

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|cod_caixa_abertura_turno    |Integer      | Código do caixa em aberto                  |  
|cooAnterior                 |Integer      |                                            | 
|valorSangria                |Real         | Valor da operação de sangria               | 
|valorSangriaDinheiro        |Real         | Valor em dinheiro da operação de sangria   | 
|codAgente                   |Int64        | Código do agente que vai realizar a operação | 
|classificacao               |Integer      |                                            | 

#### Retorno
*Descrever o retorno da função*


### getDadosFechamentoNaoECF

    function getDadosFechamentoNaoECF(codVenda: Int64): string

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da venda                            |  

#### Retorno
Retorna um pequeno texto de fechamento, baseado nos dados da venda e levando em consideração as possibilidade que temos diante da legistação para quem não usa ECF. Esse texto pode ser impresso no final ou na parte que cabe às informações complementares do cupon.


### registraUsoValeTroca
Informa ao servidor que determinado cupom de troca foi utilizado.

    procedure registraUsoValeTroca(codVenda: integer);

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da venda                            |  



### marcarTituloRecebidoVenda
Informa ao servidor que determinado título foi recebido durante uma operação não fiscal. 

    procedure marcarTituloRecebidoVenda(codVenda: integer);

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da venda                            |  



### marcaChequeTrocoUtilizado
Informa que um determinado cheque-troco foi utilizado.

    procedure marcaChequeTrocoUtilizado(codVenda: integer);  

#### Descrição dos Parâmetros  
|Parâmetro                   | Tipo        | Descrição                                  |
|----------------------------|-------------|--------------------------------------------|
|codVenda                    |Int64        | Código da venda                            |  
