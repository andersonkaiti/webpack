# Webpack 5 (Babel, PostCSS, npx, Node.js e npm)

O Webpack é uma ferramenta que os desenvolvedores JavaScript utilizam para realizar algumas operações comuns feitas para que a aplicação, desenvolvida em ambiente de desenvolvimento, `esteja pronta para o ambiente de produção`, com as devidas `otimizações` para os arquivos JavaScript e CSS.

# Operações Comuns

- Empacota os arquivos JavaScript (uma aplicação front-end normalmente tem vários arquivos) em um arquivo JavaScript final.
- Cria arquivos chamados `chunks`, também chamados de `lazy loading`, que servem para `otimizar o carregamento inicial da aplicação` e carregar outros chunks `sob demanda`. Quando o usuário entra no site, o chunk inicial é carregado primeiro e, à medida que o usuário navega pelo site, chunks adicionais são carregados conforme necessário, para que o carregamento inicial seja muito rápido.
- `Tree shaking`, que remove funções e variáveis declaradas mas `não utilizadas` na aplicação.
- `Minifying`, que remove os `espaços em branco` para tornar o arquivo de build o mais leve possível.
- `Babel transpiler`, que `transpila o código JSX para JavaScript antigo`, convertendo tags em funções React.

```
JSX:
<p>
    Hello, World!
</p>

JavaScript:
React.createElement(
    "p",
    null,
    "Hello, World!"
);
```

Quando a aplicação vai para a produção, precisamos otimizá-la, removendo os espaços em branco, funções inutilizadas etc., tornando o código o menor possível.

O Webpack fornece um `workflow` para realizar todas essas operações com uma única ferramenta.

# CSS Optimization (normalmente com PostCSS).

Também é possível trabalhar assim com CSS, pois é comum ter um arquivo CSS por componente, então podemos empacotar 30 arquivos em um arquivo completo, além de outras operações, como:

- `Vendor prefixes` (-webkit-box-shadow, por exemplo), que assegura que o estilo `funcione em todos os navegadores` (semelhante ao Babel).
- `Minification`, que `remove espaços em branco` para otimizar o arquivo final.
- `Converte` px para rem.

# Development Server

- Ele também fornece um servidor de desenvolvimento.
- Temos live reload durante o desenvolvimento.

# Modularidade

Modularidade é essencial pois, se trabalharmos apenas em um arquivo JavaScript, podem haver colisões de nomes iguais para variáveis e funções, pois muitas das vezes apenas aquele nome faz sentido.

No passado, para `implementar modularidade`, os desenvolvedores encapsulavam os trechos do código em `IIFEs`. No entanto, se dois desenvolvedores trabalharem no mesmo arquivo, um dos desenvolvedores pode salvar o arquivo e o outro estará trabalhando em um arquivo desatualizado, então dividir o projeto em vários arquivos pode ser ruim.

```
(() => {
    const paginationEl = document.querySelector(".pagination");

    const clickHandler = () => {
        // ...
    }

    paginationEl.addEventListener("click", clickHandler);
})();

(() => {
    const sortingEl = document.querySelector(".sorting");

    const clickHandler = () => {
        // ...
    }

    sortingEl.addEventListener("click", clickHandler);
})();
```

Para dividir o projeto em múltiplos arquivos, criamos uma estruturação de pastas e arquivos.

- `/src:` onde todos os arquivos são colocados.
- `/dist (distribution) ou /build`.

No entanto, no passado não era possível dividir o código em vários arquivos. Para fazer isso, é necessário utilizar o Webpack para empacotar tudo em uma arquivo e importá-lo no código HTML. Porém, nos dias atuais, `o navegador suporta o ES Modules`, que permite utilizar `import` e `export`, basta adicionar `type="module"` na tag `script`.

# Isso torna o Webpack defasado?

Não, porque:

- Podemos não querer ter 50 arquivos JavaScript, já que isso implicaria inúmeros network requests a cada import, mas sim apenas um arquivo final ou chunks (que são carregados sob demanda).
- Além disso, caso algo seja criado no código, mas não seja utilizado nele, o Webpack não incluirá isso no arquivo final.
- Ele também pode transpilar utilizando Babel: como transformar uma arrow function em uma função tradicional.
- Também podemos empacotar 50 arquivos CSS em um único arquivo e o Webpack fará otimizações, como remover espaços em branco, adicionar vendors prefixes para fazer funcionar em todos os navegadores, equivalente ao que o Babel faz com o JavaScript.

# npm - Webpack

