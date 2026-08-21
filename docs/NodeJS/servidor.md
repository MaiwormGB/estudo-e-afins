# CRIANDO UM ARQUIVO NODE.JS
> Disponibilizando um local onde a api sera depositada

Como mencionado anteriormente, o primeiro passo para a criação de uma API com node é justamente criar um arquivo com a extensão ". JS", geralmente chamado de <span class = "codigo"> server.js. </span>

Após isso, é nescessario declarar que aquele arquivo é de Node atravez do comando <span class = "codigo"> import {} from 'MODULO';</span>, já explicado no documento anterior.

**API UTILIZADA** - *fastify*

Importar a API com:

<span class = "codigo"> import{fastify} from 'fastify';</span>

usada atravez de <span class ="codigo"> const server = fastify();</span>

**A API PRECISA SER INSTALADA NA MAQUINA ATRAVEZ DO CODIGO:**

<span class ="codigo"> npm install fastify</span>

>Em caso de bloqueios que impedem o uso normal do npm, utilize .cmd na frente para permitir seu uso


## METODOS HTTP
> Tambem chamado de CRUD

*Permitem criar, ler, atualizar e deletar arquivos salvos*

- <span class ="codigo"> server.get()</span> - Metodo de leitura

- <span class ="codigo"> server.update()</span> - Metodo de atualização

- <span class ="codigo"> server.post()</span> - Metodo de criação

- <span class ="codigo"> server.delete()</span> - Metodo de deletar

**METODOS RECEBEM SEMPRE UM LINK HTTP E UMA FUNÇÃO**

*EX*

<div class = "bloco_codigo"> 

    //server.post('/videos', (request, reply) => {})

</div>




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

    .bloco_codigo{

        background-color: #353535;
        padding: 15px;
        border-radius: 10px;
        color: #d1a2a0;

    }

</style>

