# Resumo de PostgreSQL

Guia rápido de PostgreSQL para desenvolvimento web.

O foco deste documento é o uso de comandos SQL em aplicações, principalmente
para integração posterior com Node.js.

---

# 1. Conceitos básicos

## Banco de dados

Um banco de dados contém tabelas e outros objetos relacionados aos dados.

```sql
CREATE DATABASE minha_api;
```

Para apagar:

```sql
DROP DATABASE minha_api;
```

> Cuidado: `DROP DATABASE` apaga o banco e seus dados.

---

# 2. Tabelas

## Criar tabela

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL,
    senha VARCHAR(100) NOT NULL
);
```

Estrutura:

```sql
CREATE TABLE nome_tabela (
    coluna tipo restrições,
    coluna tipo restrições
);
```

---

# 3. Tipos de dados mais usados

| Tipo | Uso |
|---|---|
| `INTEGER` | Números inteiros |
| `BIGINT` | Inteiros maiores |
| `SERIAL` | Inteiro com incremento automático |
| `VARCHAR(n)` | Texto com limite |
| `TEXT` | Texto sem limite definido |
| `BOOLEAN` | `TRUE` / `FALSE` |
| `DATE` | Data |
| `TIMESTAMP` | Data e hora |
| `DECIMAL` | Valores numéricos precisos |
| `JSONB` | Dados JSON |

Exemplo:

```sql
CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10, 2),
    disponivel BOOLEAN DEFAULT TRUE,
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 4. Restrições

## PRIMARY KEY

Identifica unicamente cada registro.

```sql
id SERIAL PRIMARY KEY
```

Uma tabela só pode ter uma `PRIMARY KEY`.

---

## NOT NULL

Impede que uma coluna receba `NULL`.

```sql
nome VARCHAR(100) NOT NULL
```

---

## UNIQUE

Impede valores repetidos.

```sql
email VARCHAR(150) UNIQUE
```

Exemplo:

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    email VARCHAR(150) UNIQUE NOT NULL
);
```

---

## DEFAULT

Define um valor padrão.

```sql
ativo BOOLEAN DEFAULT TRUE
```

---

## CHECK

Cria uma condição que o valor precisa respeitar.

```sql
idade INTEGER CHECK (idade >= 0)
```

---

# 5. Chaves estrangeiras

Uma `FOREIGN KEY` relaciona uma tabela com outra.

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL,

    FOREIGN KEY (usuario_id)
        REFERENCES usuarios(id)
);
```

Nesse exemplo:

```text
usuarios
   |
   | id
   |
   v
pedidos.usuario_id
```

`pedidos.usuario_id` precisa apontar para um `usuarios.id` existente.

---

## FOREIGN KEY com nome

Também é possível nomear a restrição:

```sql
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL,

    CONSTRAINT fk_pedido_usuario
        FOREIGN KEY (usuario_id)
        REFERENCES usuarios(id)
);
```

---

# 6. INSERT

Adiciona registros.

```sql
INSERT INTO usuarios (nome, email, senha)
VALUES ('João', 'joao@email.com', '1234');
```

Vários registros:

```sql
INSERT INTO usuarios (nome, email, senha)
VALUES
    ('João', 'joao@email.com', '1234'),
    ('Maria', 'maria@email.com', '5678'),
    ('Pedro', 'pedro@email.com', 'abcd');
```

---

# 7. SELECT

Consulta registros.

```sql
SELECT * FROM usuarios;
```

Selecionando colunas específicas:

```sql
SELECT id, nome, email
FROM usuarios;
```

---

# 8. WHERE

Filtra registros.

```sql
SELECT *
FROM usuarios
WHERE id = 1;
```

Mais condições:

```sql
SELECT *
FROM usuarios
WHERE nome = 'João'
AND id = 1;
```

Ou:

```sql
SELECT *
FROM usuarios
WHERE nome = 'João'
OR nome = 'Maria';
```

---

# 9. Operadores

## Comparação

```sql
=       -- igual
<>      -- diferente
!=      -- diferente
>       -- maior
<       -- menor
>=      -- maior ou igual
<=      -- menor ou igual
```

Exemplo:

```sql
SELECT *
FROM produtos
WHERE preco > 100;
```

