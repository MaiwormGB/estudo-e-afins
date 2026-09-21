# Resumo de Node com Express — API REST e CRUD

Guia prático de Express + PostgreSQL para desenvolvimento web.

O foco é entender o fluxo:

```text
HTTP → Express → PostgreSQL → Express → HTTP
```

---

# 1. O que é uma API?

Uma API permite que diferentes aplicações se comuniquem.

No caso de uma API web:

```text
Frontend
   ↓
HTTP Request
   ↓
Express / Node.js
   ↓
PostgreSQL
   ↓
Express / Node.js
   ↓
HTTP Response
   ↓
Frontend
```

Exemplo:

```text
GET /produtos
```

A API recebe a requisição e executa:

```sql
SELECT * FROM produtos;
```

Podendo retornar:

```json
[
    {
        "id": 1,
        "nome": "Espada"
    },
    {
        "id": 2,
        "nome": "Escudo"
    }
]
```

---

# 2. Criando o projeto

Criar uma pasta:

```bash
mkdir minha-api
cd minha-api
```

Inicializar o Node:

```bash
npm init -y
```

Instalar Express e PostgreSQL:

```bash
npm install express pg
```

Estrutura inicial:

```text
minha-api/
├── node_modules/
├── package.json
├── package-lock.json
└── src/
     └── server.js
```

---

# 3. Criando o servidor Express

No `server.js`:

```js
const express = require("express");

const app = express();

app.use(express.json());

const PORT = 3000;

app.listen(PORT, () => {
    console.log(`Servidor rodando na porta ${PORT}`);
});
```

Executar:

```bash
node src/server.js
```

---

# 4. `express.json()`

Esta linha:

```js
app.use(express.json());
```

permite que o Express interprete requisições contendo JSON.

Exemplo:

```json
{
    "nome": "Espada",
    "preco": 100
}
```

Os dados ficam disponíveis em:

```js
req.body
```

---

# 5. Criando uma rota GET

```js
app.get("/produtos", (req, res) => {

    res.json({
        mensagem: "Lista de produtos"
    });

});
```

Acessando:

```text
GET /produtos
```

Resposta:

```json
{
    "mensagem": "Lista de produtos"
}
```

---

# 6. `req` e `res`

Nas rotas:

```js
app.get("/produtos", (req, res) => {
});
```

`req` = **request**

Contém informações enviadas pelo cliente.

`res` = **response**

É utilizado para enviar a resposta.

Exemplo:

```js
app.get("/teste", (req, res) => {

    console.log(req);

    res.json({
        mensagem: "Olá!"
    });

});
```

---

# 7. Métodos HTTP

Os principais métodos utilizados em um CRUD são:

| Método | Uso |
|---|---|
| `GET` | Buscar |
| `POST` | Criar |
| `PUT` | Atualizar |
| `PATCH` | Atualizar parcialmente |
| `DELETE` | Excluir |

Uma API CRUD normalmente possui:

```text
POST   /produtos
GET    /produtos
GET    /produtos/:id
PUT    /produtos/:id
DELETE /produtos/:id
```

---

# 8. Criando a conexão com PostgreSQL

Instalação:

```bash
npm install pg
```

No `server.js`:

```js
const { Pool } = require("pg");

const pool = new Pool({
    host: "localhost",
    port: 5432,
    database: "minha_api",
    user: "postgres",
    password: "senha"
});
```

O `Pool` gerencia conexões com o PostgreSQL.

---

# 9. Fazendo uma query

```js
const resultado = await pool.query(
    "SELECT * FROM produtos"
);
```

Os registros ficam em:

```js
resultado.rows
```

Exemplo:

```js
console.log(resultado.rows);
```

---

# 10. Primeiro CRUD

Considere esta tabela:

```sql
CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    complemento VARCHAR(200) NOT NULL
);
```

---

# 11. CREATE — POST

Criar produto:

```js
app.post("/produtos", async (req, res) => {

    try {

        const { nome, complemento } = req.body;

        const resultado = await pool.query(
            `
            INSERT INTO produtos (nome, complemento)
            VALUES ($1, $2)
            RETURNING *
            `,
            [nome, complemento]
        );

        res.status(201).json(resultado.rows[0]);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao criar produto"
        });

    }

});
```

Requisição:

```http
POST /produtos
```

