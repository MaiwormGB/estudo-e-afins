# Frameworks relevantes
> Com foco na criação de uma api rest basica

## EXPRESS

- Framework principal que permite a execução da API no contexto que dessa documentação

### Comando de download
> `npm.cmd install express`

### Como usar

- Após instalado, acesse o framework pelo codigo usando `const express = require("express")`

- Crie uma camada de acesso usando os comandos
> `const app = express()`

> `app.use(express.json());`

- Selecione uma porta para sua API com `const PORT = 3000`. A porta pode ser muadada, apesar da 3000 ser o padrão.

- Finalmente, permita o acesso ao servidor utilizando:

```js
app.listen(PORT, () => {

    console.log(`SUCESSO :: Servidor ativo e rodando na porta http://localhost:3000`)

});
```

*A mensagem do console pode ser o que desejar. O listen que é a parte importante. Lembresse que o servidor precisa estar ligado para ser acessado*

- Crie as requisições http pertinentes às funcionalidades de sua API. A criação de requisições é explicada em [Requisições HTTP](requisicoes.md)

<div class="barrinha"></div>

## PG

- Framework de conexão entre Postgre SQL e Node.JS. Permite o acesso ao banco de dados previamente criado

### Comando de download

>`npm.cdm install pg`

### Como usar

- Após instalado, acesse o framework pelo codigo usando `const { Pool } = require("pg")`. 

- Crie um objeto com as credenciais do banco que quer acessar:

```js
const pool =  new Pool({

    host: 'localhost',
    port: 5432,
    database: '',
    user: 'postgres',
    password: 'aluno'

})
```

- Utilize o *pool* em requisições para realizar as Queries pertinentes à funcionalidade da sua API. A associação entre queries e requisições é explicada em [Requisições HTTP](requisicoes.md)




<style>

    h1{

        color: #5EFF6B;

    }

    h2{

        color: #82d889;

    }

    h3{

        color: #98d19d;

    }

    strong{

        color: #82d889;        

    }

    em{

        color: #98d19d;

    }

    a{

        color: #5EFF6B;
        text-decoration: underline;   

    }

    .barrinha{

        width: 100%; 
        height: 5px; 
        background-color: #5EFF6B; 
        border-radius: 20px;
        margin: 7px;

    }

</style>