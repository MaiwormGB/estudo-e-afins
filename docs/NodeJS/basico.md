# CONCEITOS BASICOS DE NODE.JS
> Explicação da linguagem e fundamentos iniciais

## // O que é Node.js? //

A linguagem foi criada com o objetivo de permitir que codigos js
rodem fora do navegador. Permite a criação de APIs e Backends para aplicações, alem de automações de tarefas

<div class="barrinha"></div>
<div class="barrinha"></div>


## // Como instalar o Node.JS //

Basta || 

- Acessar o [site oficial do Node.JS](https://nodejs.org/pt-br)  

- Procurar por "Other downloads"

- Escolha "Installing Node.JS via package manager" e procure seu sistema operacional

Essa forma de download permite melhor controle de versão e de desinstalação caso algum problema aconteça.

***A Extenção usada pelo Node é .js, geralmente chamado de server.js***

<div class="barrinha"></div>
<div class="barrinha"></div>

## // Módulos do Node.JS //

Ao começar um arquivo node, você deve começar o arquivo com

<span class = "codigo">import {} from 'node:*MODULO'</span>

Existem varios modulos que podem entrar no lugar de *MODULO, onde cada um possui sua própria função baseado naquilo que você quer realizar com a aplicação

### Alguns modulos notorios

<span class = "codigo">// Node.http</span> <br> 
- Criação de servidores HTTP. Nesse caso em especifico, é preciso colocar { create server } logo após o import para auxiliar em tal criação<br>

<span class = "codigo">// Node.fs</span> <br>
- Sigla para "File Sistem". Permite a manipulação de arquivos (criar, ler, deletar, etc) <br>

<span class = "codigo">// Node.crypto</span> <br>
- Usado para lidar com criptografia <br>


<style>

    h1{

        color: #5EFF6B;

    }

    h2{

        color: #82d889;

    }

    .barrinha{

        width: 100%; 
        height: 5px; 
        background-color: #5EFF6B; 
        border-radius: 20px;
        margin: 7px;

    }

    .codigo{

        background-color: #353535;
        padding: 4px 15px;
        border-radius: 10px;
        color: #a0d1a4;

    }

</style>

