
# CIAM Core - NodeJs boilerplate

Remember to replace the tag "your_project" with the name of your project in the existing files.

## Requeriments
You need to have install it, as global, the next libraries:

```sh
$ npm install -g nodemon                # for hot reloading
$ npm install -g apidoc                 # for api documentation (more info at: http://apidocjs.com)
$ npm install -g babel-cli              # for babel transpile

```

## Commands

```sh
$ npm test                    # run tests with Jest
$ npm coverage                # run tests coverage
$ npm run build               # generate docs and transpile code
$ npm run dist                # generate distribution files
$ npm run start:dev           # starts local server with nodemon (es6) and hot reloading
$ npm docs                    # generated the API.md file using documentation
$ npm docker                  # create a new docker image into locally
$ npm run patch               # bump patch version and publish to npm e.g. 0.0.1
$ npm run minor               # bump minor version and publish to npm e.g. 0.1.0
$ npm run major               # bump major version and publish to npm e.g. 1.0.0

```



