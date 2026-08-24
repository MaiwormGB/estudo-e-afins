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