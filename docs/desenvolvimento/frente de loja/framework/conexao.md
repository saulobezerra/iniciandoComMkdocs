# Conexões
## Conexão com o banco de dados
### Criando uma conexão
Para criar uma conexão com o banco de dados, deve-se primeiro criar uma variável `query` do tipo [`TMYQuery`](../../units/unitQuery/#tmyquery) e atribuir a ela o retorno da função `criaQuery` em que é criada uma nova conexão e uma nova transação. Importante que sempre que criar uma `query` abrir um bloco `try` para tratar possíveis exceções que possam ser geradas durante a manipulação da variável `query` e um bloco `finaly` para encerrar a conexão.

    var
        query: TMYQuery;
    begin
        query := criaQuery;
        try
            // Seu código
        finaly
            query.devolvePool;
    end
  
Se o código dentro do `try` gera uma outra exceção, deve-se criar um novo bloco `try` - `except` para o tratamento dessa exceção: 

    var
        query: TMYQuery;
    begin
        query := criaQuery;
        try
            try
                // Código que gera exceção
                raise Exception.create ('minha exceção');
            except
                on e: Exception do 
                    begin
                    raise Exception.create ('ocorreu algum erro ' + e.message);
                    end;
            end;
        finaly
            query.devolvePool;
    end

#### Usando o comando `with`
Você também pode trabalhar com `query` utilizando o comando `with`.  

    begin
        with criaQuery do 
            try
                Close;      // Fecha a última sql executada
                SQL.Clear;  // Limpa o comando sql 
                SQL.Add('select first 5 * from produto');  // Adiciona à sql o texto passado por parâmetro
                Open;       // Espera o retorno da sql executada
                while not eof do 
                    begin
                    showmessage(FiledByName('nome').AsString)
                    Next
                    end;
                
            finally
                devolvePool;
            end;
    end  

No trecho de código acima uma instancia do objeto [`TMYQuery`](../../units/unitQuery/#tmyquery) está sendo passada através do [`criaQuery`](../../units/unitQuery/#criaquery) ao bloco regido pelo comando `with` e com isso temos um acesso implícito ao objeto dentro do bloco, o que nos permite ter acesso ás `functions` e `procedures` do objeto `TMYQuery` sem a necessidade do uso do 'ponto' `.`. Por exemplo, onde usamos `query.devolvePool` usa-se apenas [`devolvePool`](../../units/unitQuery/#devolvepool).  
No comando `while` percorremos o resultado da consulta, que nesse caso será o conteúdo das cinco primeiras linhas da tabela produto. E exibimos os valores do campo `nome` com `showmessage` e utilizamos o `Next` para avançar para a próxima linha.  

#### Usando `newQueryConnectionVar`
Ainda no código anterior, se em determinado ponto dentro do `try`, precisasse passar o valor ou a referência de `query` para uma função, teriamos um problema, pois não utilizamos uma variável para receber a referência do objeto passada por `criaQuery`. E uma possível solução seria:  

    var
        query: TMYQuery;
    begin
        with newQueryConnectionVar(query) do
            try
                Close;
                minhaFuncao(query);
            finally
                devolvePool;
            end;
    end

Em que o [`newQueryConnectionVar`](../../units/unitQuery/#newqueryconnectionvar) cria uma instancia de  `TMYQuery` e atribui à variável `query`. Isso acontece porque a variável `query` é passado por referência para `newQueryConnectionVar`.  

#### Usando `newQueryConnection`
No exemplo a seguir vemos o uso de `newQueryConnection` em uma operação de `update`.  

    var
        query: TMYQuery;
        query2: TMYQuery;
    begin
        with newQueryConnectionVar(query) do
            try
                query2 := newQueryConnection(query)
                Close;
                sql.clear;
                sql.add('select first 5 * from produto')
                open;
                while not eof do
                    begin
                    query2.close;
                    query2.sql.clear;
                    query2.sql.add('update product set nome = ''novo nome'' where cod_produto = ' + FieldByName('cod_produto').AsString);
                    query2.ExecSQL;
                    Next;
                    end
                query.Transaction.CommitRetaining;
            finally
                query2.devolvePool;
                query.devolvePool;
            end;
    end 

O [`newQueryConnection`](../../units/unitQuery/#newqueryconnection) cria uma nova instancia de `TMYQuery` utilizando a transação da `query` passada por parâmetro, assim a nova query criada (`query2`) é identificada como uma query filha e que quando devolvida ao `pool` sua transação não será fechada porque ela não é reponsável pela transação mãe, que nesse caso é da variável `query` que foi instanciada por `newQueryConnectionVar`, mas que também poderia ter sido instaciada utilizando `criaQuery`.
