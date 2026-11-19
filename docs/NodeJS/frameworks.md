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

<div class="barrinha"></div>

## JsonWebToken

- Framework que gera uma credencial de login inalteravel, que carrega alguns dos dados do usuario para outras partes pertinentes na aplicação e permite a validação de login antes da realização de metodos.

### Comando de download

`npm.cmd install jsonwebtoken`

### Como usar

- Após instalado, acesse o framework atravez do comando `const jwt = require('jsonwebtoken');`

- Após criar uma validação de email funcional (possivelmente usando o Bcrypt abaixo), utilize o comando abaixo para criar um token:

```js

 const token = jwt.sign(
    {
        dado: fonte
    },
    JWT_SECRET,
    {
        expiresIn: tempo
    }
 )

```

Onde:

- O primeiro objeto são os dados atrelados ao token. Normalmente dados como nome, email, etc. Um atalho, que te salva de ter que buscar o usuario toda vez que quiser esses dados.

- O *JWT_SECRET* é uma senha secreta de criptografia para seu token. Pode ser o que você quiser

- O terceiro objeto carrega outros valores relevantes para o JWT, como, no exemplo, quando o mesmo expira. O valor é sempre uma string, como *'1h'* ou *'20m'* por exemplo

<div class="barrinha"></div>

## Bcrypt

- Framework que permite a codificação de dados sensiveis, como senha, cpf, etc em um *Hash*, que permite a validação das informações sem colocar os dados sensiveis em risco.

### Comando de download

`npm.cmd install bcrypt`

### Como usar

- Após instalado, acesse o framework atravez do comando `const bcrypt = require('bcrypt')`

**Para criar um hash**

`const hash = await bcrypt.hash(senha, 10)`

*o 10 simboliza os rounds de salt, em outras palavras a complexidade do hash. por hora 10 é um numero bom. lembre de salvar o hash no lugar do dado sensivel que estiver criptografando*

**Para comparar o hash**

`const match = await bcrypt.compare(senha, hash)`


<div class="barrinha"></div>

## Dotenv (.env)

- Framework de armazenamento de dados sensiveis internos para teste e organização

### Comando de download

`npm.cmd install dotenv`

### Como usar

- Após instalado, crie um arquivo **.env** na pasta principal (*root*) da **API**

- Dentro da pagina, crie variaveis e atribua os valores que quiser a elas

`DATABASE_HOST = 'localhost'`

- Acesse o framework com `require('dotenv').config();` na pagina que você quer utilizar os dados

- Use `process.env.NOME_DA_VARIAVEL` pra acessar os dados armazenados naquela variavel.

**IMPORTANTE:** *dotenv não é o metodo ideal para esconder segredos realmente sensiveis em uma aplicação. Caso precise armazenar dados particularmente perigosos caso sejam expostos, procure outros metodos mais confiaveis do mercado*

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

    .barrinha{

        width: 100%; 
        height: 5px; 
        background-color: #5EFF6B; 
        border-radius: 20px;
        margin: 7px;

    }

</style>