Body:

```json
{
    "nome": "Espada",
    "complemento": "Espada longa"
}
```

Resposta:

```json
{
    "id": 1,
    "nome": "Espada",
    "complemento": "Espada longa"
}
```

---

# 12. Por que `$1` e `$2`?

A query:

```js
`
INSERT INTO produtos (nome, complemento)
VALUES ($1, $2)
`
```

recebe:

```js
[nome, complemento]
```

Assim:

```text
$1 → nome
$2 → complemento
```

Isso é chamado de **query parametrizada**.

Evite:

```js
`INSERT INTO produtos (nome) VALUES ('${nome}')`
```

Prefira:

```js
`
INSERT INTO produtos (nome)
VALUES ($1)
`
```

com:

```js
[nome]
```

---

# 13. READ — GET

Buscar todos os produtos:

```js
app.get("/produtos", async (req, res) => {

    try {

        const resultado = await pool.query(
            "SELECT * FROM produtos"
        );

        res.json(resultado.rows);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao buscar produtos"
        });

    }

});
```

---

# 14. GET por ID

A URL:

```text
GET /produtos/5
```

possui um parâmetro:

```text
:id
```

Criamos a rota:

```js
app.get("/produtos/:id", async (req, res) => {

    try {

        const { id } = req.params;

        const resultado = await pool.query(
            `
            SELECT *
            FROM produtos
            WHERE id = $1
            `,
            [id]
        );

        res.json(resultado.rows[0]);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao buscar produto"
        });

    }

});
```

---

# 15. `req.params`

Para:

```text
/produtos/5
```

e:

```js
app.get("/produtos/:id", ...)
```

podemos obter:

```js
req.params.id
```

Resultado:

```text
"5"
```

---

# 16. Verificando se o registro existe

Uma melhoria importante:

```js
if (resultado.rows.length === 0) {

    return res.status(404).json({
        erro: "Produto não encontrado"
    });

}
```

Exemplo completo:

```js
app.get("/produtos/:id", async (req, res) => {

    try {

        const { id } = req.params;

        const resultado = await pool.query(
            `
            SELECT *
            FROM produtos
            WHERE id = $1
            `,
            [id]
        );

        if (resultado.rows.length === 0) {

            return res.status(404).json({
                erro: "Produto não encontrado"
            });

        }

        res.json(resultado.rows[0]);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao buscar produto"
        });

    }

});
```

---

# 17. UPDATE — PUT

```js
app.put("/produtos/:id", async (req, res) => {

    try {

        const { id } = req.params;
        const { nome, complemento } = req.body;

        const resultado = await pool.query(
            `
            UPDATE produtos
            SET
                nome = $1,
                complemento = $2
            WHERE id = $3
            RETURNING *
            `,
            [nome, complemento, id]
        );

        if (resultado.rows.length === 0) {

            return res.status(404).json({
                erro: "Produto não encontrado"
            });

        }

        res.json(resultado.rows[0]);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao atualizar produto"
        });

    }

});
```

Requisição:

```http
PUT /produtos/1
```

Body:

```json
{
    "nome": "Espada grande",
    "complemento": "Espada de duas mãos"
}
```

---

# 18. DELETE

```js
app.delete("/produtos/:id", async (req, res) => {

    try {

        const { id } = req.params;

        const resultado = await pool.query(
            `
            DELETE FROM produtos
            WHERE id = $1
            RETURNING *
            `,
            [id]
        );

        if (resultado.rows.length === 0) {

            return res.status(404).json({
                erro: "Produto não encontrado"
            });

        }

        res.json(resultado.rows[0]);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao excluir produto"
        });

    }

});
```

---

# 19. Status HTTP

Alguns códigos importantes:

| Código | Significado |
|---:|---|
| `200` | OK |
| `201` | Criado |
| `204` | Sucesso sem conteúdo |
| `400` | Requisição inválida |
| `401` | Não autenticado |
| `403` | Sem permissão |
| `404` | Não encontrado |
| `409` | Conflito |
| `500` | Erro interno |

Exemplo:

```js
res.status(201).json(produto);
```

---

# 20. Validação básica

Não é uma boa ideia simplesmente confiar no `req.body`.

Exemplo:

```js
const { nome, complemento } = req.body;

if (!nome || !complemento) {

    return res.status(400).json({
        erro: "Nome e complemento são obrigatórios"
    });

}
```

