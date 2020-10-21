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

## +build => v0.1.? : builder le conteneur avec un DockerFile pour copier package.json et package-lock.json et effectuer les install npm nécessaires

### créer le fichier `webpack.config.js` à la racine

```js
webpack.config.js

module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
}
```
