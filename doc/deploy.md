# How to Deploy bcsinfo

bcsinfo is splitted into 3 repos:
* [https://github.com/cryptofugu/bcsinfo](https://github.com/cryptofugu/bcsinfo)
* [https://github.com/cryptofugu/bcsinfo-api](https://github.com/cryptofugu/bcsinfo-api)
* [https://github.com/cryptofugu/bcsinfo-ui](https://github.com/cryptofugu/bcsinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy bcs core
1. `git clone --recursive https://github.com/cryptofugu/bcschain-core.git --branch=bcsinfo`
2. Follow the instructions of [https://github.com/cryptofugu/bcschain-core/blob/master/README.md#building-bcs-core](https://github.com/cryptofugu/bcschain-core/blob/master/README.md#building-bcs-core) to build bcs
3. Run `bcsd` with `-logevents=1` enabled

## Deploy bcsinfo
1. `git clone https://github.com/cryptofugu/bcsinfo.git`
2. `cd bcsinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `bcsinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `bcsinfo` under a process manager (like `pm2`), to restart the process when `bcsinfo` crashes.

## Deploy bcsinfo-api
1. `git clone https://github.com/cryptofugu/bcsinfo-api.git`
2. `cd bcsinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy bcsinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/cryptofugu/bcsinfo-ui.git`
2. `cd bcsinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "BCSINFO_API_BASE_CLIENT=/api/ BCSINFO_API_BASE_SERVER=http://localhost:7001/ BCSINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `bcsinfo-ui` on port 3000
4. Edit nuxt.config.js if needed. For example, add your keys for google-analytics and/or yandex-metrika modules.
5. `npm run build`
6. `npm start`