Depois disso:

```js
const resultado = await pool.query(...);
```

A validação da API e as restrições do banco trabalham juntas.

Por exemplo:

```sql
nome VARCHAR(100) NOT NULL
```

e:

```js
if (!nome) {
    return res.status(400).json(...);
}
```

---

# 21. Query com múltiplos filtros

Exemplo:

```js
app.get("/produtos", async (req, res) => {

    const { nome } = req.query;

    const resultado = await pool.query(
        `
        SELECT *
        FROM produtos
        WHERE nome ILIKE $1
        `,
        [`%${nome}%`]
    );

    res.json(resultado.rows);

});
```

URL:

```text
GET /produtos?nome=espada
```

`req.query` contém:

```js
{
    nome: "espada"
}
```

---

# 22. `req.body`, `req.params` e `req.query`

São três coisas utilizadas constantemente:

## `req.body`

Dados enviados no corpo:

```json
{
    "nome": "Espada"
}
```

Acessar:

```js
req.body.nome
```

---

## `req.params`

Dados presentes na URL:

```text
/produtos/10
```

Rota:

```js
/produtos/:id
```

Acessar:

```js
req.params.id
```

---

## `req.query`

Parâmetros após `?`:

```text
/produtos?nome=espada
```

Acessar:

```js
req.query.nome
```

---

# 23. CRUD completo

Para uma tabela `produtos`:

```text
                    PostgreSQL
                         ▲
                         │
                     SQL Query
                         │
                         ▼
Cliente → Express → Controller
  │                       │
  └──── HTTP Response ◄───┘
```

Endpoints:

| Método | Endpoint | SQL |
|---|---|---|
| `POST` | `/produtos` | `INSERT` |
| `GET` | `/produtos` | `SELECT` |
| `GET` | `/produtos/:id` | `SELECT ... WHERE` |
| `PUT` | `/produtos/:id` | `UPDATE` |
| `DELETE` | `/produtos/:id` | `DELETE` |

---

# 24. Checklist para criar um CRUD

```text
[ ] Criar projeto Node
[ ] npm init -y

[ ] Instalar Express
[ ] npm install express

[ ] Instalar pg
[ ] npm install pg

[ ] Criar servidor Express
[ ] app.use(express.json())

[ ] Criar conexão Pool
[ ] Conectar ao PostgreSQL

[ ] Criar tabela

[ ] Criar POST
[ ] Criar GET
[ ] Criar GET por ID
[ ] Criar PUT
[ ] Criar DELETE

[ ] Usar queries parametrizadas
[ ] Validar req.body
[ ] Verificar registros inexistentes
[ ] Usar status HTTP apropriados

```

---

# 25. O que memorizar primeiro

Para começar a desenvolver APIs com Express + PostgreSQL, concentre-se nestes conceitos:

## Express

```js
express()
app.use()
app.get()
app.post()
app.put()
app.delete()
req.body
req.params
req.query
res.json()
res.status()
```

## PostgreSQL

```sql
CREATE TABLE
INSERT
SELECT
UPDATE
DELETE
WHERE
JOIN
PRIMARY KEY
FOREIGN KEY
RETURNING
```

## `pg`

```js
Pool
pool.query()
resultado.rows
$1
$2
$3
```

---

# 26. Autenticação e JWT

Até aqui, a API permite acessar as rotas sem saber quem está fazendo a requisição.

Para controlar isso, podemos implementar **autenticação**.

A ideia será:

```text
Cliente
   ↓
POST /login
   ↓
Express
   ↓
PostgreSQL
   ↓
usuário e senha corretos
   ↓
JWT
   ↓
Cliente recebe o token
   ↓
envia o token nas próximas requisições
   ↓
Middleware verifica o token
   ↓
rota protegida
```

---

# 27. O que é autenticação?

**Autenticação** é verificar a identidade de alguém.

Por exemplo:

```text
usuário: joao
senha: 123
```

A API verifica no banco:

```text
João existe?
A senha está correta?
```

Se estiver:

```text
Usuário autenticado
```

Já **autorização** é verificar se o usuário autenticado possui permissão para executar determinada ação.

Exemplo:

```text
Autenticação:
"Quem é você?"

Autorização:
"Você pode fazer isso?"
```

