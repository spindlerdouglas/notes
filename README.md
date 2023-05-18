A few notes on troubleshooting based on experience acquired from previous development projects and exercises.

## Javascript and Typescript
- build script may look like: "build": "rimraf dist && tsc"
- in VS Code, sometimes the behavior is quite erratic and certain dependencies are not found. I found it useful sometimes to simply delete the complete _dist_ and _node_modules_ folders, rebuild the project and reinstall all dependencies again from scratch

### Postgres and migrations
- package.json must trigger the database table creation via the migration files before starting the app, via script "prestart"
- db-migrate doesn't work with typescript directly. Also, a .js file cannot access a .ts file directly. As such, define the migration as a regular .js file and make the imports that require typescript files point to the _dist_ folder, which contains the files transpiled to regular javascript. 
- use a config.ts file for specifying the postgres connection string for local tests. For productive scenarios, retrieve the environment variable via process.env
  - in Kubernetes this comes from the environment variable as specified in the deployments in the .yaml files.
- the connection string needs to be passed to the pg.Pool
- make sure that npm run db:start is executed before starting the app when testing locally

## Docker
- Dockerfile must ensure all relevant folders are copied 
- build the docker image and test locally via docker run to check for errors
- in Docker desktop, inspect the image and check if all relevant files have been copied to the expected directories
- if everything is correct then build and push to the repository

## Kubernetes
- use _kubectl get pods_ and _kubectl logs <pod>_ to check for errors in k8s
