# Nx Upgrade 16.0 to 16.1

A repo to reproduce the issue of migrating from Nx 16.0 to 16.1 were an
application has a main that points to a folder instead of a file.

## Background

With Go projects that are using workspaces the main property needs to point to a folder
instead of a file or else the compiler won't be able to compile the module.

## Reproduce

To reproduce the issue run the following commands:

```shell
pnpm nx migrate 16.1.4
pnpm install --no-frozen-lockfile
pnpm exec nx migrate --run-migrations --verbose
```

Observe the migrations fail and you can see the issue is processing the bootstrap file.

This may be due to [extract-standalone-config-from-bootstrap.ts#L132](https://github.com/nrwl/nx/blob/16.1.4/packages/angular/src/migrations/update-16-1-0/extract-standalone-config-from-bootstrap.ts#L132)
only checking if the main property has a value and not if the property is a path to a file and not a directoy.