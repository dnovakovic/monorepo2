# Monorepo2

## Context
This is an example of monorepo with several packages located under /packages folder. What packages are about is of no importance: what is of importance is that packages represent layered structure of the application. There are for packages present:
* ui
* types
* data
* ui

Important to note is that packages ui and types are independed (root, base packages). Package date depends on ui, types. Package ui depens on all other packages (ui, types, data). Hence, when building, lerna will build root packages first moving on upwards through dependency graph. 

All packages will start from version 0.0.1.

## How to build & run application.
As previously stated how application is running is not primary concern here. One can build and run with following series of yarn scripts. Please that these scripts rely on lerna (see package.json of top package).

* yarn
* yarn lerna link
* yarn clean
* yarn build
* yarn dev 

After yarn dev console should output
```
Listening on http://localhost:1234
```
## NPM registry
In order to publish (and reuse) packages in this monorepo same should be published into npm registry. To play role of npm registry we can make use of verdaccio (private npm proxy registry).

Simply run dockerized version of verdaccio. Note: this version of ```docker run``` will pass customized configuration file for verdaccio in order to increase maximum size of package that can be uploaded to registry
```
docker run -it --name verdaccio -p 4873:4873 -v $(pwd)/conf/verdaccio:/verdaccio/conf verdaccio/verdaccio
```

After this registry should be available on following url
```
http://localhost:4873/
```

## Publishing with lerna
In order to see list of packages available for publishing use
```
yarn lerna changed
```

In order to publish one must first create user and login to npm registry (in this case, verdaccio)
```
npm adduser --registry http://localhost:4873/
```

Initially packages all packages can be published with
```
yarn lerna publish --registry=http://localhost:4873/
```

This form of ```yarn lerna publish``` will present menu where one can pick exactly how version of the packages should be bumped (increased). Packages will then appear in the npm registry under new version number. 

## Flow of package development
In everyday activities new code will be checked in some of the packages. Given what package is updated, dependent packages will also be listed as changed. 
```
yarn lerna changed
```

Version of changed packages can be bumped and packages published into repository with the same lerna publish command
```
yarn lerna publish --registry=http://localhost:4873/
```

Or if you'd like to avoid answering all confirmation questions about what and if to publish and simply publish patched version of the package
```
yarn lerna publish patch --registry=http://localhost:4873/ --yes
```