---

# 10. IN

Verifica se o valor está dentro de uma lista.

```sql
SELECT *
FROM usuarios
WHERE id IN (1, 2, 3);
```

---

# 11. BETWEEN

Seleciona valores dentro de um intervalo.

```sql
SELECT *
FROM produtos
WHERE preco BETWEEN 50 AND 100;
```

---

# 12. LIKE

Pesquisa texto.

```sql
SELECT *
FROM usuarios
WHERE nome LIKE 'Jo%';
```

`%` representa qualquer quantidade de caracteres.

Exemplos:

```text
'Jo%'      -- começa com Jo
'%Jo'      -- termina com Jo
'%Jo%'     -- contém Jo
```

---

# 13. ILIKE

No PostgreSQL, `ILIKE` funciona como `LIKE`, mas ignora diferença
entre letras maiúsculas e minúsculas.

```sql
SELECT *
FROM usuarios
WHERE nome ILIKE '%joão%';
```

---

# 14. NULL

`NULL` representa ausência de valor.

Não use:

```sql
WHERE email = NULL
```

Use:

```sql
WHERE email IS NULL
```

Ou:

```sql
WHERE email IS NOT NULL
```

---

# 15. ORDER BY

Ordena os resultados.

```sql
SELECT *
FROM produtos
ORDER BY preco;
```

Ordem decrescente:

```sql
SELECT *
FROM produtos
ORDER BY preco DESC;
```

Ordem crescente:

```sql
SELECT *
FROM produtos
ORDER BY preco ASC;
```

---

# 16. LIMIT

Limita a quantidade de resultados.

```sql
SELECT *
FROM produtos
LIMIT 10;
```

---

# 17. OFFSET

Pula determinada quantidade de registros.

```sql
SELECT *
FROM produtos
LIMIT 10
OFFSET 20;
```

Útil para paginação.

Exemplo:

```text
Página 1:
LIMIT 10 OFFSET 0

Página 2:
LIMIT 10 OFFSET 10

Página 3:
LIMIT 10 OFFSET 20
```

---

# 18. UPDATE

Atualiza registros.

```sql
UPDATE usuarios
SET nome = 'Carlos'
WHERE id = 1;
```

Mais de uma coluna:

```sql
UPDATE usuarios
SET
    nome = 'Carlos',
    email = 'carlos@email.com'
WHERE id = 1;
```

> Sempre tome cuidado com o `WHERE`.
>
> Sem `WHERE`, todos os registros serão alterados.

---

# 19. DELETE

Remove registros.

```sql
DELETE FROM usuarios
WHERE id = 1;
```

> Sem `WHERE`, todos os registros serão removidos.

---

# 20. RETURNING

O PostgreSQL permite retornar o registro afetado por
`INSERT`, `UPDATE` e `DELETE`.

Isso é especialmente útil em APIs.

## INSERT

```sql
INSERT INTO usuarios (nome, email)
VALUES ('João', 'joao@email.com')
RETURNING *;
```

Ou apenas o ID:

```sql
INSERT INTO usuarios (nome, email)
VALUES ('João', 'joao@email.com')
RETURNING id;
```

## UPDATE

```sql
UPDATE usuarios
SET nome = 'Carlos'
WHERE id = 1
RETURNING *;
```

## DELETE

```sql
DELETE FROM usuarios
WHERE id = 1
RETURNING *;
```

---

# 21. JOIN

Usado para consultar dados relacionados entre tabelas.

Exemplo:

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100)
);

CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER REFERENCES usuarios(id),
    produto VARCHAR(100)
);
```

## INNER JOIN

Retorna registros que possuem correspondência nas duas tabelas.

```sql
SELECT
    usuarios.nome,
    pedidos.produto
FROM usuarios
INNER JOIN pedidos
    ON pedidos.usuario_id = usuarios.id;
```

Resultado conceitual:

```text
nome     | produto
---------|--------
João     | Espada
Maria    | Poção
João     | Escudo
```

---

## LEFT JOIN

Retorna todos os registros da tabela da esquerda,
mesmo que não tenham correspondência.

```sql
SELECT
    usuarios.nome,
    pedidos.produto
