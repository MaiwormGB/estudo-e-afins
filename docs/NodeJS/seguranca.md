# Autenticações de segurança
> Para validar APIs

## Middleware

- Camada intermediária que **valida as credenciais do usuario** com base no token gerado pelo JWT

- Crie uma pasta chamada **middlewares** e um arquivo chamado **authMiddleware.js**

*Se certifique de que JWT e dotenv já estejam instalados*

**EX**

```js
const jwt = require('jsonwebtoken')
require('dotenv').config()

function authMiddleware(req, res, next) {

    const authHeader = req.headers.authorization;

    if (!authHeader){

        return res.status(401).json({
            mensagem: "ERRO:: TOKEN NÃO INFORMADO"
        })

    }

    const token = authHeader.split(' ')[1];

    try {

        const decoded = jwt.verify(token, process.env.JWT_SECRET);

        req.usuario = decoded;

        next();
    } catch(error){

        return res.status(401).json({
            mensagem: "ERRO:: TOKEN INVALIDO"
        })

    }
    
}

module.exports = authMiddleware;
```

onde:

- **authHeader** pega o header da requisição, onde o token deve ser informado pelo usuario.

- O token é usado atravez do **jwt.verify** e confirma que a requisição pode continuar com **next()**

*Para utilizar o middleware em uma rota, coloque o mesmo entre o caminho e a parte final da rota*

**EX**

`app.post('/funcionario', authMiddleware, async (req, res) => {...})`

*Tambem lembre de puxar a função pro arquivo onde as rotas se encontram (server.js)*

**EX**

`const authMiddleware = require('../middlewares/authMiddleware')`


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