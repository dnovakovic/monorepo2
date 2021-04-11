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

Simply run dockerized version of verdaccio. 
```
docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio
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

Initially packages all packages can be published with
```
yarn lerna publish --registry=http://localhost:4873/
```
This will present menu where user can pick exactly how version of the packages should be bumped (increased). Packages will then appear in the repository under new version number. 