---

# 28. O problema depois do login

Imagine:

```http
POST /login
```

Body:

```json
{
    "usuario": "joao",
    "senha": "123"
}
```

A API verifica os dados e confirma que o login está correto.

Mas depois o cliente faz:

```http
GET /produtos
```

Como a API sabe que essa requisição pertence ao João?

Uma solução é utilizar um **token**.

---

# 29. O que é um token?

Um token é uma credencial que pode ser enviada pelo cliente para provar que ele já passou pelo processo de autenticação.

Uma analogia:

```text
Entrada de um evento
       ↓
apresenta ingresso
       ↓
ingresso é validado
       ↓
recebe uma pulseira
       ↓
usa a pulseira para acessar áreas permitidas
```

Na API:

```text
Login
  ↓
usuário + senha
  ↓
credenciais verificadas
  ↓
recebe token
  ↓
envia token nas próximas requisições
```

---

# 30. O que é JWT?

JWT significa:

**JSON Web Token**

É um formato utilizado para transportar informações entre partes de uma aplicação, permitindo que essas informações sejam verificadas por meio de uma assinatura.

Um JWT possui três partes:

```text
HEADER.PAYLOAD.SIGNATURE
```

Exemplo:

```text
eyJhbGciOiJIUzI1NiIs...
.
eyJpZCI6MTAsIm5vbWUiOiJKb2FvIn0...
.
assinatura...
```

As três partes são:

```text
HEADER
   ↓
informações sobre o token

PAYLOAD
   ↓
dados/claims

SIGNATURE
   ↓
assinatura utilizada para verificar o token
```

---

# 31. Header

O header contém informações sobre o token.

Exemplo:

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

`alg` indica o algoritmo utilizado para a assinatura.

`typ` indica o tipo do token.

Normalmente a biblioteca JWT cria essa parte automaticamente.

---

# 32. Payload

O payload contém os dados que queremos transportar.

Exemplo:

```json
{
    "id": 10,
    "nome": "Joao"
}
```

Também podemos ter informações relacionadas ao próprio token:

```json
{
    "id": 10,
    "nome": "Joao",
    "iat": 1758180000,
    "exp": 1758183600
}
```

`iat` representa o momento em que o token foi emitido.

`exp` representa o momento em que o token expira.

---

# 33. Não coloque informações secretas no payload

O payload de um JWT não deve ser tratado como um local para armazenar informações secretas.

Não coloque, por exemplo:

```json
{
    "senha": "123456"
}
```

A assinatura serve principalmente para permitir verificar a integridade e autenticidade do token.

JWT não deve ser confundido com criptografia de dados.

---

# 34. Signature

A assinatura é criada a partir do conteúdo do token e de uma chave.

De forma simplificada:

```text
HEADER + PAYLOAD + CHAVE SECRETA
                 ↓
             assinatura
```

Se alguém modificar o payload:

```json
{
    "id": 10,
    "nome": "Joao",
    "admin": true
}
```

a assinatura original não corresponderá mais ao conteúdo alterado.

A API pode detectar essa alteração ao verificar o token.

---

# 35. Chave secreta

A API precisa possuir uma chave usada para assinar e verificar os tokens.

Uma variável de ambiente pode ser utilizada:

```env
JWT_SECRET=minha-chave-secreta
```

No código:

```js
process.env.JWT_SECRET
```

A chave deve ser protegida e não deve ser publicada no código-fonte ou no repositório.

---

# 36. Instalando JWT

Para Node.js, podemos utilizar a biblioteca `jsonwebtoken`.

Instalar:

```bash
npm install jsonwebtoken
```

Importar:

```js
const jwt = require("jsonwebtoken");
```

As duas operações mais importantes inicialmente são:

```js
jwt.sign()
jwt.verify()
```

`sign()` cria um token.

`verify()` verifica um token.

---

# 37. Criando um JWT

Exemplo:

```js
const token = jwt.sign(
    {
        id: 10,
        nome: "Joao"
    },
    process.env.JWT_SECRET,
    {
        expiresIn: "1h"
    }
);
```

Os argumentos são:

```text
jwt.sign(
    payload,
    chave secreta,
    opções
)
```

Neste exemplo:

```text
payload
    ↓
id e nome do usuário

chave secreta
    ↓
process.env.JWT_SECRET

expiresIn
    ↓
token expira em 1 hora
```

