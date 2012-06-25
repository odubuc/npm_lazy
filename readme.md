# npm_lazy: a lazy local cache for NPM

Use case: you have a bunch of servers that pull in dependencies as part of your deploy script from NPM.

NPM is slow and can cause errors if you have many servers that access it at the same time.

This server caches the NPM packages locally for local deployment.

## Key points

- Lazy caching: no need to replicate or install CouchDB. npm_lazy packages and metadata. Since caching is lazy, you don't need to explicitly manage it. Just point npm on your deployment to the local npm_lazy server and it'll do the right thing (see caching logic below).
- Local cache for tarfiles: The response URLs for NPM packages are rewritten on the fly to refer to npm_lazy.
- Data is stored as files: everything gets put under ./db/ as JSON and tar files. No database to install or manage.

## Caching logic

When a resource is requested:

- Anything that we don't have locally gets fetched from registry.npmjs.org on demand.
- Metadata is updated when the resource is requested the first time after a restart, and if the resource is requested an hour later (which is the max age for package metadata).

## Using npm_lazy

Start the server (node server.js), change config.js to set the port and external URL for npm_lazy.

Point npm to npm_lazy:

     npm config set registry http://localhost:8080/


