# Etapes d'installs : de 'old-school' à 'moderne javascript'

## old-school => v0.1.2 : un fichier index.html avec appel index.js et moment.min.js (sous /usr/shar/nginx/html sur le serveur)

### workspace : old-school

```json
./node_modules
  |
  |
index.html
index.js
moment.min.js (copié/collé de [moment.min.js](https://momentjs.com/downloads/moment.js))
docker-compose.yml
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
      volumes:
        - ./src:/usr/share/nginx/html
      networks:
        - nginx-proxy
      restart: unless-stopped
  networks:
    nginx-proxy:
      external: true
package.json
```

## +npm => v0.1.3 : install de moment.min.js via `npm` et appel dans node_module

### installer le module `moment`

```bash
npm install moment -P
```

### supprimer le fichier `moment.min.js` sous `./src`

```bash
rm ./src/moment.min.js
```

### dans `index.html` effectuer l'appel de `moment.min.js` depuis `node_module`

```html
index.hml

/* ... */
<script src="node_modules/moment/min/moment.min.js"></script>
/* ... */
```

### workspace : +npm

```json
./node_modules
  |
  moment
  |
index.html
index.js
-moment.min.js(deleted)
docker-compose.yml
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
      volumes:
        - ./src:/usr/share/nginx/html
      networks:
        - nginx-proxy
      restart: unless-stopped
  networks:
    nginx-proxy:
      external: true
package.json
```

## +webpack => v0.1.4 : build manuel de `index.js` avec `webpack`

### installer [`webpack`](https://webpack.js.org/concepts/)

```bash
npm i -D webpack webpack-cli
```

### compiler index.js avec `webpack`

```bash
./node_modules/.bin/webpack ./index.js --output-filename bundle.js
```

Cette commande génère le fichier `bundle.js` et son répertoire `dist`

OU

```bash
./node_modules/.bin/webpack ./index.js -o dist
```

Cette commande génère le fichier `main.js` et son répertoire `dist`

### Dans le fichier html, remplacer l'appel à `index.js` par `dist/main.js`

```html
index.hml

/* ... */
<!-- <script src="index.js"></script> -->
<script src="dist/bundle.js"></script>
/* ... */
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
docker-compose.yml
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
      volumes:
        - ./src:/usr/share/nginx/html
      networks:
        - nginx-proxy
      restart: unless-stopped
  networks:
    nginx-proxy:
      external: true
package.json
```

## configurer webpack => v0.1.5

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

Il est désormais possible de compiler index.js avec webpack sans indiquer de paramètre (le fichier de configuration par défaut étant webpack.config.js).
Exécuter la commande suivante après avoir supprimé le répertoire `/dist` va le recréer avec son contenu.

```bash
./node_modules/.bin/webpack
```

### optimiser le fichier webpack.config.js

```js
webpack.config.js
- se base sur le __dirname
- précise le répertoire de sortie
-----------------
const path = require("path");

module.exports = {
  entry: {
    main: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "bundle.js",
  },
};
```

### rendre le fichier de sortie `bundle.js` lisible en développement en créant un fichier de configuration adhoc

```js
webpack.dev.config.js
- précise le mode 'development'
- utilise l'outils de formatage 'source-map'
-----------------
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: path.resolve(__dirname, "./index.js"),
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "bundle.js",
  },
  devtool: "source-map",
};
```

### générer le fichier html à partir d'un template

#### installer le plugin `html-webpack-plugin`

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
    <script src="dist/bundle.js"></script>
  </head>

  <body class="bg-bg">
    <div id="root"></div>
    <h1>nicolaspetitot.com</h1>
    <h2>VPS expérimental</h2>

    <p>
      VPS à usage de formation sur le projet Camino de la Fabrique Numérique
    </p>
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
    filename: "bundle.js",
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

### créer des scripts adhoc dans le package.json

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

## +build => v0.1.? : builder le conteneur avec un DockerFile pour copier package.json et package-lock.json et effectuer les install npm nécessaires