---

# 38. JWT no login

O JWT normalmente é criado depois que o usuário e a senha foram verificados.

Exemplo:

```js
app.post("/login", async (req, res) => {

    const { usuario, senha } = req.body;

    const resultado = await pool.query(
        "SELECT * FROM usuario WHERE nome = $1",
        [usuario]
    );

    if (resultado.rows.length === 0) {

        return res.status(401).json({
            erro: "Usuário ou senha inválidos"
        });

    }

    const user = resultado.rows[0];

    // Em uma aplicação real,
    // a senha deve ser armazenada como hash.
    if (senha !== user.senha) {

        return res.status(401).json({
            erro: "Usuário ou senha inválidos"
        });

    }

    const token = jwt.sign(
        {
            id: user.id,
            nome: user.nome
        },
        process.env.JWT_SECRET,
        {
            expiresIn: "1h"
        }
    );

    res.json({
        token: token
    });

});
```

O fluxo é:

```text
POST /login
     ↓
recebe usuário e senha
     ↓
consulta PostgreSQL
     ↓
verifica credenciais
     ↓
jwt.sign()
     ↓
retorna token
```

---

# 39. Resposta do login

Se o login funcionar:

```json
{
    "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

O cliente pode guardar esse token e enviá-lo nas próximas requisições.

---

# 40. Authorization Header

O token normalmente é enviado no header:

```http
Authorization: Bearer TOKEN
```

Exemplo:

```http
GET /produtos
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

`Bearer` indica que o valor seguinte é o token utilizado para autenticação.

---

# 41. Middleware de autenticação

Como várias rotas podem precisar de autenticação, é útil criar um middleware.

Exemplo:

```text
middleware/
└── autenticar.js
```

Código:

```js
const jwt = require("jsonwebtoken");

function autenticar(req, res, next) {

    const authHeader = req.headers.authorization;

    if (!authHeader) {

        return res.status(401).json({
            erro: "Token não fornecido"
        });

    }

    const partes = authHeader.split(" ");

    const tipo = partes[0];
    const token = partes[1];

    if (tipo !== "Bearer" || !token) {

        return res.status(401).json({
            erro: "Formato de token inválido"
        });

    }

    try {

        const usuario = jwt.verify(
            token,
            process.env.JWT_SECRET
        );

        req.usuario = usuario;

        next();

    } catch (erro) {

        return res.status(401).json({
            erro: "Token inválido ou expirado"
        });

    }

}

module.exports = autenticar;
```

---

# 42. Como o middleware funciona?

A requisição:

```text
GET /produtos
```

passa pelo middleware:

```text
GET /produtos
      ↓
autenticar
      ↓
existe token?
      ↓
jwt.verify()
      ↓
token válido?
   ↓       ↓
 não      sim
   ↓        ↓
 401      next()
            ↓
       rota /produtos
```

---

# 43. `jwt.verify()`

A parte principal da verificação é:

```js
const usuario = jwt.verify(
    token,
    process.env.JWT_SECRET
);
```

A biblioteca verifica se o token é válido utilizando a chave secreta.

Se o token estiver inválido ou expirado, ocorre um erro.

Por isso utilizamos:

```js
try {

    // verifica JWT

} catch (erro) {

    // token inválido
}
```

---

# 44. `req.usuario`

Depois de verificar o token:

```js
const usuario = jwt.verify(
    token,
    process.env.JWT_SECRET
);
```

podemos colocar os dados do usuário dentro da requisição:

```js
req.usuario = usuario;
```

Assim, as próximas etapas da requisição podem acessar:

```js
req.usuario.id
```

ou:

```js
req.usuario.nome
```

Por exemplo:

```js
console.log(req.usuario);
```

Pode produzir:

```json
{
    "id": 10,
    "nome": "Joao",
    "iat": 1758180000,
    "exp": 1758183600
}
```

---

# 45. `next()`

No Express, um middleware pode decidir se a requisição deve continuar.

Quando o token é válido:

```js
next();
```

permite que a execução continue.

Quando o token é inválido:

```js
return res.status(401).json({
    erro: "Token inválido"
});
```

a requisição termina ali.

---

# 46. Protegendo uma rota

Importar:

```js
const autenticar = require("./middleware/autenticar");
```

