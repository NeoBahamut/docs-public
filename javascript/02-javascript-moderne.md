# Etapes d'installs : de 'old-school' à 'moderne javascript'

## old-school : un fichier index.html avec appel index.js et moment.min.js (sous /usr/shar/nginx/html sur le serveur)

### index.html

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="utf-8" />
    <title>Dinausors</title>
    <script src="moment.min.js"></script>
    <script src="index.js"></script>
  </head>

  <body class="bg-bg">
    <h1>Hello from HTML</h1>
  </body>
</html>
```

### index.js

```js
console.log("Hello from JS");
console.log(moment().startOf("day").fromNow());
```

### moment.min.js

copié/collé de [moment.min.js](https://momentjs.com/downloads/moment.js)

```js
//! moment.js
//! version : 2.29.1
//! authors : Tim Wood, Iskren Chernev, Moment.js contributors
//! license : MIT
//! momentjs.com
...
```

### docker-compose.local.yml (destiné au dev: tourne sur localhost)

```yml
version: "3"
services:
  app:
    container_name: com_nicolaspetitot
    image: nginx:alpine
    environment:
      VIRTUAL_HOST: ${URL}
      LETSENCRYPT_HOST: ${URL}
      LETSENCRYPT_EMAIL: ${EMAIL}
    expose:
      - 80
    ports:
      - 8080:80
    volumes:
      - .:/usr/share/nginx/html
    networks:
      - nginx-proxy
    restart: unless-stopped
networks:
  nginx-proxy:
    external: true
```

### workspace : old-school

```json
index.html
index.js
moment.min.js (copié/collé de [moment.min.js](https://momentjs.com/downloads/moment.js))
docker-compose.local.yml
```

## +npm : install de moment.min.js via `npm` et appel dans node_modules

### initialiser npm sur le projet => crée le fichier `package.json`

```bash
npm init
```

### installer le module `moment` :  mode production avec `--save-prod` ou `-P` ou par défaut 

```bash
    npm i moment
<=> npm install moment
<=> npm install moment --save-prod
<=> npm install moment -P
```

### supprimer le fichier `moment.min.js` sous `./src`

```bash
rm ./src/moment.min.js
```

### dans `index.html` effectuer l'appel de `moment.min.js` depuis `node_module`

```html
index.hml
=========

/* ... */
<script src="node_modules/moment/min/moment.min.js"></script>
/* ... */
```

### workspace : +npm

```json
./node_modules
  moment
index.html
index.js
[-moment.min.js(deleted)]
docker-compose.local.yml
package.json
```

## +webpack : build manuel de `index.js` avec `webpack`

### tenter d'importer `moment` avec `require` dans `index.js` (et supprimer la src depuis  index.html)

```js
index.js
========

var moment = require("moment");

console.log("Hello from JS");
console.log(moment().startOf("day").fromNow());
```

### dans `index.html` suprimer l'appel de `moment.min.js` depuis `node_module`

```html
index.hml
=========

/* ... */
<!-- <script src="node_modules/moment/min/moment.min.js"></script> -->
/* ... */
```

### installer [`webpack`](https://webpack.js.org/concepts/)

```bash
npm i -D webpack webpack-cli

OU

# la latest de webpack-cli fait planter !
npm i -D webpack webpack-cli@4.1.0

```

### compiler index.js avec `webpack`

```bash
./node_modules/.bin/webpack ./index.js --output-filename bundle.js
```

Cette commande génère le fichier `bundle.js` et son répertoire `dist` (par défaut)

OU

```bash
./node_modules/.bin/webpack ./index.js -o dist
```

Cette commande génère le fichier `main.js` (par défaut) et son répertoire `dist`

### Dans le fichier html, remplacer l'appel à `index.js` par `dist/bundle.js`

```html
index.hml
=========

/* ... */
<!-- <script src="index.js"></script> -->
<script src="dist/bundle.js"></script>
/* ... */
```

### Adapter la source dans le fichier du docker-compose (docker-compose.local.yml encore pour le moment en dev)

```yml
version: "3"
services:
  app:
    container_name: com_nicolaspetitot
    image: nginx:alpine
    environment:
      VIRTUAL_HOST: ${URL}
      LETSENCRYPT_HOST: ${URL}
      LETSENCRYPT_EMAIL: ${EMAIL}
    expose:
      - 80
    ports:
      - 8080:80
    volumes:
      - ./dist:/usr/share/nginx/html
    networks:
      - nginx-proxy
    restart: unless-stopped
networks:
  nginx-proxy:
    external: true
```

### workspace : +webpack

```json
./dist
  |
  ├── bundle.js
./node_modules
  |
  moment
  |
  webpack
  webpack-cli
  |
index.html
index.js
docker-compose.local.yml
package.json
```

## + config webpack : ajout du fichier `webpack.config.js`

### créer le fichier `webpack.config.js` à la racine

```js
webpack.config.js
-----------------
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
}
```

Il est désormais possible de compiler index.js avec webpack sans indiquer de paramètre (le fichier de configuration par défaut étant webpack.config.js, le répertoire 'dist' est également par défaut).
Exécuter la commande suivante après avoir supprimé le répertoire `/dist` va le recréer avec son contenu.

```bash
./node_modules/.bin/webpack
```

### optimiser le fichier webpack.config.js

```js
webpack.config.js
- se base sur le __dirname
- précise le répertoire de sortie
- utilise le contenthash pour la gestion du cache
-----------------
const path = require("path");

module.exports = {
  entry: {
    bundle: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "[name].[contenthash].js",
  },
};
```

### optionnel : rendre le fichier de sortie `main.hash.js` "lisible" (non formaté) en développement en créant un fichier de configuration adhoc

```js
webpack.dev.config.js
- précise le mode 'development' (formate le fichier de sortie en lecture humaine)
- utilise 'source-map' (les erreurs sont indiquées dans le source !)
-----------------
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    bundle: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "[name].[contenthash].js",
  },
  devtool: "source-map",
};
```

### optionnel : créer des scripts adhoc dans le package.json

```json
package.json
------------
/* ... */
  "scripts": {
    /* ... */
    "build": "webpack",
    "dev:build": "webpack --config webpack.dev.config.js",
    /* ... */
  },
