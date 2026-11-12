# Requisições HTTP
> As rotas que os dados tomarão

*Antes de acessar essa parte, configure seu servidor usando Express e Pg, seguindo os passos descritos nas duas primeiras sessões de [Frameworks relevantes](frameworks.md)* 

Uma requisição http segue a seguinte estrutura:

```js
app.metodo('caminho', async (request, response) =>{

    const resultado = await pool.query(
        `
            QUERY SQL
        `
    )

    response.json(resultado.rows)

})
```

onde:

### Metodo

- Indica para a API que tipo de manipulação de dados será feita.
- Os principais metodos usados são:
    - **GET**
    - **POST**
    - **PUT**
    - **DELETE**

Cada metodo possui um proposito proprio

#### GET - app.get()

- Retorna dados estaticos através de queries
- Não consegue acessar request body, podendo apenas receber parametros em seu caminho

#### POST - app.post()

- Permite o envio de dados para o banco
- Possui acesso ao request body, permitindo envio de blocos  de dados Json

#### PUT - app.put()

- Permite a atualização de uma tabela já preenchida.
- Possui acesso ao request body.

#### DELETE - app.delete()

- Permite a exclusão de uma tabela com base em seu id.
- Não possui acesso ao request.body, podendo apenas receber parametros em seu caminho.

---

### Caminho

- Indica o caminho que o navegador deve utilizar para acessar a requisição no servidor
- Sempre começar com /
- Pode ou não possuir parametros

#### Caminho sem parametros

**EX** 

`app.get('/')`

`app.get('/usuario')`

#### Caminho com parametros

**EX**

`app.get('/usuario/:id')`

### Request & Response

- Parametros internos dos requerimentos para auxiliar em seu uso
- Request representa dados **enviados para a API**, enquanto Response representa o **resultado do requerimento**
- Geralmente, são incurtados para *req* e *res*, respectivamente

#### REQUEST (req)

- Geralmente utilizado nos contextos *request body* e *request params*

**EX**

``` js
app.put('/usuario/:id', async (req, res) =>{

    const { id } = req.params;
    const { nome, senha, cpf } = req.body;

})
```

#### RESPONSE (res)

- Utilizado para retornar o resultado de um requerimento

**EX**

``` js
app.get('/usuario', async (req, res) =>{
    
    const resultado = await pool.query
    (
        //query ai muito doida
    )

    res.json({
        mensagem:'SUCESSO: Busca realizada',
        dados: resultado.rows
    })

})
```

### Resultado

- Variavel onde a query será realizada
- Antes de ser retornada ao usuario, os dados adquiridos devem ser tornados em um array através de *resultado.rows*
- **IMPORTANTE:** Para que a query possa ser efetuada, coloque *async* logo antes dos parametros da requisição e *await* logo antes da query ser efetuada. Sem isso, a query não irá funcionar devidamente

*EX*

``` js
app.post('/usuario', async (req, res) =>{

    const resultado = await pool.query
    (
        //query ai muito doida
    )

    res.json({
        mensagem:'SUCESSO: Busca realizada',
        dados: resultado.rows[0] // O 0 especifica que é apenas para puxar o ultimo elemento adicionado
    })

})
```

#### QUERIES SQL

> Para uma visão mais aprofundada de como realizar queries, acesse [Resumo de PostgreSQL](../PostgreSQL/resumo.md)

- Após criar a variavel de resultado e inserir `await pool.query()`, abra duas crases dentro dos parenteses. É dentro dessas crases que a query propriamente dita sera escrita
- **IMPORTANTE:** As crases são essenciais para permitir a inserção de variaveis dentro da query, e por isso não podem ser substituidas por *' '* ou *" "*
- A query segue o padrão de queries comuns de PostgreSQL, com o diferencial da inserção de variaveis que representam os dados colhetados pelo *req*.
- Os dados das variaveis são sifrões seguidos por um numero *($1, $2, $3, etc)*. Fora das crases, crie uma estrutura de array *[variavel, variavel, variavel]*

**EX**

``` js
app.put('/usuario/:id', async (req, res) =>{

    const { id } = req.params;
    const { nome, senha } = req.body;

    const resultado = await pool.query(
        `
            UPDATE usuario
            SET
                nome = $1,
                senha = $2
            WHERE id = $3
            RETURNING *
        `,
        [nome, senha, id]
    )

    res.json({
        mensagem:'SUCESSO: Usuario atualizado',
        dados: resultado.rows[0] // O 0 especifica que é apenas para puxar o ultimo elemento adicionado
    })

})
```




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

    h4{

        color: #5EFF6B;
        background-color: #304231;
        padding: 5px;
        border-radius: 5px

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