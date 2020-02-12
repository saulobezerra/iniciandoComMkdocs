# Convenções Java 
## Convenções de Código
Esta seção tem como base o documento de convenções para código Java da *Oracle*.

### Organizações de Arquivos Fonte
Arquivos com mais de 2000 linhas de código são incômodos e devem ser evitados. Além disso, cada arquivo deve conter somente um elemento *top-level* (classe, interface ou enum).

#### Declarações de Classe e Interface
A tabela seguinte descreve as partes de uma declaração de classe ou interface, na ordem em que elas devem aparecer:

|     | **Parte da Declaração da Classe/Interface**                |
|:---:|------------------------------------------------------------|
| 1   | Instrução class ou interface.                              |
| 2   | Definição de classes / interfaces / enumerações aninhadas. |
| 3   | Variáveis estáticas.                                       |
| 4   | Métodos estáticos.                                         |
| 5   | Variáveis de instância.                                    |
| 6   | Construtores.                                              |
| 7   | Métodos de instância.                                      |  

Variáveis e construtores devem ser agrupados por acessibilidade (do mais acessível para o menos acessível). Métodos devem ser agrupados por funcionalidade, com a ressalva de que métodos compartilhados devem aparecer por último na classe. Por exemplo, suponha a seguinte situação:

- Uma classe possui os métodos públicos *pb1(), pb2() e pb3()* e os métodos privados *pv1(), pv2() e pv3()*.
- *pv1()* se relaciona com pb1(); pv2() se relaciona com *pb1() e pb2(); pv3()* se relaciona com pb1() e pb3().   

Uma ordem na qual estes métodos poderiam aparecem na classe seria: *pb1(), pv1(), pb2(), pb3(), pv2(), pv3()*. 

#### Declarações de Enumerações

Na declaração de enumerações, cada constante deve ficar em uma linha separada. Os demais membros de uma enumeração devem seguir as mesmas regras definidas para declaração de classes e interfaces. 

### Indentação

A unidade de indentação deve ser de três espaços e caracteres de tabulação devem ser evitados. No eclipse, estas configurações podem ser definidas da seguinte forma:

- Selecione a opção de menu *Window->Preferences*.
- Na janela de diálogo *Preferences*, selecione o item *Java->Code Style->Formatter*:

![Formatação de estilo de codificação Java](/img/formatacao_estilos_java.jpg "Figura 1: Formatação de estilo de codificação Java")  
<span class="tamanho_fonte"> **Figura 1:** *Formatação de estilo de codificação Java* </span>

- A fim de não alterar o *profile* padrão do *eclipse*, deve-se criar um novo. Para tal, clique no botão *New*..., insira um nome para o *profile* (recomenda-se definir o nome como *Redesoft Eclipse Profile*) e defina que ele deve ter suas configurações inicias baseadas nas configurações padrões do eclipse (*Eclipse [built-in]*).

![Inserindo um novo profile](/img/novo_profile.jpg "Figura 2: Inserindo um novo profile")  
<span class="tamanho_fonte"> **Figura 2:** *Inserindo um novo profile* </span>

- Selecionando a opção para alterar o *profile* recém-criado, obtém-se a janela de diálogo *Profile* *'Redesoft Eclipse Profile'*, que deve ter alguns campos alterados, conforme a Figura 3:

![Alterando o profile recém-criado](/img/alterando_profile.jpg "Figura 3: Alterando o profile recém-criado")  
<span class="tamanho_fonte"> **Figura 3:** *Alterando o profile recém-criado* </span>

#### Comprimento de Linha

Um limite de 80 caracteres por linha faz com que um código seja melhor impresso, mas quase sempre causa incômodo ao desenvolvedor, devido ao espaço muito reduzido. Como impressão de código não será uma atividade tão comum no ambiente de desenvolvimento da Redesoft, um limite de 120 caracteres deverá ser adotado, e linhas com uma quantidade maior de caracteres devem ser evitadas. Esse não é o valor padrão utilizado pelo *eclipse*, devendo ser modificado da seguinte forma:

- Selecione a opção de menu *Window->Preferences*.
- Na janela de diálogo *Preferences*, selecione o item *General->Editors->Text Editors*. O campo *Displayed tab width* deve ser modificado para 3 e o campo *Print margin column* deve ser modificado para 120:

![Mudando a margem de impressão do editor de código](/img/alterando_profile.jpg "Figura 4: Mudando a margem de impressão do editor de código")  
<span class="tamanho_fonte"> **Figura 4:** *Mudando a margem de impressão do editor de código* </span>

