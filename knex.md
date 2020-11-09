# [KNEX](https://knexjs.org/)

Constructeur de requêtes SQL

## Installation

de knex 

```bash
npm install knex --save
```

de la librairie de bdd appropriée (postgres ici)

```bash
npm install pg
```

## Initialisation de la librairie

```js
var knex = require('knex')({
  client: 'pg',
  connection: {
    host : '127.0.0.1',
    user : <database_user>,
    password : <database_password>,
    database : <databse_name>
  },
  [migrations: {
    directory: <migrations_directory_path>
  },]
  [migrations: {
    directory: <migrations_directory_path>
  },]
});
```

## Help

```bash
knex --help

Usage: cli [options] [command]

Options:
  -V, --version                   output the version number
  --debug                         Run with debugging.
  --knexfile [path]               Specify the knexfile path.
  --knexpath [path]               Specify the path to knex instance.
  --cwd [path]                    Specify the working directory.
  --client [name]                 Set DB client without a knexfile.
  --connection [address]          Set DB connection without a knexfile.
  --migrations-directory [path]   Set migrations directory without a knexfile.
  --migrations-table-name [path]  Set migrations table name without a knexfile.
  --env [name]                    environment, default: process.env.NODE_ENV || development
  --esm                           Enable ESM interop.
  --specific [path]               Specify one seed file to execute.
  --timestamp-filename-prefix     Enable a timestamp prefix on name of generated seed files.
  -h, --help                      display help for command

Commands:
  init [options]                          Create a fresh knexfile.
  migrate:make [options] <name>           Create a named migration file.
  migrate:latest [options]                Run all migrations that have not yet been run.
  migrate:up [<name>]                     Run the next or the specified migration that has not yet been run.
  migrate:rollback [options]              Rollback the last batch of migrations performed.
  migrate:down [<name>]                   Undo the last or the specified migration that was already run.
  migrate:currentVersion                  View the current version for the migration.
  migrate:list|migrate:status             List all migrations files with status.
  migrate:unlock                          Forcibly unlocks the migrations lock table.
  seed:make [options] <name>              Create a named seed file.
  seed:run [options]                      Run seed files.
  `help [command]                          display help for command`
```

## Migrations

nécessite un fichier de configuration `knexfile`

### le fichier de config peut être généré par la commande suivante (crée le fichier `knexfile.js` à la racine)

```bash
knex init
```

### créer des fichiers de migration

```bash
knex migrate:make [options] <migration_name>
```
