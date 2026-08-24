# CONCEITOS BASICOS DE HTML
> Explicação da linguagem e fundamentos iniciais

## // O que é HTML? //

Acronimo de "HyperText Markup Language", HTML foi criado para permitir a criação e comparilhamento de paginas (originalmente, artigos cientificos) atravez de uma rede , seja ela local, como na época de sua criação, ou seja ela a própria internet, na forma de paginas e sites acessiveis por links. <br>

Atualmente, a linguagem é utilizada para criar a estrutura geral de uma aplicação web (ou site), dispondo de elementos pré prontos (como botões, links e paragráfos) que posteriormente receberão melhorias visuais (CSS) e funcionalidades (JavaScript). <br>

<div class="barrinha"></div>
<div class="barrinha"></div>


## // Configurando um documento HTML //

A extrutura base de um documento HTML segue a seguir:

<div class = "bloco_codigo">

&lt;!DOCTYPE html&gt; <br>
&lt;html lang="*LINGUAGEM"&gt; <br>
&lt;head&gt; <br>
&nbsp;&nbsp;&nbsp;&nbsp;&lt;meta charset="UTF-8"&gt; <br>
&nbsp;&nbsp;&nbsp;&nbsp;&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; <br>
&nbsp;&nbsp;&nbsp;&nbsp;&lt;title&gt;*TITULO&lt;/title&gt; <br>
&nbsp;&nbsp;&nbsp;&nbsp;&lt;link rel="stylesheet" href="styles/style.css"&gt; <br>
&lt;/head&gt; <br>
&lt;body&gt; <br>
<br>
&nbsp;&nbsp;&nbsp;*CONTEUDO <br>
<br>
&lt;/body&gt; <br>
&lt;/html&gt;

</div>

<br> **Onde ||**

---

<span class = "codigo"> &lt;!DOCTYPE html&gt; </span> <br>

</> Indica ao navegador que aquele é um documento HTML e que deve ser interpretado como tal.

---

<span class = "codigo"> &lt;html lang="*LINGUAGEM"&gt;</span> <br> & <br> <span class = "codigo"> &lt;/html&gt;</span>

</> Indicam o inicio e o final da pagina HTML. Tudo que compor a pagina será colocado entre essas duas tags. O atributo "lang" funciona para definir uma linguagem para a pagina em navegadores, como "pt-br" ou "en"

---

<span class = "codigo"> &lt;head&gt; </span> <br> & <br> <span class = "codigo"> &lt;/head&gt; </span>

</> Indicam o inicio e o final do cabeçalho da pagina, dedicado a caracteristicas da pagina que o usuario não deve ver (com excessão do título). Alguns exêmplos de caracteristicas são metadados, como <span class = "codigo"> &lt;meta charset="UTF-8"&gt; </span> e <span class = "codigo"> &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; </span> e links de arquivos externos como <span class = "codigo"> &lt;link rel="stylesheet" href="styles/style.css"&gt; </span>

---

<span class = "codigo"> &lt;body&gt;</span> <br> & <br> <span class = "codigo"> &lt;/body&gt;</span>

</> Indicam o inicio e o final do corpo da pagina, onde todos os elementos visiveis ao usuario serão colocados, como botões, imagens, links, etc.

<div class="barrinha"></div>
<div class="barrinha"></div>


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