#### Quebra de Linhas
Quando uma uma expressão ultrapassar o limite de caracteres de uma linha, ela deve ser quebrada de acordo com as seguintes regras:

- Quebre depois de uma vírgula.
- Quebre antes de um operador.
- Prefira quebras de mais alto nível em vez das de menor nível.
- Alinha a nova linha em relação a sua descendente com uma tabulação.

## Tipos de Dados Permitidos
### Numérico
#### Inteiros
    // Tipo 1: Para valores entre -32768 e 32767
    @Campo(coluna = "IDADE", tipo = "SMALLINT")
    private short idade;

    // Tipo 2: Para valores entre -2.147.483.648 e 2.147.483.647
    @Campo(coluna = "QTD_ITENS_AUDITADOS", tipo = "INTEGER")
    private int quantidadeItensAuditados;

    // Tipo 3: Para valores entre -9.223.372.036.854.775.808 e
    // 9.223.372.036.854.775.807
    @Campo(coluna = "VALOR_MAXIMO", tipo = "BIGINT")
    private long valorMaximo;

    // Tipo 4: Para valores entre -2.147.483.648 e 2.147.483.647
    // utilizamos este tipo para mapeamento de chave primária, com a
    // finalidade de manter compatibilidade com o legado
    @Campo(coluna = "COD_TERMINAL", tipo = "NUMERIC(10,0)", chavePrimaria =  true, generator = true)
    private long codigo;

#### Flutuantes
Obs.: Tipos flutuantes devem ser usados quando não houver necessidade de executar-se comparações entre valores desses tipos, e quando a exatidão não for um critério importante.
Por exemplo: O cálculo dos totais de débitos e de créditos do Razão devem ser exatamente iguais, portanto números de ponto flutuante não devem ser usados.

    // Tipo 1: Para valores entre -1.79*10+38 e 1.79*10+38
    @Campo(coluna = "PESO_BRUTO", tipo = "FLOAT")
    private float pesoBruto;

    // Tipo 2: Para valores entre -3.40*10+308 e 3.40*10+308
    @Campo(coluna = "PESO_BRUTO", tipo = "DOUBLE")
    private double pesoBruto;

#### Moeda
Obs.: Valores do tipo moeda poderão ser representados de três formas possíveis, mostradas abaixo. O terceiro tipo de representação deve ser usado quando se desejar usar valores monetários com exatidão. Neste caso, deve-se multiplicar o valor pela precisão desejada na hora de armazenar e dividir por esta mesma precisão na hora de recuperar.

    // Tipo 1: Para valores entre -214.748,3648 e -214.478,3647
    @Campo(coluna = "SALARIO", tipo = "DECIMAL(10,4)")
    private double salario;

    // Tipo 2: Para valores entre -922.337.203.685.477,5808 e
    // 922.337.203.685.477,5807
    @Campo(coluna = "SALARIO", tipo = "DECIMAL(18,4)")
    private double salario;

    // Tipo 3: Neste caso o intervalo de valores permitido será 
    // obtido através dos valores mínimos e máximos do tipo long
    // divididos por 10 elevado a uma potência de valor igual à
    // precisão para casas decimais desejada. Por exemplo, se é
    // desejado armazenar valores monetários com duas casas
    // decimais, o intervalo de dados permitido será entre
    // -92.233.720.368.547.758,08 e 92.233.720.368.547.758,07
    @Campo(coluna = "SALARIO", tipo = "BIGINT")
    private long salario;

### Boolean
Obs.: Para valores booleanos, dois tipos serão permitidos. O primeiro deve ser utilizado quando compatibilidade for requisito necessário. Para novos projetos, sugere-se a adoção do segundo tipo.

    // Tipo 1:
    // True como “true” e False como “false”.
    @Campo(coluna = "EH_GERENTE", tipo = "VARCHAR(5)")
    private boolean ehGerente;

    // Tipo 2:
    // True como 1 e False como 0.
    @Campo(coluna = "EH_GERENTE", tipo = "SMALLINT")
    private boolean ehGerente;

### Texto

    // Tipo 1: Para ser usado em textos de tamanho variável, com 
    // tamanho máximo de 32767 caracteres.
    @Campo(coluna = "NOME", tipo = "VARCHAR(<tamanho>)")
    private String nome;

    // Tipo 2: Para ser usado em textos de tamanho fixo, com tamanho
    // entre 1 e 32767. Exemplo: CEP, CPF, CNPJ, TELEFONE, etc.
    @Campo(coluna = "CEP",tipo = "CHAR(<tamanho>)")
    private String CEP;

    // Tipo 3: Para ser usado em textos que possam ter mais que
    // 32767 caracteres.
    @Campo(coluna = "CAPITULO", tipo = "BLOB SUB_TYPE TEXT")
    private String capitulo;

