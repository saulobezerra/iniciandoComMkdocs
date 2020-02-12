# unitQuery
Essa unit possui dois tipos de dados estruturados personalizados o `TMYQuery` e o `TMyTransaction`, allém de suas funções e procedimentos.

## TMYQuery
É um objeto da classe `TIBQuery` e possui as seguintes propriedades e métodos.

### Propriedades

| Propriedade | Tipo | Descrição |
| ----------- | ---- | --------- |
| queryFilha | boolean | |
| removidaDaMemoria | boolean | |
| vezesUtilidadas | integer | |
| disponivelUso | boolean | |
| ultimoUso | ttime | |
| pediuPraDestruir | boolean | |

### Construtor e Destrutor
    constructor Create(AOwner: TComponent); override;

    destructor destroy; override;
### Métodos
#### Open
*Descrição do método*

    procedure Open;
#### ExecSQL
*Descrição do método*

    procedure ExecSQL
#### openSQLQuery
*Descrição do método*

    procedure openSQLQuery
#### _Open
*Descrição do método*

    procedure _Open
#### _ExecSQL
*Descrição do método*

    procedure _ExecSQL
#### devolvePool
*Descrição do método*

    procedure devolvePool
#### removeDaMemoria
*Descrição do método*

    procedure removeDaMemoria (destroiObjeto: boolean = true);

| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| destroiObjeto | boolean | |

## TMyTransaction
É um objeto da classe `TIBTransaction` e possui as seguintes propriedades.

### Propriedades
| Propriedade | Tipo | Descrição |
| ----------- | ---- | --------- |
| pilhaQuerysFilha | TObjectStack | |

### Construtor e Destrutor
    constructor Create(AOwner: TComponent); override;

    destructor destroy; override;   


## Funções e Procedimentos da Unit

### executaQuery

    function executaQuery(query: TIBQuery; comentario: string = ''): boolean;

#### Descrição de parâmetros
| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| query | TIBQuery | |

#### Retorno
*Descrição do retorno*

### abreQuery

    function abreQuery(query: TIBQuery; comentario: string = ''): boolean;

#### Descrição de parâmetros
| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| query | TIBQuery | |

#### Retorno
*Descrição do retorno*

### criaQuery
Cria uma nova conexão.

    function criaQuery: TMYQuery;
    
#### Retorno
Retorna um objeto TMYQuery.


### newQueryConnection
Cria uma nova instância de `TMYQuery` utilizando a transação da `query` passada por parâmetro.

    function newQueryConnection(banco: TIDatabase = nil; transacao: TIBTransaction = nil; ignoraPool: boolean = false): TMYQuery; overload;

Esta função possui uma sobrecarga que cria uma nova conexão e uma nova transação.

    function newQueryConnection(queryTransaction: TIBQuery): TMYQuery; overload;

#### Descrição de parâmetros
| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| banco | TIDatabase | |
| transacao | TIBTransaction | |
| ignoraPool | boolean | |
| queryTransaction | TIBQuery | |

#### Retorno
Retorna um objeto TMYQuery.


### newQueryConnectionVar
Cria uma instancia de  `TMYQuery` e atribui essa instancia no parâmetro `query` que é passado por referência.

    function newQueryConnectionVar (var query: TMYQuery): TMYQuery;  

#### Descrição de parâmetros
| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| @query | TMYQuery | |

#### Retorno
Retorna uma instancia do TMYQuery no parâmetro `query` passado por referência.


### sqlAberta

    function sqlAberta: string;

#### Retorno
*Descrição do retorno*


### liberaAlteracaoViaQuery

    procedure liberaAlteracaoViaQuery(query: TMYQuery)  

#### Descrição de parâmetros
| Parâmetro | Tipo | Descrição |
| ----------- | ---- | --------- |
| query | TMYQuery | |
