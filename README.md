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




