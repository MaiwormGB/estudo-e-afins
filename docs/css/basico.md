# Conceitos basicos de css

> Linguagem utilizada para a estilização de paginas HTML

- Sigla significa **Cascading Style Sheets**
- Modifica a aparencia e fortamação de uma pagina HTML
- Podem ser aplicados **dentro de um elmento**,**dentro de um codigo HTML** ou **acessado de uma pagina externa atravez de um link**

Elementos são afetados por **CSS** atravez de **Regras (rules)**, tambem chamadas de **Style Rules**. Essas regras seguem o seguinte padrão:

**Seletor** {

    propriedade: valor;
    outra-propriedade: outro-valor;

}

### Seletor

- é o objeto afetado, podendo ser uma div, um tipo de texto, uma classe, um id, etc.

- **Propriedade** é uma caracteristica a ser alterada, como cor, tamanho, plano de fundo, etc.
- **Valor** é o parametro usado para alterar uma propriedade, como um hexadecimal de cor, uma quantia de pixels, etc...

- Varios seletores podem ser postos um na frente do outros para que o bloco de propriedades afete a todos.

- Seletores podem ser: 

    - **tipos de elementos** 

    *Ex* `button { /*estilo...*/}`

    - **classes** 
    
    *Ex* `.main { /*estilo...*/}`

    - **ids**

    *Ex* `#mensagens_lista { /*estilo...*/ }`
    
    - **Atributos** 

    *EX* `[trecho-destaque] { /*estilo...*/}`

    *Atributos em especial possuem algumas formas alternativas de serem usados*

    **Tem valor exato**

    `[input-type = "number"] { /*estilo...*/}`

    **Possui valor em si**

    `[class *= "button"] { /*estilo...*/ }`

    *Nesse exemplo, outras classes podem coexistir no elemento sem invalidar o seletor*

    **Começa com o valor**

    `[href ^= "https://"] { /*estilo...*/ }`

    **Termina com o valor**

    `[src $= ".png"] { /*estilo...*/ }`

    - **Pseudo-classes**

    *Baseados em especificos estados ou interações*

    **EX** `.button:hover { /*estilo...*/ }`

    - **Pseudo-elementos**

    *Permite afetar partes especificas de um elemento*

    **EX** `.button::before { /*estilo...*/ }`

### Relações de prioridade
> Que regra sera usada quando multiplas regras conflitantes são aplicadas

- Geralmente, seguem a hierarquia de prioridade
**(Do com mais pro com menos)**

    - **Inline styles** *(Estilos dentro de um elemento html)*
    - **Id** 
    - **Classes**
    - **Seletores de elemento**

- Quanto mais prioridade um seletor possui *(quanto mais especifico ele é)*, maior a probabilidade daquele estilo ser aplicado ao elemento.

- Elementos filhos **herdarão caracteristicas pertinentes de seu elemento pai** que tenham sido especificadas no mesmo. Seletores que afetam elementos filhos possuiem **maior prioridade** que caracteristicas do elemento pai, que são sobrepostas nesse contexto.

- Colocar **!important** na frente do valor de uma propriedade faz a mesma sobrescrever qualquer outra propriedade anteriormente declarada para aquele seletor.

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