### Data e Hora

    // Tipo 1: Para ser usado em datas, sem horas.
    @Campo(coluna = "DATA", tipo = "DATE")
    private String data;

    // Tipo 2: Para ser usado em horas.
    @Campo(coluna = "HORA", tipo = "TIME")
    private String hora;

    // Tipo 3: Para ser usado em junções de datas e horas.
    @Campo(coluna = "DATAHORA", tipo = "TIMESTAMP")
    private String dataHora;

###
    // Dentro do PO
    ....

    public enum GrauDeInstrucao {

        ENSINO_FUNDAMENTAL("Ensino Fundamental"),
        ENSINO_MEDIO("Ensino Médio");

        private String descricao;

        private GrauDeInstrucao(String descricao) {
        this.descricao = descricao;
        } 

        @Override
        public String toString() {
        return this.descricao;
        }
    }

    ....

    @Campo(coluna = "GRAU_DE_INSTRUCAO", tipo = "SMALLINT")
    private short grauDeInstrucao;

## Convenções SQL (Em Classes Java)

Para os métodos que necessitarem realizar consultas direto ao banco, através da montagem de comandos SQL, recomendamos a indentação das consultas, para que as mesmas sejam legíveis tanto no código java, quanto se precisassemos visualizar a mesma no console do SGBD, por exemplo. Os comandos SQL ficam em minúsculo, conforme exemplo:

    public static String[][] consultaIteracoes(long codEmpresa) throws Exception {
    Conexao conexao = null;
    try {
        conexao = Servidor.getConexao(Web.getSessao());

        StringBuilder sql = new StringBuilder();
        sql.append("\n select ");
        sql.append("\n   DI. COD_DEMANDA_INTERACAO CODIGO, ");
        sql.append("\n   DI.STATUS, ");
        sql.append("\n   DI.DATA, ");
        sql.append("\n   DI.HORA ");
        sql.append("\n from ");
        sql.append("\n   DEMANDA_INTERACAO DI ");
        sql.append("\n join ");
        sql.append("\n   DEMANDA D ");
        sql.append("\n    on DI.COD_DEMANDA = D.COD_DEMANDA ");
        sql.append("\n     and DI.COD_DEMANDA = ").append(codDemanda);
        sql.append("\n order by ");
        sql.append("\n   DI.DATA, DI.HORA ");

    return Web.preparaConsulta(Servidor.getCachedRowSet(conexao, 
    sql.toString()));
        } finally {
        Conexao.sclose(conexao);
        }

    }

## Declarações
Uma declaração por linha é recomendada. O nome da variável deve ser legível o suficiente para uma leitura entendível, e se não for possível realiza-se o comentário sobre a razão da criação da mesma. Esta convenção se aplica independente do local onde se realizem declarações (métodos, etc);

    private long codFormaPagto;
    private long codNivel;

    // subtracao de vendas e impostos
    private long totalLiquido;

## Documentação
*Javadoc* é uma ferramenta para a criação de documentação de pacotes, classes, atributos e métodos 

Java a partir do processamento do código fonte com comentários em formato adequado.  

Os comentários para javadoc devem ser iniciados por /** e encerrados por */, podendo incluir diretrizes sobre o que deve ser incluído na documentação gerada (em HTML).  

Os comentários devem estar localizados imediatamente antes da definição da classe ou membro comentado. 

Cada comentário pode conter uma descrição textual sobre a classe ou membro, possivelmente incluindo *tags* HTML, e diretrizes para javadoc.  

Para métodos de negócio (BO), recomendamos sempre a realização de comentários, para que estes sejam facilmente entendidos por outras aplicações que possam se utilizar dos mesmos através de *webservices*. Métodos das classes de PO (*getters and setters*) não precisam de documentação.  

O limite de número de caracteres para um nome de tabela é de 22 caracteres, devido a criação do generator, GLOB_COD_[TABELA]. O limite para o tamanho de um campo normal da tabela, exceto a chave primária é de 32 caracteres.  

O espaçamento entre parenteses, chaves e operadores deve seguir o padrão:

    if (codigo == 0 && ehCliente) {
        // usar chaves somente se tiver mais de uma linha
        // de instrução.
    }