Depois:

```js
app.get(
    "/produtos",
    autenticar,
    async (req, res) => {

        const resultado = await pool.query(
            "SELECT * FROM produtos"
        );

        res.json(resultado.rows);

    }
);
```

Agora:

```text
GET /produtos
       ↓
autenticar
       ↓
JWT válido
       ↓
rota /produtos
```

Sem token:

```text
GET /produtos
       ↓
autenticar
       ↓
401 Unauthorized
```

---

# 47. Protegendo várias rotas

Podemos colocar o middleware diretamente nas rotas:

```js
app.get("/produtos", autenticar, ...);

app.post("/produtos", autenticar, ...);

app.put("/produtos/:id", autenticar, ...);

app.delete("/produtos/:id", autenticar, ...);
```

Também podemos aplicar o middleware a um conjunto de rotas:

```js
app.use("/produtos", autenticar);
```

Nesse caso, as rotas que começam com `/produtos` passam pelo middleware.

---

# 48. Utilizando o usuário autenticado

Uma das maiores utilidades do JWT é conseguir identificar o usuário que fez a requisição.

Por exemplo:

```js
app.post(
    "/registros",
    autenticar,
    async (req, res) => {

        const usuarioId = req.usuario.id;

        // restante da operação

    }
);
```

O cliente pode enviar:

```json
{
    "produto_id": 5,
    "tipo": "entrada",
    "quantidade": 20
}
```

Sem precisar enviar:

```json
{
    "usuario_id": 10
}
```

A API obtém o usuário a partir do JWT:

```js
const usuarioId = req.usuario.id;
```

---

# 49. JWT + PostgreSQL

Considere uma tabela:

```sql
CREATE TABLE registro (
    id SERIAL PRIMARY KEY,
    usuario_id INT NOT NULL,
    produto_id INT NOT NULL,
    data TIMESTAMP NOT NULL,
    tipo VARCHAR NOT NULL,
    quantidade INT NOT NULL
);
```

Uma rota protegida poderia utilizar:

```js
app.post(
    "/registros",
    autenticar,
    async (req, res) => {

        const usuarioId = req.usuario.id;

        const {
            produto_id,
            tipo,
            quantidade
        } = req.body;

        const resultado = await pool.query(
            `
            INSERT INTO registro
                (usuario_id, produto_id, data, tipo, quantidade)
            VALUES
                ($1, $2, NOW(), $3, $4)
            RETURNING *
            `,
            [
                usuarioId,
                produto_id,
                tipo,
                quantidade
            ]
        );

        res.status(201).json(
            resultado.rows[0]
        );

    }
);
```

Fluxo:

```text
Login
  ↓
JWT
  ↓
Authorization: Bearer JWT
  ↓
autenticar
  ↓
req.usuario.id
  ↓
usuario_id
  ↓
INSERT
```

---

# 50. Testando JWT no Thunder Client

## 1. Fazer login

```http
POST /login
```

Body:

```json
{
    "usuario": "joao",
    "senha": "123"
}
```

Resposta:

```json
{
    "token": "eyJhbGciOi..."
}
```

---

## 2. Copiar o token

Copie:

```text
eyJhbGciOi...
```

---

## 3. Acessar uma rota protegida

```http
GET /produtos
```

Header:

```text
Authorization: Bearer eyJhbGciOi...
```

Se estiver correto:

```text
200 OK
```

Sem token:

```text
401 Unauthorized
```

Com token inválido ou expirado:

```text
401 Unauthorized
```

---

# 51. JWT e sessão tradicional

Uma forma tradicional de autenticação pode manter uma sessão no servidor.

Com JWT, a ideia pode ser:

```text
Cliente
   ↓
JWT
   ↓
Servidor verifica assinatura
```

O servidor não precisa necessariamente manter uma sessão individual para cada usuário.

Isso é conhecido como uma abordagem **stateless**.

O JWT ainda pode ser utilizado junto com outros mecanismos de controle de sessão, mas a ideia básica é que a própria credencial contém as informações necessárias para sua validação.

---

# 52. JWT não substitui autorização

Ter um JWT válido significa que o usuário foi autenticado.

Isso não significa automaticamente que ele pode fazer qualquer coisa.

Por exemplo:

```json
{
    "id": 10,
    "nome": "Joao",
    "tipo": "usuario"
}
```

