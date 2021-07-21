# Contentful Migration Tool

[![Test](https://github.com/marcomontalbano/contentful-migration-tool/actions/workflows/test.yml/badge.svg)](https://github.com/marcomontalbano/contentful-migration-tool/actions/workflows/test.yml)
[![Docker](https://img.shields.io/docker/v/marcomontalbano/contentful-migration.svg?style=plastic&logo=docker&logoColor=white&style=flat)](https://hub.docker.com/r/marcomontalbano/contentful-migration)

Run Contentful migrations easier.


## CLI usage

[`contentful-migration`](https://github.com/contentful/contentful-migration) is a peerDependency, so it is required.

You can run directly this command:

```sh
npx -p contentful-migration@latest -p contentful-migration-tool contentful-migration-tool run ./migrations
```

Or you can install **contentful-migration** and **contentful-migration-tool** as devDependencies and then just run:

```sh
# install dependencies
npm install --save-dev contentful-migration@latest contentful-migration-tool

# run migrations
npx contentful-migration-tool run ./migrations
```

If you use **TypeScript**, you will also need **ts-node** to run TypeScript migrations:

```sh
# install dependencies
npm install --save-dev contentful-migration@latest contentful-migration-tool ts-node

# run migrations
npx ts-node ./node_modules/.bin/contentful-migration-tool run ./migrations
```

Remember to set the required environment variables before running the above commands.

### Options

#### Environment Variables

* **`CONTENT_MANAGEMENT_TOKEN`** - ***required*** - Contentful Content Management Token. You can create one from the section *API keys* under your space settings.

* **`SPACE_ID`** - ***required*** - Contentful Space ID. You can get the Space ID from the section *General settings* under your space settings. The Space ID is also visibile in the url.

* **`ENVIRONMENT_ID`** - ***required*** - Contentful Environment ID.

## Docker usage

```sh
docker run --rm --name contentful-migration-runner -e CONTENT_MANAGEMENT_TOKEN=$CONTENT_MANAGEMENT_TOKEN -e SPACE_ID=$SPACE_ID -e ENVIRONMENT_ID=$ENVIRONMENT_ID -v $(pwd)/migrations:/app/migrations marcomontalbano/contentful-migration
```

### Options

#### Environment Variables

* **`CONTENT_MANAGEMENT_TOKEN`** - ***required*** - Contentful Content Management Token. You can create one from the section *API keys* under your space settings.

* **`SPACE_ID`** - ***required*** - Contentful Space ID. You can get the Space ID from the section *General settings* under your space settings. The Space ID is also visibile in the url.

* **`ENVIRONMENT_ID`** - ***required*** - Contentful Environment ID.

#### Volumes

* **`/app/migrations`** - ***required*** - Migrations folder.

#### Arguments

* **`--cfmversion 4.0.0`** - ***optional*** - Use this argument if you want to change the `contentful-migration` version. (default to `latest`)


## Migrations folder

Either you use **Docker** or **CLI**, you should create a `/migrations` folder in your root folder. This folder will contain all your migration description files.

A migration description file is a `.js` or `.ts` file that contains a migration script. This scripts are written using [Contentful Migration](https://github.com/contentful/contentful-migration) syntax.

The filename must follow this naming convention:

**`<version>` `-` `<description>`** `.ts`

**version** starts from `1` and must be incremental.

**description** is used to easily recognize the purpose of the migration.

A real example can be: `1-create-author.ts`

----

e.g. `javascript`

```js
module.exports = function (migration, context) {
  const author = migration.createContentType('author');
  const name = author.createField('name');
  name.type('Symbol').required(true);
};
```

e.g. `typescript`

```ts
import { MigrationFunction } from 'contentful-migration'

const migrate: MigrationFunction = (migration) => {
  const author = migration.createContentType('author');
  const name = author.createField('name');
  name.type('Symbol').required(true);
}

export = migrate
```

[Read more](https://github.com/contentful/contentful-migration)
