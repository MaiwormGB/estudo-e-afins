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
