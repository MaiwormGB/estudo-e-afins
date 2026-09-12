# ELEMENTOS PRINCIPAIS DE HTML
> Repertorio de elementos e suas sintaxes

## Formatação de texto

### Titulos | <span class = "codigo"> &lt;h<sup>n</sup>&gt; </span>
> Possuem tag de fechamento

Hierarquia de 6 a 1, onde quanto menor o numero, maior a importancia do titulo.

<div class = "barrinha"></div>

### Paragrafos | <span class = "codigo"> &lt;p&gt; </span>
> Possuem tag de fechamento

Blocos de texto simples. possuem formação interna

---

- <span class = "codigo"> &lt;strong&gt; </span>

- <span class = "codigo"> &lt;em&gt; </span>
- <span class = "codigo"> &lt;u&gt; </span>
- <span class = "codigo"> &lt;sup&gt; </span>
- <span class = "codigo"> &lt;sub&gt; </span>
- <span class = "codigo"> &lt;del&gt; </span>
- <span class = "codigo"> &lt;mark&gt; </span>
- <span class = "codigo"> &lt;q&gt; </span>
- <span class = "codigo"> &lt;cite&gt; </span>
>todos possuem tag de fechamento

<div class = "barrinha"></div>

### Citações

<span class = "codigo"> &lt;q&gt; </span>
> possui tag de fechamento

> Pode ser usada como formatação interna para &lt;p&gt;

- Coloca o texto entre aspas (" ")

- Utilize <span class = "codigo"> cite = " " </span> como atributo para anexar links de referencias

<span class = "codigo"> &lt;blockquote&gt; </span>
> possui tag de fechamento

- Mesmo que &lt;q&gt;, porem coloca o texto em uma caixa. usada para textos maiores

<span class = "codigo"> &lt;cite&gt; </span>
> possui tag de fechamento

> Pode ser usada como formatação interna para &lt;p&gt;

- Usada para anexar links externos de citações. Coloca o texto em italico

<span class = "codigo"> &lt;footer&gt; </span>
> possui tag de fechamento

- Coloca uma nota de rodape em um bloco de texto

<div class = "barrinha"></div>

### Quebra de linhas

<span class = "codigo"> &lt;br&gt; </span>
> sem tag de fechamento

- Força uma linha a ser pulada

<span class = "codigo"> &lt;hr&gt; </span>
> sem tag de fechamento

- Cria uma linha horizontal que separa o conteudo

<div class = "barrinha"> </div>

### Display de codigo

<span class = "codigo"> &lt;code&gt; </span>
> possui tag de fechamento

- Formata o codigo corretamente e o aninha em um bloco destacado do resto

<span class = "codigo"> &lt;pre&gt; </span>
> possui tag de fechamento

- Mantem a formatação de um texto já formatado anteriormente

<div class = "barrinha"> </div>

### Listas

existem 3 tipos de lista

<span class = "codigo"> &lt;ul&gt; </span>
> possui tag de fechamento

- Declara uma lista desordenada, onde a ordem não importa

<span class = "codigo"> &lt;ol&gt; </span>
> possui tag de fechamento

- Declara uma lista ordenada, onde a ordem importa

<span class = "codigo"> &lt;dl&gt; </span>
> possui tag de fechamento

- Declara uma lista de definição, onde cada item possui uma definição atrelada a si

---

Existem tambem os elementos internos das listas

<span class = "codigo"> &lt;li&gt; </span>
> possui tag de fechamento

- Define um elemento da lista

<span class = "codigo"> &lt;dt&gt; </span>
> possui tag de fechamento

- Define um termo de uma lista de definição

<span class = "codigo"> &lt;dd&gt; </span>
> possui tag de fechamento

- Atrela a definição de um termo

<div class = "barrinha"> </div>

### Tabelas / Matrizes | <span class = "codigo"> &lt;table&gt; </span>
> Possui tag de fechamento

Separam conteudo em fileiras e colunas para organização

---

<span class = "codigo"> &lt;tr&gt; </span>
> Possui tag de fechamento

- Define uma fileira 

<span class = "codigo"> &lt;td&gt; </span>
> Possui tag de fechamento

- Define uma coluna

*Os elementos <span class = "codigo"> rowspan = " " </span> e <span class = "codigo"> colspan = " " </span> indicam que uma determinada celula ocupa mais de uma fileira ou coluna respectivamente*

---

<span class = "codigo"> &lt;th&gt; </span>
> Possui tag de fechamento

- Define um cabeçalho para uma coluna. Seu texto fica em negrito e são alinhados no centro

<span class = "codigo"> &lt;caption&gt; </span>
> Possui tag de fechamento

- Define uma descrição simples de uma tabela

<div class = "barrinha"> </div>

### Hyperlinks | <span class = "codigo"> &lt;a&gt; </span>
> Possui tag de fechamento

Permite atrelar um link a um texto via o atributo <span class = "codigo"> href = "" </span>

O atributo pode receber tanto links completos (Com http) ou caminhos relativos, que apenas funcionam para acessar arquivos dentro do mesmo diretório da página.

O atributo tambem pode ser utilizado para redirecionar o usuario a um elemento da mesma pagina, caso o id do mesmo seja colocado antecedido por # dentro do atributo dentro do atributo

> Outros elementos podem ser colocados dentro da tag &lt;a&gt; para torna-los clicaveis com links tambem, como imagems e divs

---

<span class = "codigo"> download</span> Baixa os conteudos do link

<span class = "codigo"> rel</span> Especifica que tipo de conteudo esta presente no link (como uma stylesheet, por exemplo)

<span class = "codigo"> tittle </span> Define um titulo ao elemento. Após ficar com o mouse em cima do elemento por alguns segundos, o titulo será visivel

<div class = "barrinha"> </div>

### Video / Audio | <span class = "codigo"> &lt;video&gt; / &lt;audio&gt; </span>
> Ambos possuem tags de fechamento

Permitem colocar videos e audios na pagina, utilizando do atributo <span class = "codigo"> src</span> para especificar o link do video, podendo ser tanto interno quanto externo


---

<span class = "codigo"> control</span> disponibiliza controles de media pre prontos (pause, skip, etc)

<span class = "codigo"> atoplay</span> faz o conteudo tocar assim que a pagina carregar

<span class = "codigo"> loop</span> faz o conteudo ficar em loop

<span class = "codigo"> preload</span> garante que o conteudo vá carregar primeiro

<span class = "codigo"> aposter</span>  atrela uma imagem ao video enquanto o mesmo não esta tocando

---

<span class = "codigo"> &lt; source&gt; </span> 
> não possui tag de fechamento

Permite a disponibilização de varias fontes de video e audio para garantir a acessibilidade em diferentes navegadores. Cada source precisa de um<span class = "codigo">src</span> e <span class = "codigo">type</span> proprio



<style>

    h1{

        color: #ff765e;

    }

    h2{

        color: #eb938d;

    }

    .barrinha{

        width: 100%; 
        height: 5px; 
        background-color: #ff765e; 
        border-radius: 20px;
        margin: 7px;

    }

    .codigo{

        background-color: #353535;
        padding: 4px 15px;
        border-radius: 10px;
        color: #d1a2a0;
        margin-left: 10px;

    }

    .bloco_codigo{

        background-color: #353535;
        padding: 15px;
        border-radius: 10px;
        color: #d1a2a0;

    }

</style>