O [**Webpack**](https://www.npmjs.com/package/webpack) se tornou popular por ser o empacotador padrão do React.js e ele se trata de código JavaScript que iremos executar. Ele pode ser instalado a partir do npm, que é o gerenciador de pacotes do Node.js.

Para instalar o pacote Webpack, basta instalar o Node.js e utilizar `CLIs` no `Prompt de Comando`, que é um `software standalone`, ou no `terminal integrado` do `Visual Studio Code`, que se trata de um `IDE`, pois ele fornece um `ambiente com várias ferramentas integradas de forma nativa` e vai `muito além de apenas permitir editar arquivos`.

Em primeiro lugar, é necessário `inicializar` o projeto com para o npm.

```
npm init -y
```

E instalar os pacotes `Webpack`, que é basicamente o núcleo do Webpack, e o `Webpack-CLI`, que será utilizado para `interagir` com ele (para interagir, podemos criar um novo arquivo ou utilizar comandos, que é o caso do webpack-cli). Em seguida, é adicionada a `flag --save-dev` para que os pacotes sejam salvos no arquivo `package.json` e especificar que eles apenas serão utilizados em `ambiente de desenvolvimento`.

```
npm install webpack webpack-cli --save-dev
```

# node_modules volumoso

Quando o pacote Webpack é instalado, por exemplo, é possível visualizar na `pasta node_modules/webpack` que ele `também possui um package.json` e, consequentemente, ele também `dependende de vários pacotes`. Por esse motivo que, ao instalar um pacote, `vários outros são instalados`.

# Configuração do Webpack

Como já dito anterior, o `Webpack é uma ferramenta`, e como uma ferramenta, `ele contém várias opções`. Por conta disso, é necessário criar um `arquivo de configuração na raiz do projeto` chamado `webpack.config.js`. Nele, utiliza-se o `CommonJS` para `exportar` algumas configurações, como o `modo` em que ele será utilizado (de produção), o `ponto de entrada` da aplicação e o nome do `arquivo final` que será gerado ao final do empacotamento.

```
module.exports = {
    mode: "production",
    entry: "./src/index.js",
    output: {
        filename: "main.js"
    }
}
```

# Execução do Webpack

O npm criará uma pasta .bin (binários) com os executáveis, então basta adicionar um `script` ao `package.json`.

```
"scripts": {
    "build": "webpack"
},
```

E executar o comando `npm run build`.

```
npm run build

> webpack@1.0.0 build
> webpack

asset main.js 180 bytes [emitted] [minimized] (name: main)
orphan modules 482 bytes [orphan] 3 modules
./src/index.js + 3 modules 553 bytes [built] [code generated]
```

E o `arquivo main.js` com o código empacotado será gerado.

```
(()=>{"use strict";const e=document.querySelector(".pagination"),t=document.querySelector(".sorting");e.addEventListener("click",(()=>{})),t.addEventListener("click",(()=>{}))})();
```

# Npx

Também é possível utilizar o Webpack sem adicioná-lo aos scripts com o `CLI npx`.

```
npx webpack
asset main.js 180 bytes [emitted] [minimized] (name: main)
orphan modules 482 bytes [orphan] 3 modules
./src/index.js + 3 modules 553 bytes [built] [code generated]
```

# Babel Transpiler

O empacotamento foi realizado corretamente, mas ainda é necessário `adaptar o código JavaScript para ser suportado` por `navegadores antigos`, pois o código utiliza `recursos do ES6`, como `arrow functions` e `constantes`.

Para isso, o `transpilador Babel` será instalado no projeto utilizando o `@babel/core`, que é o núcleo do Babel, o `@babel/preset-env`, que fornece algumas `predefinições por padrão`, e o `babel-loader`, pois utilizaremos o Babel no `contexto do Webpack` e, para isso, utilizamos `loaders` para `integrá-lo ao workflow do Webpack`.

```
npm install @babel/core @babel/preset-env babel-loader --save-dev
```

`Todos os arquivos JavaScript são tratados como pacotes`, então é necessário definir algumas `regras` que o Webpack aplicará caso os pacotes sejam encontrados. Para isso, a propriedade module será criada e, dentro dela, haverá um `array que armazenará todas as regras`. O primeiro elemento indica que o Webpack utilizará o `babel-loader` caso a `extensão` do arquivo seja `.js`, que será encontrado com a `expressão regular \.js`, e excluindo os arquivos presentes no `diretório node_modules`.

```
module: {
    rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            use: ["babel-loader"]
        }
    ]
}
```

Quando o Babel for executado, ele buscará por uma série de arquivos, e um deles é o `babel.config.js`, ou `babelrc`, que conterá as configurações dele. Nele, é necessário indicar o `preset`, que é uma `conjunto pré-definido de opções`, além de polyfills (core-js), que servem para fazer um recurso novo, e que não existe em versões mais antigas, funcionar.

```
npm install core-js --save-dev
```

```
module.exports = {
    presets: [
        [
            "@babel/preset-env",
            {
                useBuiltIns: "usage",
                corejs: "3.0"
            }
        ]
    ]
};
```

# Executando o Webpack com o Babel

```
npx webpack
asset main.js 190 bytes [emitted] [minimized] (name: main)
orphan modules 501 bytes [orphan] 3 modules
./src/index.js + 3 modules 571 bytes [built] [code generated]
```

```
(()=>{"use strict";var e=document.querySelector(".pagination"),t=document.querySelector(".sorting");e.addEventListener("click",(function(){})),t.addEventListener("click",(function(){}))})();
```

# Configurando o package.json para o Babel

O Babel está transformando `const em var`, mas as `arrow functions não são convertidas` em funções tradicionais compatíveis com navegadores mais antigos. Para isso, dentro do `arquivo package.json`, é necessário adicionar a `propriedade browserslist` e a `porcentagem`, que indicará que ele deve ser compatível com navegadores que representam mais de 0.2% de uso de mercado. Não é recomendável adicionar todos os navegadores, pois haveriam mais polyfills e mais códigos extras, tornando o código final mais volumoso.

```
"browserslist": [
    ">0.2%"
],
```

Após adicionar essa configuração, o Webpack será executado de novo.

```
npx webpack
asset main.js 195 bytes [emitted] [minimized] (name: main)
orphan modules 501 bytes [orphan] 3 modules
./src/index.js + 3 modules 571 bytes [built] [code generated]
```

E, desta vez, as arrow functions foram substituídas por funções tradicionais.

```
!function(){"use strict";var e=document.querySelector(".pagination"),n=document.querySelector(".sorting");e.addEventListener("click",(function(){})),n.addEventListener("click",(function(){}))}();
```

# Webpack para arquivos CSS

Para realizar essas otimizações para arquivos CSS, é necessário instalar alguns pacotes. O primeiro é o `PostCSS`, cujo pacote é o `postcss-loader`, que é um loader, pois será utilizado no contexto do Webpack, e em seguida os `plugins`, como o `cssnano`, que será responsável pela operação de `minify dos arquivos CSS`, o `autoprefixer`, que adicionará os `vendors prefixers`, equivalente ao Babel, e o `postcss-pxtorem`, que trocará os valores com unidade `pixels por rem`.

```
npm install postcss-loader cssnano autoprefixer postcss-pxtorem --save-dev
```

Após a instalação dos pacotes, é necessário `configurar o Webpack` para utilizar o `postcss-loader`, indicando que o PostCSS será executado em arquivos com a extensão .css.

```
rules: [
    {
        test: /\.css$/,
        use: ["postcss-loader"]
    }
]
```

E também criar um arquivo de configuração chamado de `postcss.config.js`, já que ele é uma ferramenta que pode conter várias opções de configuração e funcionar de maneiras distintas. É necessário especificar quais `plugins` o `PostCSS` deve utilizar, no entanto, os `plugins instalados são pacotes`, então é necessário utilizar a `função require` para importar todos eles. Ao importar o `pacote postcss-pxtorem`, podemos chamar a função logo na importação e passar como `argumento` um objeto que indicará quais `propriedades devem ter seus valores convertidos` de `pixels para rem`. Nele, é necessário utilizar a `propriedade propList` que será um array com a `string "*"`, indicando que abrangerá `todos` os valores cuja unidade é o pixel.

```
module.exports = {
    plugins: [
        require("autoprefixer"),
        require("cssnano"),
        require("postcss-pxtorem")({ propList: ["*"] })
    ]
};
```

# Extraindo o CSS em um arquivo final

Para finalmente empacotar o CSS em um arquivo final, é necessário instalar os seguintes pacotes.

```
npm install css-loader mini-css-extract-plugin --save-dev
```

Para utilizar o `plugin mini-css-extract-plugin` instalado, é necessário adicionar a propriedade plugins. A propriedade será um array que aceitará o `construtor do MiniCssExtractPlugin` e, como argumento, um objeto será passado com a `propriedade filename`, que indicará o nome do arquivo final.

```
plugins: [
    new MiniCssExtractPlugin({
        filename: "main.css"
    })
]
```

À medida que Webpack é executado, ele precisa passar pelo postcss-loader, o css-loader e o MiniCssExtractPlugin.loader, que serão adicionados no array da `propriedade use`.

```
module: {
    rules: [
        {
            test: /\.css$/,
            use: [MiniCssExtractPlugin.loader, "css-loader", "postcss-loader"]
        }
    ]
}
```

# Executando o Webpack com o PostCSS

```
npx webpack
asset main.js 195 bytes [compared for emit] [minimized] (name: main)
asset main.css 187 bytes [emitted] (name: main)
Entrypoint main 382 bytes = main.css 187 bytes main.js 195 bytes
orphan modules 3.45 KiB (javascript) 997 bytes (runtime) [orphan] 11 modules
cacheable modules 593 bytes (javascript) 186 bytes (css/mini-extract)
  ./src/index.js + 3 modules 593 bytes [built] [code generated]
  css ./node_modules/css-loader/dist/cjs.js!./node_modules/postcss-loader/dist/cjs.js!./src/index.css 186 bytes [built] [code generated]
```

```
*,:after,:before{box-sizing:border-box;margin:0;padding:0}button{background-color:#6d6d6d;border:none;color:#fff;margin:1em;padding:1em}button:hover{background-color:#000;cursor:pointer}
```

# Arquivos finais

Após a execução completa do Webpack, basta copiar e colar o arquivo index.html, que é o ponto de entrada da aplicação, e importar os arquivos main.css e main.js otimizados. O diretório /dist, ao final, ficará da seguinte forma.

```
└───dist
    │   index.html
    │   main.css
    │   main.js
```