# CONCEITOS BASICOS DE CSS

> Linguagem utilizada para a estilização de paginas HTML

- Sigla significa **Cascading Style Sheets**
- Modifica a aparencia e fortamação de uma pagina HTML
- Podem ser aplicados *dentro de um elmento*,*dentro de um codigo HTML* ou *acessado de uma pagina externa atravez de um link*

Elementos são afetados por **CSS** atravez de **Regras (rules)**, tambem chamadas de **Style Rules**. Essas regras seguem o seguinte padrão:

**Seletor** {

    propriedade: valor;
    outra-propriedade: outro-valor;

}

Onde:

### Seletor

- é o objeto afetado, podendo ser uma div, um tipo de texto, uma classe, um id, etc.

- Varios seletores podem ser postos um na frente do outros para que o bloco de propriedades afete a todos.

- **Propriedade** é uma caracteristica a ser alterada, como cor, tamanho, plano de fundo, etc.
- **Valor** é o parametro usado para alterar uma propriedade, como um hexadecimal de cor, uma quantia de pixels, etc...

<style>

    h1{

        color: #5ed1ff;

    }

    h2{

        color: #8dc1eb;

    }


    strong, h3{

        color: #a2ceff;

    }

    em{

        color: #778fd3;

    }

    .barrinha{

        width: 100%; 
        height: 5px; 
        background-color: #5eb7ff; 
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