e:

```json
{
    "id": 20,
    "nome": "Maria",
    "tipo": "admin"
}
```

A API poderia verificar permissões antes de determinadas operações.

Por exemplo:

```js
if (req.usuario.tipo !== "admin") {

    return res.status(403).json({
        erro: "Sem permissão"
    });

}
```

Nesse caso:

```text
401 → não autenticado

403 → autenticado, mas sem permissão
```

---

# 53. `401` vs `403`

| Código | Significado |
|---:|---|
| `401` | Usuário não autenticado / credencial inválida |
| `403` | Usuário autenticado, mas sem permissão |

Exemplo:

```text
Sem JWT
   ↓
401
```

```text
JWT válido + sem permissão
   ↓
403
```

---

# 54. Estrutura recomendada

Conforme a API cresce, podemos separar o middleware das rotas:

```text
minha-api/
│
├── node_modules/
├── package.json
├── .env
│
└── src/
    ├── server.js
    │
    ├── middleware/
    │   └── autenticar.js
    │
    ├── routes/
    │   ├── login.js
    │   ├── produtos.js
    │   └── registros.js
    │
    └── controllers/
        ├── loginController.js
        ├── produtoController.js
        └── registroController.js
```

O middleware fica responsável pela autenticação.

As rotas ficam responsáveis por receber as requisições.

Os controllers podem concentrar a lógica das operações.

---

# 55. Fluxo completo da aplicação

```text
                  POST /login
                       │
                       ▼
                usuário + senha
                       │
                       ▼
                   Express
                       │
                       ▼
                 PostgreSQL
                       │
                       ▼
              credenciais válidas?
                 /           \
               NÃO           SIM
                │             │
               401            ▼
                         jwt.sign()
                              │
                              ▼
                             JWT
                              │
                              ▼
                           Cliente
                              │
                 Authorization: Bearer JWT
                              │
                              ▼
                      Rota protegida
                              │
                              ▼
                         Middleware
                              │
                        jwt.verify()
                         /        \
                     inválido      válido
                        │            │
                       401           ▼
                              req.usuario
                                   │
                                   ▼
                                  Rota
                                   │
                                   ▼
                              PostgreSQL
```

---

# 56. Checklist de JWT

```text
[ ] Instalar jsonwebtoken

[ ] Criar JWT_SECRET

[ ] Configurar variável de ambiente

[ ] Criar rota POST /login

[ ] Verificar usuário no PostgreSQL

[ ] Verificar senha

[ ] Criar JWT com jwt.sign()

[ ] Retornar token ao cliente

[ ] Criar middleware de autenticação

[ ] Ler Authorization

[ ] Extrair Bearer token

[ ] Usar jwt.verify()

[ ] Colocar usuário em req.usuario

[ ] Usar next()

[ ] Proteger rotas

[ ] Utilizar req.usuario.id quando necessário

[ ] Implementar autorização quando necessário
```

---

# 57. O que memorizar primeiro

Não é necessário decorar toda a implementação de uma vez.

Comece entendendo:

```text
LOGIN
  ↓
JWT
  ↓
Authorization: Bearer TOKEN
  ↓
middleware
  ↓
jwt.verify()
  ↓
req.usuario
  ↓
rota protegida
```

E principalmente:

```js
jwt.sign()
```

→ cria o token.

```js
jwt.verify()
```

→ verifica o token.

```js
req.usuario
```

→ permite que as rotas utilizem as informações do usuário autenticado.

---

# 58. Conceitos para estudar depois

Depois de entender o fluxo básico, os próximos assuntos importantes são:

```text
JWT
 ↓
Hash de senha
 ↓
bcrypt / Argon2
 ↓
Autorização
 ↓
Roles / permissões
 ↓
Access token
 ↓
Refresh token
 ↓
Expiração
 ↓
Logout / revogação
 ↓
Cookies ou armazenamento do token
 ↓
HTTPS
```

O objetivo inicial não é decorar todos esses conceitos, mas entender claramente o ciclo:

```text
"Eu faço login"
       ↓
"Recebo uma credencial"
       ↓
"Envio essa credencial"
       ↓
"A API verifica"
       ↓
"A API descobre quem sou"
       ↓
"A API decide se posso executar a operação"
```


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
        color: #a0d1a4;

    }

</style>