FROM usuarios
LEFT JOIN pedidos
    ON pedidos.usuario_id = usuarios.id;
```

Isso permite encontrar, por exemplo, usuários que ainda
não possuem pedidos.

---

# 22. Alias

Podemos dar apelidos para tabelas e colunas.

```sql
SELECT
    u.nome,
    p.produto
FROM usuarios AS u
JOIN pedidos AS p
    ON p.usuario_id = u.id;
```

O `AS` pode ser omitido:

```sql
FROM usuarios u
JOIN pedidos p
```

---

# 23. Funções de agregação

## COUNT

Conta registros.

```sql
SELECT COUNT(*)
FROM usuarios;
```

---

## SUM

Soma valores.

```sql
SELECT SUM(preco)
FROM produtos;
```

---

## AVG

Calcula média.

```sql
SELECT AVG(preco)
FROM produtos;
```

---

## MAX

Maior valor.

```sql
SELECT MAX(preco)
FROM produtos;
```

---

## MIN

Menor valor.

```sql
SELECT MIN(preco)
FROM produtos;
```

---

# 24. GROUP BY

Agrupa registros.

Exemplo:

```sql
SELECT usuario_id, COUNT(*)
FROM pedidos
GROUP BY usuario_id;
```

Resultado conceitual:

```text
usuario_id | count
-----------|------
1          | 5
2          | 3
3          | 8
```

---

# 25. HAVING

Filtra grupos criados pelo `GROUP BY`.

```sql
SELECT usuario_id, COUNT(*)
FROM pedidos
GROUP BY usuario_id
HAVING COUNT(*) > 5;
```

Diferença:

```text
WHERE  -> filtra registros antes do agrupamento

HAVING -> filtra grupos depois do agrupamento
```

---

# 26. DISTINCT

Remove valores duplicados.

```sql
SELECT DISTINCT usuario_id
FROM pedidos;
```

---

# 27. Alterar tabela

## Adicionar coluna

```sql
ALTER TABLE usuarios
ADD COLUMN idade INTEGER;
```

---

## Remover coluna

```sql
ALTER TABLE usuarios
DROP COLUMN idade;
```

---

## Alterar tipo

```sql
ALTER TABLE usuarios
ALTER COLUMN nome TYPE TEXT;
```

---

## Adicionar restrição

```sql
ALTER TABLE usuarios
ADD CONSTRAINT email_unico UNIQUE (email);
```

---

# 28. Renomear

## Coluna

```sql
ALTER TABLE usuarios
RENAME COLUMN nome TO nome_completo;
```

## Tabela

```sql
ALTER TABLE usuarios
RENAME TO clientes;
```

---

# 29. Remover tabela

```sql
DROP TABLE usuarios;
```

Se existirem dependências:

```sql
DROP TABLE usuarios CASCADE;
```

> `CASCADE` pode remover objetos dependentes.
> Use com cuidado.

---

# 30. TRUNCATE

Remove todos os registros de uma tabela.

```sql
TRUNCATE TABLE usuarios;
```

Diferente de:

```sql
DELETE FROM usuarios;
```

`TRUNCATE` é destinado a limpar a tabela inteira e não permite
um `WHERE`.

---

# 31. JSONB

O PostgreSQL possui suporte para JSON através do tipo `JSONB`.

Exemplo:

```sql
CREATE TABLE itens (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    dados JSONB
);
```

Inserindo JSON:

```sql
INSERT INTO itens (nome, dados)
VALUES (
    'Espada',
    '{"dano": "2d6", "alcance": 1}'
);
```

Consultando:

```sql
SELECT dados
FROM itens;
```

Acessando uma propriedade:

```sql
SELECT dados->'dano'
FROM itens;
```

Como texto:

```sql
SELECT dados->>'dano'
FROM itens;
```

Filtrando:

```sql
SELECT *
FROM itens
WHERE dados->>'dano' = '2d6';
```

---

# 32. Arrays

PostgreSQL também possui arrays.

```sql
CREATE TABLE personagens (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    talentos TEXT[]
);
```

Inserindo:

```sql
INSERT INTO personagens (nome, talentos)
VALUES (
    'Aragorn',
    ARRAY['Espadachim', 'Liderança']
);
```

Ou:

```sql
INSERT INTO personagens (nome, talentos)
VALUES (
    'Aragorn',
    '{"Espadachim", "Liderança"}'
);
```

---

# 33. Subquery

Uma query pode ser utilizada dentro de outra.

Exemplo:

```sql
SELECT *
FROM produtos
WHERE preco > (
    SELECT AVG(preco)
    FROM produtos
);
```

Nesse caso, são selecionados produtos com preço acima da média.

---

# 34. EXISTS

Verifica se uma subquery retorna algum resultado.

```sql
SELECT *
FROM usuarios u
WHERE EXISTS (
    SELECT 1
    FROM pedidos p
    WHERE p.usuario_id = u.id
);
```

Retorna usuários que possuem pelo menos um pedido.

---

# 35. CASE

Permite criar condições dentro do `SELECT`.

```sql
SELECT
    nome,
    preco,
    CASE
        WHEN preco < 50 THEN 'barato'
        WHEN preco < 100 THEN 'medio'
        ELSE 'caro'
    END AS categoria
