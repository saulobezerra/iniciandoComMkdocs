# Convenções Delphi

## Indentação
A unidade de indentação deve ser de três espaços e caracteres de tabulação
devem ser evitados. No *Delphi 7*, estas configurações podem ser definidas da seguinte
forma:  

- Selecione a opção de menu *Tools -> Editor Options*.
- Na janela de diálogo *Editor Properties*  selecione a aba *Display*.
    ![Mudando margem de impressão do editor ](/img/delphi7_editorProperties.jpg "Figura 5: Mudando margem de impressão do editor")  
    <span class="tamanho_fonte"> **Figura 5:** *Mudando margem de impressão do editor* </span>

- Localize o combo *Rigth margin* e altere o seu valor para 100.
- Ainda na janela Editor Properties, selecione a aba Source Options.
    ![Ativando a identação manual ](/img/delphi7_editorProperties_sourceOptions.jpg "Figura 6: Ativando a identação manual")  
    <span class="tamanho_fonte"> **Figura 6:** *Ativando a identação manual* </span>

- Localize os campos *Block indent* e *Tab stops* e altere os seus valores para 1. Estes valores facilitam o trabalho de organização do código, além de atribuir ao desenvolvedor a tarefa de indentação manual.

## Tipos de Telas
Devido à padronização da interface Delphi, existem somente dois tipos de telas permitidos, e o desenvolvedor deverá criar seus formulários herdando de uma delas. Para criação de telas no Delphi, deve-se primeiro escolher o tipo de formulário desejado. Existem dois, o chamado *Formulário de Cadastro Padrão*, usado para telas do tipo cadastro básico, pois a mesma contém botões para Salvar, Limpar, Excluir, Localizar, Imprimir, Primeiro(|<), Anterior(<), Próximo(|>) e Ultimo(|>), e o *Formulário Padrão* que pode ser utilizado para construir qualquer outro tipo de tela. Isto é necessário para manter a padronização das telas em características como tamanho e fonte dos rótulos, cores dos painéis, etc. A maioria das telas pode ser construída fazendo uma cópia de uma tela semelhante, assim se pode aproveitar eventos comuns, como o de limpar, localizar, imprimir, quantidade de rótulos e campos, etc. Até se recomenda essa cópia pois facilita a regulagem da posição e do tamanho dos componentes na tela, o que é uma convenção a ser obedecida.  

## Padrão Visual
A princípio, para manter o padrão visual, siga as propriedades contempladas na aplicação de referência. Como ajuda extra, temos abaixo uma descrição das propriedades que devem ser utilizadas.

### Dimensões da Tela  
O tamanho ideal (não obrigatório) para as telas é 586 (altura) x 777 (largura), pois caracteriza uma tamanho pouco menor do que a tela principal do projeto: *frmMenuPrincipalERP* (*Tfrm_MPrincipalERP*) (**Figura 7**);

![Menu principal ](/img/b2click_menuPrincipal.jpg "Figura 7: Menu principal")  
<span class="tamanho_fonte"> **Figura 7:** *Menu principal* </span>

### Cadastro Padrão
Todas as telas de cadastro, devem herdar de formCadastroPadrao (*Tfrm_FormCadastroPadrao*) (**Figura 8**);

![Cadastro padrão ](/img/cadastro_padrao.jpg "Figura 8: Tela de cadastro padrão")  
<span class="tamanho_fonte"> **Figura 8:** *Tela de cadastro padrão* </span>

#### Botão Imprimir
Caso seja necessário adicionar o botão imprimir, o mesmo deve ser adicionado ao painel de funcionalidades do cadastro padrão (**Figura 9**), e os eventos no trecho de código **Exemplo** deveram ser adicionados a tela;

![Cadastro padrão ](/img/botao_imprimir.jpg "Figura 9: Tela de cadastro padrão com o botão imprimir")  
<span class="tamanho_fonte"> **Figura 9:** *Tela de cadastro padrão com o botão imprimir* </span>

    // Exemplo:  Eventos do botão imprimir
    procedure TFrm_NomeDaTela.botaoImpimirMouseEnter(Sender: TObject);
    begin
        inherited
        botaoImprimir.BackColor := corBotaoEvento;
    end

    procedure TFrm_NomeDaTela.botaoImpimirMouseLeave(Sender: TObject);
    begin
        inherited
        botaoImprimir.BackColor := clWhite;
    end

