# Etapes d'installs : de 'old-school' à 'moderne javascript'

## old-school => v0.1.5 : un fichier index.html avec appel index.js et moment.min.js (sous /usr/shar/nginx/html sur le serveur)

### workspace : old-school

```bash
./node_modules
  |
  |
./src
  index.html
  index.js
  moment.min.js (copié/collé de [moment.min.js](https://momentjs.com/downloads/moment.js))
docker-compose.yml
    nginx-proxy
    app
      volumes
        ./src:/usr/share/nginx/html
package.json
```

## +npm => v0.1.6 : install de moment.min.js via `npm` et appel dans node_module

### installer le module `moment`

```bash
npm install moment -P
```

### supprimer le fichier `moment.min.js` sous `./src`

```bash
rm ./src/moment.min.js
```

### dans `index.html` effectuer l'appel de `moment.min.js` depuis `node_module`

```bash
/* ... */
<script src="../node_modules/moment/min/moment.min.js"></script>
/* ... */
```

### workspace : +npm

```bash
./node_modules
  |
  moment
  |
./src
  index.html
  index.js
docker-compose.yml
    nginx-proxy
    app
      volumes
        ./src:/usr/share/nginx/html
package.json
```

## +build => v0.1.7 : builder le conteneur avec un DockerFile pour copier package.json et package-lock.json et effectuer les install npm nécessaires

## +webpack => v0.1.8 : install via `npm` de `webpack` et `webpack-cli`

### installer `webpack`

```bash
npm i -D webpack webpack-cli
```

### workspace : +webpack

```bash
./node_modules
  |
  moment
  |
  webpack
  webpack-cli
  |
./src
  index.html
  index.js
docker-compose.yml
    nginx-proxy
    app
      volumes
        ./src:/usr/share/nginx/html
package.json
```