FROM produtos;
```

---

# 36. Transações

Transações permitem executar várias operações como uma única unidade.

```sql
BEGIN;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 100
WHERE id = 2;

COMMIT;
```

Se algo der errado:

```sql
ROLLBACK;
```

Estrutura:

```text
BEGIN
  ↓
operações
  ↓
COMMIT
```

Ou:

```text
BEGIN
  ↓
operações
  ↓
ROLLBACK
```

---

# 37. Índices

Índices podem acelerar consultas em determinadas colunas.

```sql
CREATE INDEX idx_usuario_email
ON usuarios(email);
```

Remover:

```sql
DROP INDEX idx_usuario_email;
```

Índices são especialmente úteis para colunas frequentemente utilizadas
em buscas, filtros e relacionamentos.

---

# 38. Comandos úteis para inspeção

No PostgreSQL pelo `psql`:

```sql
\l
```

Lista bancos.

```sql
\c nome_banco
```

Conecta a um banco.

```sql
\dt
```

Lista tabelas.

```sql
\d nome_tabela
```

Mostra a estrutura de uma tabela.

```sql
\du
```

Lista usuários/roles.

```sql
\q
```

Sai do `psql`.

---

# 39. Exemplo de CRUD completo

Tabela:

```sql
CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    complemento VARCHAR(200) NOT NULL
);
```

## CREATE

```sql
INSERT INTO produtos (nome, complemento)
VALUES ('Espada', 'Espada longa')
RETURNING *;
```

## READ

```sql
SELECT *
FROM produtos;
```

Por ID:

```sql
SELECT *
FROM produtos
WHERE id = 1;
```

## UPDATE

```sql
UPDATE produtos
SET complemento = 'Espada longa de aço'
WHERE id = 1
RETURNING *;
```

## DELETE

```sql
DELETE FROM produtos
WHERE id = 1
RETURNING *;
```

---

# 40. CRUD com relacionamento

Exemplo:

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE registros (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL,
    produto_id INTEGER NOT NULL,
    data TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (usuario_id)
        REFERENCES usuarios(id),

    FOREIGN KEY (produto_id)
        REFERENCES produtos(id)
);
```

Inserindo um registro:

```sql
INSERT INTO registros (usuario_id, produto_id)
VALUES (1, 5)
RETURNING *;
```

Consultando com os nomes:

```sql
SELECT
    r.id,
    u.nome AS usuario,
    p.nome AS produto,
    r.data
FROM registros r
JOIN usuarios u
    ON r.usuario_id = u.id
JOIN produtos p
    ON r.produto_id = p.id;
```

---

# 41. SQL + Node.js

Para PostgreSQL em Node.js, uma biblioteca comum é o `pg`.

Instalação:

```bash
npm install pg
```

Conexão:

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

---

# 42. Executando uma query

```js
const resultado = await pool.query(
    "SELECT * FROM usuarios"
);

console.log(resultado.rows);
```

`resultado.rows` contém os registros retornados.

---

# 43. Query com parâmetros

Nunca monte uma query usando diretamente dados fornecidos
pelo usuário.

Evite:

```js
const nome = req.body.nome;

const resultado = await pool.query(
    `SELECT * FROM usuarios WHERE nome = '${nome}'`
);
```

