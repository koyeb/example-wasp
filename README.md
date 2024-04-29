<div align="center">
  <a href="https://koyeb.com">
    <img src="https://www.koyeb.com/static/images/icons/koyeb.svg" alt="Logo" width="80" height="80">
  </a>
  <h3 align="center">Koyeb Serverless Platform</h3>
  <p align="center">
    Deploy a Wasp application on Koyeb
    <br />
    <a href="https://koyeb.com">Learn more about Koyeb</a>
    ·
    <a href="https://koyeb.com/docs">Explore the documentation</a>
    ·
    <a href="https://koyeb.com/tutorials">Discover our tutorials</a>
  </p>
</div>


## About Koyeb and the Wasp example application

Koyeb is a developer-friendly serverless platform to deploy apps globally. No-ops, servers, or infrastructure management.  This repository contains a Wasp application you can deploy on the Koyeb serverless platform for testing.

This example application is designed to show how a Wasp application can be built and deployed on Koyeb.  The application is a simple todo list based on a [Wasp project template](https://wasp-lang.dev/docs/project/starter-templates#todo-app-w-typescript).  It consists of a server backend and a static web frontend which are deployed separately.  The application is backed by a PostgreSQL database to store user data and todo list items.

## Getting Started

Follow the steps below to deploy and run the Wasp application on your Koyeb account.

### Requirements

* A [Koyeb account](https://app.koyeb.com/auth/signup) to build, deploy, and run this application.
* An externally accessible PostgreSQL database.  You can spin up a free [PostgreSQL database with Koyeb](https://app.koyeb.com/database-services) to use for this application.

### Deploy using the Koyeb button

The fastest way to deploy the Wasp application is to click the [Deploy to Koyeb](https://www.koyeb.com/docs/build-and-deploy/deploy-to-koyeb-button) buttons below.

First, deploy the Wasp server backend with this button:

[![Deploy to Koyeb](https://www.koyeb.com/static/images/deploy/button.svg)](https://app.koyeb.com/deploy?name=example-wasp-server&type=git&repository=koyeb%2Fexample-wasp&branch=main&builder=dockerfile&target=server-production&instance_type=nano&env%5BDATABASE_URL%5D=CHANGE_ME&env%5BJWT_SECRET%5D=CHANGE_ME&env%5BWASP_SERVER_URL%5D=CHANGE_ME&env%5BWASP_WEB_CLIENT_URL%5D=CHANGE_ME&ports=8000%3Bhttp%3B%2Fapi)

Open the **Environment variables** section and set the appropriate values for your application:

- `DATABASE_URL`: This is your PostgreSQL database's connection string with `?ssl_mode=require` appended to the end.
- `WASP_SERVER_URL`: This is the URL where the server backend will be deployed, including the `/api` path.  Open the **App and Service names** section to find or set the App name. Set this using the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app/api`.
- `WASP_WEB_CLIENT_URL`: This is the URL where the web frontend will be deployed.  We will deploy the web frontend to the web root (`/`), so fill this in using the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app`.
- `JWT_SECRET`: This is the JSON web token (JWT) used during authentication. Set this to a randomly generated string.  You can generate an appropriate string by typing `openssl rand -base64 24` in your terminal.

Next, deploy the Wasp web frontend using this button:

[![Deploy to Koyeb](https://www.koyeb.com/static/images/deploy/button.svg)](https://app.koyeb.com/deploy?name=example-wasp-web-app&type=git&repository=koyeb%2Fexample-wasp&branch=main&builder=dockerfile&target=web-app-production&instance_type=nano&env%5BREACT_APP_API_URL%5D=CHANGE_ME&ports=8000%3Bhttp%3B%2F)

Open the **Environment variables** section and set the following variable:

- `REACT_APP_API_URL`: Set this to the URL for your backend service.  This should be the same value you used for `WASP_SERVER_URL` in the backend configuration.  It will have the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app/api`.

Open the **App and Service names** section to deploy this to the same App domain as the backend Service.

_To modify this application example, you will need to fork this repository. Checkout the [fork and deploy](#fork-and-deploy-to-koyeb) instructions._

## Fork and deploy to Koyeb

If you want to customize and enhance this application, you need to fork this repository.

If you used the **Deploy to Koyeb** button, you can simply link your service to your forked repository to be able to push changes.  Alternatively, you can manually create the application as described below.

On the [Koyeb Control Panel](//app.koyeb.com/apps), on the **Overview** tab, click the **Create Web Service** button to begin.

To deploy the server backend, use the following procedure:

1. Select **GitHub** as the deployment method.
2. Select your Wasp project repository repository.
3. In the **Builder** section, select **Dockerfile**.  Click the **Override** toggle associated with **Target** and enter `server-production` in the field.
4. In the **App and Service names** section at the bottom, choose a name for your App and Service.  Your App name will be a component of the public URL for your Service, using the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app`.
5. In the **Exposed ports** section, change the **Path** for the configured port from `/` to `/api`.
6. In the **Environment variables** section, click **Bulk edit** to enter multiple environment variables at once. In the text box that appears, paste the following:

   ```
   DATABASE_URL=
   WASP_SERVER_URL=
   WASP_WEB_CLIENT_URL=
   JWT_SECRET=
   ```

   Set the variable values to reference your own information as follows:

   - `DATABASE_URL`: This is the PostgreSQL database's connection string with `?ssl_mode=require` appended to the end.
   - `WASP_SERVER_URL`: This is the URL where the server backend will be deployed, including the `/api` path.  Set it using the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app/api`.
   - `WASP_WEB_CLIENT_URL`: This is the URL where the web frontend will be deployed.  We will deploy the web frontend to the web root (`/`), so fill this in using the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app`.
   - `JWT_SECRET`: This is the JSON web token (JWT) used during authentication. Set this to a randomly generated string.  You can generate an appropriate string by typing `openssl rand -base64 24` in your terminal.

7. Click **Deploy**.

To deploy the web frontend, use the following procedure: 

On the **Apps** tab of the [Koyeb control panel](https://app.koyeb.com/apps), click on the Wasp server backend App.  In the upper-right corner, click **Create Service** to create a new Service within the same application domain.

1. Select **GitHub** as the deployment method.
2. Select your Wasp project repository repository.
3. In the **Builder** section, select **Dockerfile**.  Click the **Override** toggle associated with **Target** and enter `web-app-production` in the field.
4. In the **Environment variables** section, click **Add variable** to add the following variable:

    * `REACT_APP_API_URL`: Set this to the URL for your backend service.  This should be the same value you used for `WASP_SERVER_URL` in the backend configuration.  It will have the following format: `https://<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app/api`.

5. Click **Deploy**.

## Contributing

If you have any questions, ideas or suggestions regarding this application sample, feel free to open an [issue](https://github.com/koyeb/example-wasp/issues) or fork this repository and open a [pull request](https://github.com/koyeb/example-wasp/pulls).

## Contact

[Koyeb](https://www.koyeb.com) - [@gokoyeb](https://twitter.com/gokoyeb) - [Slack](http://slack.koyeb.com/)
