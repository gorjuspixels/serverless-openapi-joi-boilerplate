# Serverless OpenAPI Joi Boilerplate Project
[![Build Status](https://travis-ci.org/anttiviljami/serverless-openapi-joi-boilerplate.svg?branch=master)](https://travis-ci.org/anttiviljami/serverless-openapi-joi-boilerplate) [![License](http://img.shields.io/:license-mit-blue.svg)](http://anttiviljami.mit-license.org)

Boilerplate serverless API project with generated OpenAPI docs (formerly known as swagger) from Joi validation.

Inspired by Hapi and [hapi-swagger](https://github.com/glennjones/hapi-swagger)

- [x] **Auto-generated OpenAPI docs from code** at endpoint `/swagger.json` (configurable)
- [x] **Joi validated requests**
- [x] Local development with serverless-offline + Docker
- [x] Knex migrations & query builder on PostgreSQL
- [x] Example CRUD API
- [x] Tests with jest
- [x] Local Swagger UI to view OpenAPI docs
- [x] Travis CI configuration for serverless deployment
- [x] Typescript configuration with sensible defaults
- [x] Prettier + Tslint

## Quick Start

Requirements:
- Node.js v8+
- Docker

```
cp .env.sample .env # Set up environment variables
source .env
npm install
docker-compose up --detach # PostgreSQL running, Swagger UI listening at http://localhost:9001
npm run migrate # Set up database schema with knex migrations
npm run dev # Serverless offline listening at http://localhost:9000
```

## Codebase

- `.env` shell environment file for development, use `.env.sample` to create your own
- `serverless.yml` serverless config file, defines functions, endpoints and cloudformation resources
- `src/**` typescript source code
  - `src/handler.ts` serverless main entrypoint, this is where all http requests and function invocations start from
  - `src/routes.ts` where api routes are defined
  - `src/validation/**` joi validation definitions for api routes
  - `src/handler/**` handlers for when routing is finished, this is where control logic happens
  - `src/core/**` core business logic + reading & writing to database) happens here
  - `src/util/**` boring utilities like router logic and helpers are stored here
  - `src/types/**` type definitions (.d.ts files)
  - `src/migrations/**` knex database migration files (use `knex migrate:make` to create these)
  - `src/seeds/**` knex database seed files (use `knex seeds:make` to create these)
- `__tests__/**` jest tests
- `docker-compose.yml` docker compose file, defines Swagger UI and PostgreSQL containers for development

## Deploy

Make sure your AWS IAM access is set-up with sufficient privileges and access keys are set.
```
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
```

Full deploy with Serverless. (This will take some time on the first time)
```
npm run build # build project
serverless deploy --stage dev # deploy stage
```

In order to save time, you can also just deploy a single function and skip Cloudformation after first deploy is finished
```
npm run pre-deploy
serverless deploy function -f <function name> --stage dev
```
