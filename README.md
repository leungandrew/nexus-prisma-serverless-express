# Setup

```
yarn install
yarn build
```

If you already have your own Postgres DB spun up, then add a
`prisma/.env` with your `DATABASE_URL=postgres://yadayada` and run:

```
yarn -s prisma migrate up --experimental
yarn -s prisma generate
yarn -s ts-node prisma/seed.ts
```

Run the server locally:

```
DATABASE_URL=<your-postgres-db> yarn start
```

Check that it works locally, then deploy to serverless:

```
yarn deploy
```

Excerpt of `yarn deploy`:

```
    "deploy:export": "echo \"module.exports = nexus_1.server.express\" >> .nexus/build/index.js",
    "deploy:rename": "mv .nexus/build/index.js .nexus/build/app.js",
    "deploy": "yarn deploy:export && yarn deploy:rename && serverless deploy",
```

## Notes
Serverless Express component expects an app.js that exports the express
server, which is what the `deploy:export` script does.

Secondly, we rename the `index.js` to `app.js` so Serverless picks up
the entrypoint correctly.

This should lead you to a playground that works with the schema well
defined.  But you will notice that when making a query, the following
error appears:

```
{
  "error": {
    "errors": [
      {
        "message": "\nInvalid `prisma.world.findMany()` invocation in\n/var/task/api/graphql.js:46:37\n\n\n  EROFS: read-only file system, chmod '/var/task/node_modules/.prisma/client/query-engine-rhel-openssl-1.0.x'",
        "locations": [
          {
            "line": 2,
            "column": 3
          }
        ],
        "path": [
          "worlds"
        ]
      }
    ],
    "data": {
      "worlds": null
    }
  }
}
```

Second note: I realize that we haven't exposed the `DATABASE_URL` to
Serverless yet at this point.  This is as simple as adding:
```
inputs:
  env:
    DATABASE_URL: "postgres://yadayada"
```
to the `serverless.yml`

but because we can't get past the above error, we won't see the error
where Prisma can't find the database.
