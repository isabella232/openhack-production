# The Urlist - Frontend

## Table Of Contents

<!-- toc -->

- [Dependencies](#dependencies)
- [Frontend configurations](#frontend-configurations)
- [Build and run the frontend locally](#build-and-run-the-frontend-locally)
- [Debugging the application](#debugging-the-application)
- [Docker local development](#docker-local-development)
- [UI Views](#ui-views)
  <!-- tocstop -->

---

## Dependencies

The frontend for this project is built with the following libraries and frameworks:

- [Node 10+](https://nodejs.org/en/) / [NPM 6+](https://www.npmjs.com/get-npm)
- [TypeScript](https://www.typescriptlang.org/)
- [Vue.js](https://github.com/vuejs/vue) / [Vue CLI](https://github.com/vuejs/vue-cli)
- [Vuelidate](https://github.com/vuelidate/vuelidate)
- [Axios](https://github.com/axios/axios)

### Other useful tools

- [Visual Studio Code](https://code.visualstudio.com/?WT.mc_id=theurlist-github-buhollan)
- [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur&WT.mc_id=theurlist-github-buhollan)
- [VS Code Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome&WT.mc_id=theurlist-github-buhollan)
- [Vue VS Code Extension Pack](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-extensionpack&WT.mc_id=theurlist-github-buhollan)
- [Vue browser devtools](https://github.com/vuejs/vue-devtools)

---

## Frontend configurations

There are a few configurations needed for the frontend app to run, those are passed as [VUE environment configs](https://cli.vuejs.org/guide/mode-and-env.html)

- Follow the [backend README guide](../api/README.md) on how to run the Backend locally.

- Once the Backend has started, you will want to get the port the Backend uses _(local port may change depending upon the IDE being used)_.

- You can find the values for the `OpenID` configuration from Azure DevOps variable groups (Pipelines -> Library -> Variable groups).

  > You can learn more about OpenID Connect (OIDC) [here](../docs/AzureADB2C.md).

- Copy the sample `.env` and fill in the missing values

  ```bash
  cp .env.sample.local .env.development.local
  ```

---

## Build and run the frontend locally

If you prefer to use Docker, we have provided [setup instructions below](#docker-local-development)

### Setup NPM for the frontend

```bash
# Ensure you are in the frontend folder
cd frontend

# Install Vue CLI globally
npm install -g @vue/cli

# Install npm packages for frontend project
# IMPORTANT: you'll need 'npm 6+'
npm ci
```

### Serve development build

```bash
# Serves a local application on port '8080'
npm run serve
```

![localhost serve](docs/Images/localhost_serve.png)

To see other view of the app, take a look in [UI Views](#ui-views)

### Create production optimized build

```bash
npm run build
```

> This creates a dist folder under frontend

### Lints and fixes files

```bash
npm run lint
```

---

## Debugging the application

- Follow the instructions in the [Build and run the frontend locally](#build-and-run-the-frontend-locally) to start application

- In Chrome, open the Chrome Developer Tools
- Select `Source` then expand `webpack` then expand the `.` folder then expand `src` and find the TypeScript file that you would like to debug and `double-click` the line in the TypeScript file that you are interested in. See the screenshot below as an example:

  ![localhost serve](docs/Images/localhost_debugging.png)

---

## Docker local development

By default the front-end will be running in `development mode`, consequently make sure you setup the environment files as described [above](#frontend-configurations).

```bash
docker build -t linkylink-fe .
docker run -it --rm -p 8080:8080 --name frontend linkylink-fe
```

Alternatively, you can pass the environment variables to override any settings from the `.env.[mode]` files

```bash
docker build -t linkylink-fe .
docker run -it --rm -p 8080:8080 --name frontend \
-e VUE_APP_BACKEND="http://localhost:5000" \
-e VUE_APP_OIDC_AUTHORITY="https://<b2c login subdomain>.b2clogin.com/testprodoh.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_SignUp_SignIn" \
-e VUE_APP_OIDC_CLIENT_ID="<client id>" \
-e VUE_APP_OIDC_SCOPE="openid https://<b2c login subdomain>.onmicrosoft.com/api/UrlBundle.ReadWrite" \
-e "VUE_APP_APPINSIGHTS_INSTRUMENTATIONKEY=[Azure Application Insights Instrumentation Key]" \
linkylink-fe
```

---

## UI Views

<details>
<summary>Expand to see the different views of the app</summary>

### Homepage view

This is the first page you would see when visiting the frontend. You can start the process of creating a link-bundles from here.

![Homepage picture](docs/Images/Homepage.png)

### Edit view

This is the page where you would create your lists.

![Edit picture](docs/Images/Edit_page.png)

### User view

This page would list all list-bundles created for a signed-in user.

![Manage lists picture](docs/Images/Manage_lists_page.png)

### List view

This page is used to view the list-bundle for a given vanity URL.

![View list picture](docs/Images/View_page.png)

</details>