/* ... */
```

### générer le fichier html à partir d'un template

#### installer le plugin [`html-webpack-plugin`](https://github.com/jantimon/html-webpack-plugin)

```bash
npm i -D html-webpack-plugin
```

#### créer le fichier `template.html`, incluant des variables et une div 'root'

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="utf-8" />
    <title><%= htmlWebPackPlugin.options.title %></title>
  </head>

  <body class="bg-bg">
    <div id="root"></div>
  </body>
</html>
```

#### créer une propriété `plugins` dans le fichier de configuration de webpack

```js
webpack.conf.js
---------------
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    main: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "[name].[contenthash].js",
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "nicolaspetitot.com",
      template: path.resolve(__dirname, "./template.html"),
      filename: "index.html",
    }),
  ],
};
```

#### modifier le fichier source `index.js`

```js
index.js
--------
/* ... */
// Tester html-webpack-plugin : js -> html
const h1 = document.createElement("h1");
h1.textContent = "nicolaspetitot.com";

const h2 = document.createElement("h2");
h2.textContent = "VPS expérimental";

const p = document.createElement("p");
p.textContent =
  "VPS à usage de formation sur le projet Camino de la Fabrique Numérique";

const app = document.querySelector("#root");
app.append(h1);
app.append(h2);
app.append(p);
/* ... */
```

#### modifier la source du volume dans le `docker-compose.yml` et relancer le docker-compose

```yml
/* ... */
    volumes:
      - ./dist:/usr/share/nginx/html
/* ... */
```

```bash
docker-compose restart
```

#### supprimer le fichier `index.html` à la racine

```bash
rm ./index.html
```

### optionnel : cleaner le répertoire `dist` après chaque build (ne garder que les fichiers issus du dernier build)

#### installer le plugin [clean-webpack-plugin](https://github.com/johnagan/clean-webpack-plugin)

```bash
npm i -D clean-webpack-plugin
```

#### compléter le fichier `webpack.config.js`