#### 
    procedure TFrm_NomeDaTela.FormShow(Sender: TObject);
    begin
        inherited
        botaoImprimir.GlowColor := corBotao;
    end

### Telas Padrão
Telas que não possuem as operações básicas de cadastro, devem herdar de frmPadrao (*TFormularioPadrao*) (**Figura 10**);

![Tela padrão ](/img/tela_padrao.jpg "Figura 10: Tela padrão")  
<span class="tamanho_fonte"> **Figura 10:** *Tela padrão* </span>

### Caixas de Texto
- Espaçamento vertical entre as caixas de texto (*TRxCalcEdit, TComboBox, TdateEdit …*), devem ser de 5 espaços (**Figura 11**); 
- Espaçamento horizontal entre as caixas de texto e os rótulos das mesmas (*TLabel*), devem ser de 7 espaços (**Figura 11**);

![Cadastro produto ](/img/cadastro_produto.jpg "Figura 11: Cadastro de produto")  
<span class="tamanho_fonte"> **Figura 11:** *Cadastro de produto* </span>

### Rótulos
#### Simples
- Tamanho: 10
- Tipo: tahoma
- cor: clBlack
- height: 17

#### Campos Obrigatórios
- Tamanho: 10
- Tipo: tahoma
- cor: clBlue
- height: 17
- Obs.: O programador deve validar a obrigatoriedade  dos campos referentes a tais rótulos

#### Dinâmicos
- Tamanho: 10
- Tipo: tahoma
- cor: clGreen
- height: 17
- Obs.: Os valores destes rótulos são apagados quando se limpa a tela através da procedure limpaComponentes(Self)

#### Título GRID
- Tamanho: 10
- Tipo: tahoma
- Cor: clBlack
- Height: 17
- Style: [fsBold]

![Cadastro produto ](/img/cadastro_contaCorrente.jpg "Figura 12: Cadastro de conta corrente")  
<span class="tamanho_fonte"> **Figura 12:** *Cadastro de conta corrente* </span>

## Declarações
Uma declaração por linha é recomendada. O nome da variável deve ser legível o suficiente para uma leitura entendível, e se não for possível realiza-se o comentário sobre a razão da criação da mesma. A declaração deve seguir o padrão *“nomeVariavel: tipo;”* Esta convenção se aplica independente do local onde se realizem declarações (métodos, etc);

    ficha: TNo;
    telefone: TLista;

    //casas decimais utilizadas
    fatorArredondamento: Integer;

## Atribuições
Uma atribuição por linha é recomendada obedecendo ao seguinte padrão *“nomeVariavel := valor”*.

    ficha := TNo.Create('FichaPO');
    contador := 0;

## Documentação
Comentários são sempre bem vindos, pois os mesmos facilitam o entendimento do código. Para comentários em delphi utilizamos “//” ou “{}”. Os comentários devem estar localizados imediatamente antes da definição da funcionalidade(classe, função ou procedimento) comentado.

## Outras Convenções

- Retornos de consultas devem ter o título com apenas a primeira palavra em maiúsculo;

- Qualquer campo de entrada de valor como campos de código, data, texto, combobox, checkbox devem ter seu nome começando com o sufixo `campo<Cod><nome representativo>` ou `campo<nome representativo>`. Já os labels devem ter o nome começam com o sufixo `label<nome representativo>`, assim como `grid<nome representativo>`;

- Indentação:  
    - Blocos de código que estiverem entre *begin end* devem ser alinhados com o *begin end*, exemplo:  
>        begin
>        mess('Arquivo de entidades não definido corretamente');
>        Exit
>        end;
    - Blocos *if then, try except, while do, with do* devem dar um espaçamento de tamanho três entres as demais linhas:
>        if campoCodBanco.AsInteger <> o then
>           begin
>           banco := BancoPO.Create;
>           banco.codigo := campoCodBanco.AsInteger;
>           end;
    