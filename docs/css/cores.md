# Cores e pintura
> Comol lidar com cores e paletas em CSS

- Para usar cores em **CSS**, alguns modelos diferentes podem ser usados baseado no ambiente usado, na preferencia do desenvolvedor e no resultado que busca atingir

## Modelos de cor

### RGB (Red-Green-Blue)

- Possui **3 canais de cor** *(red, green, blue)*, cujos valores podem variar de **0** até **255**, representando a intensidade de cada canal. A mistura de valores é o que cria a variedade de cores possivel.

- Pode ser aplicado a uma propriedade com:

`rgb(red, green, blue)`

*Os valores escritos devem ser substituidos pelos numeros supracitados*

- Pode ser modificada para **RGBA**, onde **A** *(Alfa)* simboliza a transparencia da cor

`rbg(128, 0, 128, 0.5)` *50% de opacidade*

### HEX (Hexadecimal)

- Possui um codigo de **6 caracteres** que define a cor a ser representada, começando com um simbolo de tralha **(#)**

- Cada par de caracteres equivale a um valor em **RGB**

`#ff0000`

- Pode ser modificado para possuir transparencia colocando mais **2 caracteres** no final.

`#ff000080` *50% de opacidade*

### HSL (Hue-Saturation-Lightness)

- Possui uma abordagem mais facil de ser entendida pelo ser humano

`hsl(hue, saturation, lightness)`

- Seus três elementos podem ser entendidos da seguinte forma:

    - **HUE** representa a cor em si a ser usada. Varia de 0 a 360, seguindo uma logica similar a uma roda de cores, onde *0* representa **vermelho**, *120* representa **verde** e *240* representa **azul**
    - **Saturation** representa a puresa/intensidade de uma cor. *0%* é saturação nenhuma *(escala de cinza)* e *100%* representa saturação completa
    - **Lightness** representa o brilho da cor. *0%* é completamente preto enquanto *100%* é completamente branco

- Pode ser modificado para possuir transparencia de forma similar ao **RGB**

`hsla(60, 30%, 50%, 1)`

### CMYK (Cyan, Magenta, Yellow, Key)

- Usado para imprimir e reproduzir cores. Atua de forma a **subtrair** cores, ao inves de **adicionar**, como no caso do **RGB**

- Uma pagina pode sofrer perdas durante a conversão de **RBG** para **CMYK** durante uma impressão. Utilize funções ou bibliotecas prontas para fazer a conversão quando for imprimir algo e manter a qualidade original da pagina.

## Propriedades relacionadas a cor

**propriedades gerais**

- color

**propriedades de caixa**

- background / background-color
- background-image
- border / border-color
- box-shadow
- outline-color

**propriedades de elemento**

- text-shadow
- fill
- caret-color
- column-fill



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