```js
webpack.conf.js
---------------
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
  entry: {
    main: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "[name].[contenthash].js",
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "nicolaspetitot.com",
      template: path.resolve(__dirname, "./template.html"),
      filename: "index.html",
    }),
    new CleanWebpackPlugin(),
  ],
};
```

### optionnel : tester l'app sur un server http autonome en localhost avec `http-server` (c'est magique !)

#### installer http-server

```bash
npm i http-server
```

#### démarrer le serveur

```bash
npx http-server [/dist] # pour lancer le index.html compilé dans /dist, le répertoire par défaut étant ./public s'il existe ou ./ sinon
```

### Utiliser des [`loaders`](https://www.taniarascia.com/how-to-use-webpack/#modules-and-loaders)

Le but est de pré-traiter, en utilisant les modules, les fichiers chargés tels que :

- fichiers javascript classiques
- images
- css
- compilateurs (Babel, TypeScript)

#### Installer le compiler Javascript [Babel](https://babeljs.io/)

```bash
npm i -D babel-loader @babel/core @babel/preset-env @babel/plugin-proposal-class-properties
```

OU

```bash
npm i -D babel-loader babel-core babel-preset-env @babel/plugin-proposal-class-properties
```

#### compléter le fichier `webpack.config.js` avec la propriété `module`

```js
webpack.conf.js
---------------
/* ... */
  module: {
    rules: [
      // JavaScript
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ["babel-loader"],
      },
    ],
  },
/* ... */
```

#### tester l'interprétation d'ancien code

```js
index.js
--------
/* ... */
// Tester du code ancien
// créer une instance de classe sans constructeur
class StartUp {
  name = "camino";
}

const myStartUp = new StartUp();

const p2 = document.createElement("p");
p2.textContent = `${myStartUp.name} est top !`;

app.append(p2);
/* ... */
```

génère une erreur au build :

```bash
ERROR in ./index.js 26:7
Module parse failed: Unexpected token (26:7)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
| // créer une instance de classe sans constructeur
| class StartUp {
>   name = "camino";
| }

Add @babel/plugin-proposal-class-properties (https://git.io/vb4SL) to the 'plugins' section of your Babel config to enable transformation.

```

#### corriger l'erreur par l'ajout du fichier `.babelrc` et des options dans les règles du module

```js
.babelrc
--------
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

```js
webpack.conf.js
---------------
/* ... */
  module: {
    rules: [
      // JavaScript
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
          },
        },
      },
    ],
  },
/* ... */
```

## +build => v0.1.6 : builder le conteneur avec un Dockerfile

```yml
# multi staging

# build à partir d'une image 'node'
FROM node:13-alpine as build-stage
LABEL maintainer=nicolas.petitot@developpement-durable.gouv.fr

WORKDIR /app

# copie de package.json et package-lock.json afin d'effectuer les installations npm nécessaires
COPY package*.json ./
RUN npm ci

# compie des sources
COPY index.js ./
COPY template.html ./
COPY webpack.config.js ./
COPY .babelrc ./

# build de l'application
RUN npm run build

# serveur nginx à partir d'une image nginx
FROM nginx:alpine as production-stage
WORKDIR /app

# copie du répertoire /dist issu de la compilation précédente
COPY --from=build-stage /app/dist ./app
```

### workspace : final

```json
./dist
  |
  ├── index.html
  ├── main.ab793889e4382725a7e8.js
./node_modules
  |
  @babel
  |
  moment
  |
  webpack
  webpack-cli
  |
.babelrc
docker-compose.yml
  version: "3"
    services:
      app:
        container_name: com_nicolaspetitot
        build: .
        environment:
          VIRTUAL_HOST: ${URL}
          LETSENCRYPT_HOST: ${URL}
          LETSENCRYPT_EMAIL: ${EMAIL}
        expose:
          - 80
        volumes:
          - ./dist:/usr/share/nginx/html
        networks:
          - nginx-proxy
        restart: unless-stopped
    networks:
      nginx-proxy:
        external: true
Dockerfile
index.js
package.json
template.html
webpack.config.js
```
