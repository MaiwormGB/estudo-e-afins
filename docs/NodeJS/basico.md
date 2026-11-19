# Conceitos basicos de node.js 
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

## // Extensões para VS Code //

**ESSENCIAIS**

- **Node extension pack**
- **Thunder Client** *(para testar as requisições. Lembre-se de criar uma )*

**RECOMENDAÇÕES**

- **Palenight Theme**
- **Symbols** *(pra deixar bonito :3)*

<div class="barrinha"></div>
<div class="barrinha"></div>

## // Primeiros passos //

- Crie uma pasta para sua **API** e a acesse no VScode
- Caso sua **API** esteja dentro de uma outra pasta, garanta que a pasta esteja selecionada no **Terminal**. 

*Comandos de terminal para lembrar*

> `ls` descobre quais são os diretorios dentro daquela pasta

> `cd` entra em um diretorio pelo nome

- Execute: `npm.cmd init -y`

- Dentro da pasta da API, crie uma pasta chamada *src* e, dentro dela, um arquivo chamado *server.js*. Esse será o arquivo principal da API

<div class="barrinha"></div>

## // Frameworks utilizados //

- Express       
> O principal que guia o funcionamento da API
- Pg            
> Para acessar o PostgreSQL com a API
- JsonWebToken  
> Para fazer a confirmação do
- Bcrypt
> Para criar o HASH dos dados sensiveis
- dotenv
> Para colocar as credenciais em um arquivo externo

Instale todos os frameworks após criar o projeto com o comando abaixo. **Certifiquece novamente que a pasta da API esteja selecionada no terminal**
> `npm.cmd install express pg jsonwebtoken bcrypt dotenv`

<div class="barrinha"></div>

## // Iniciando um servidor //

- Após fazer a configuração inicial de sua **API**, execute `node src/server.js` para iniciar o servidor node no localhost. 

*Caso o arquivo tenha um nome diferente ou esteja em outro diretório, coloque o caminho pertinente para acessa-lo dentro da API*



<style>

    h1{

        color: #5EFF6B;

    }

    h2{

        color: #82d889;

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

    .codigo{

        background-color: #353535;
        padding: 4px 15px;
        border-radius: 10px;
        color: #a0d1a4;

    }

</style>

