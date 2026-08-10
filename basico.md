<h1 >CONCEITOS BASICOS DE NODE.JS</h1>

<blockquote> Explicação da linguagem e fundamentos iniciais </blockquote>

<h2>// O que é Node.js? //</h2>

<p> A linguagem foi criada com o objetivo de permitir que codigos js <br> rodem fora do navegador. Permite a criação de APIs e Backends para aplicações, alem de automações de tarefas </p>

<h2>// Como instalar o Node.JS //</h2>

<p>Basta ||</p>

<ul> <li> Acessar o <a href="https://nodejs.org/pt-br">site oficial do Node.JS</a> </li> <li> Procurar por "Other downloads" </li> <li> Escolha "Installing Node.JS via package manager" e procure seu sistema operacional </li> </ul>

<p> Essa forma de download permite melhor controle de versão e de desinstalação caso algum problema aconteça. </p>

<p> <strong><i>A Extenção usada pelo Node é .js, geralmente chamado de server.js</i></strong> </p>

<h2>// Módulos do Node.JS //</h2>

<p> Ao começar um arquivo node, você deve começar o arquivo com </p>

<pre><code>import {} from 'node:*MODULO'</code></pre>

<p> Existem varios modulos que podem entrar no lugar de *MODULO, onde cada um possui sua própria função baseado naquilo que você quer realizar com a aplicação </p>

<h3>Alguns modulos notorios</h3>

<h4>// Node:http</h4>

<ul> <li> Criação de servidores HTTP. Nesse caso em especifico, é preciso colocar { create server } logo após o import para auxiliar em tal criação </li> </ul>

<h4>// Node:fs</h4>

<ul> <li> Sigla para "File Sistem". Permite a manipulação de arquivos (criar, ler, deletar, etc) </li> </ul>

<h4>// Node:crypto</h4>

<ul> <li> Usado para lidar com criptografia </li> </ul>