Prefira parâmetros:

```js
const nome = req.body.nome;

const resultado = await pool.query(
    "SELECT * FROM usuarios WHERE nome = $1",
    [nome]
);
```

Parâmetros:

```text
$1 -> primeiro valor
$2 -> segundo valor
$3 -> terceiro valor
```

Exemplo:

```js
const resultado = await pool.query(
    `
    SELECT *
    FROM usuarios
    WHERE nome = $1
    AND email = $2
    `,
    [nome, email]
);
```

---

# 44. INSERT no Node.js

```js
const resultado = await pool.query(
    `
    INSERT INTO usuarios (nome, email)
    VALUES ($1, $2)
    RETURNING *
    `,
    [nome, email]
);

const usuario = resultado.rows[0];
```

---

# 45. UPDATE no Node.js

```js
const resultado = await pool.query(
    `
    UPDATE usuarios
    SET nome = $1
    WHERE id = $2
    RETURNING *
    `,
    [nome, id]
);
```

---

# 46. DELETE no Node.js

```js
const resultado = await pool.query(
    `
    DELETE FROM usuarios
    WHERE id = $1
    RETURNING *
    `,
    [id]
);
```

---

# 47. Tratamento de erro

```js
try {

    const resultado = await pool.query(
        "SELECT * FROM usuarios"
    );

    console.log(resultado.rows);

} catch (erro) {

    console.error(erro);

}
```

---

# 48. Padrão básico de uma API

Uma API normalmente recebe uma requisição,
executa uma query e devolve o resultado.

Exemplo com Express:

```js
app.get("/usuarios", async (req, res) => {

    try {

        const resultado = await pool.query(
            "SELECT * FROM usuarios"
        );

        res.json(resultado.rows);

    } catch (erro) {

        console.error(erro);

        res.status(500).json({
            erro: "Erro ao buscar usuários"
        });

    }

});
```

Fluxo:

```text
Cliente
   ↓
HTTP Request
   ↓
Node.js / Express
   ↓
SQL Query
   ↓
PostgreSQL
   ↓
resultado.rows
   ↓
HTTP Response
```

---

# 49. CRUD de uma API

Uma estrutura comum:

```text
POST   /usuarios       → INSERT
GET    /usuarios       → SELECT
GET    /usuarios/:id   → SELECT WHERE
PUT    /usuarios/:id   → UPDATE
DELETE /usuarios/:id   → DELETE
```

Exemplo:

```text
POST /produtos
```

```sql
INSERT INTO produtos (...)
VALUES (...);
```

---

```text
GET /produtos
```

```sql
SELECT * FROM produtos;
```

---

```text
GET /produtos/5
```

```sql
SELECT *
FROM produtos
WHERE id = 5;
```

---

```text
PUT /produtos/5
```

```sql
UPDATE produtos
SET ...
WHERE id = 5;
```

---

```text
DELETE /produtos/5
```

```sql
DELETE FROM produtos
WHERE id = 5;
```

---

# 50. Resumo dos comandos principais

| Comando | Função |
|---|---|
| `CREATE DATABASE` | Criar banco |
| `CREATE TABLE` | Criar tabela |
| `ALTER TABLE` | Alterar tabela |
| `DROP TABLE` | Remover tabela |
| `INSERT INTO` | Inserir dados |
| `SELECT` | Consultar dados |
| `UPDATE` | Alterar dados |
| `DELETE` | Remover dados |
| `TRUNCATE` | Limpar tabela |
| `WHERE` | Filtrar |
| `ORDER BY` | Ordenar |
| `LIMIT` | Limitar resultados |
| `OFFSET` | Pular resultados |
| `JOIN` | Relacionar tabelas |
| `GROUP BY` | Agrupar |
| `HAVING` | Filtrar grupos |
| `DISTINCT` | Remover duplicados |
| `RETURNING` | Retornar dados afetados |
| `BEGIN` | Iniciar transação |
| `COMMIT` | Confirmar transação |
| `ROLLBACK` | Desfazer transação |
| `CREATE INDEX` | Criar índice |



<style>

    h1{

        color: #695eff;

    }

    h2{

        color: #a29cf8;;

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