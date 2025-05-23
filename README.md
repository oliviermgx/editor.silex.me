# Silex Platform by Silex Labs

This is also a good example on how to customize Silex. And it has a Dockerfile for easy deployment.

It has convenient environment variables and many plugins enabled by default, and it is ready to be deployed on CapRover.

Check [the docs for how to run it with Docker or nodejs](https://docs.silex.me/en/dev/run).

> **Note:** There is no npm package for this repo, it is meant to be run as a standalone instance that you can configure with env vars and config files, not a CLI tool. Use `@silexlabs/silex` for the CLI tool and use the config files of this repo to get the same result.

## Why this package?

This package is a good example of how to customize Silex, and it is a good starting point to create your own instance of Silex.

This repo holds the code behind these Silex instances:

* [Silex instance hosted for free by Silex Labs foundation](https://v3.silex.me)
* [The legacy Silex instance hosted for free by Silex Labs foundation](https://editor.silex.me)
* [Silex one click app on CapRover](https://github.com/caprover/one-click-apps/blob/master/public/v4/apps/silex-platform.yml)

### [silex-lib repository](https://github.com/silexlabs/silex-lib/) VS this Silex Platform repository

| Feature | Main Silex repository | Silex Platform repository |
|---------|-----------------------|---------------------------|
| Customizable | Yes | Yes |
| Docker image | Yes | Yes |
| CapRover one click app | No | Yes |
| Automatic deployment to CapRover | No | Yes |
| Env vars to enable or disable cloud services | No | Yes |
| Env vars to customize the instance | Yes ([doc](https://docs.silex.me/en/dev/options)) | Yes ([doc](#environment-variables)) |

### Plugins enabled by default in this repo, not in the main Silex repo:

| Plugin | Main Silex repository | Silex Platform repository |
|--------|-----------------------|---------------------------|
| Silex CMS plugin | No | Yes |
| Dashboard plugin for multi-site edition | No | Yes |
| Onboarding plugin (send users a welcome email) | No | Yes |
| FTP storage | No | Yes |
| FTP hosting | No | Yes |
| Download hosting | No | Yes |
| Gitlab storage | No | Yes |
| Gitlab hosting | No | Yes |

## Features

This code adds features to the editor specific to our instance (in `index.js` and `index.pug`):

* [x] Multi-site with the Dashboard plugin
* [x] Automatic deployment to [CapRover](https://caprover.com/) (see the captain-definition file and the file `.github/workflows/caprover.yml`)
* [x] Onboarding: Send an email with [brevo the 1st time we see a user](https://brevo.com/) + use Silex notification system to guide users through the first steps
* [x] Enable or disable cloud services and hosting providers with env vars
* [x] [Docker image](https://hub.docker.com/r/silexlabs/silex-platform)

## Run the code

### With Docker

```bash
docker run -p 8080:8080 silexlabs/silex-platform
```

Add environment variables to customize the instance - here is an example with FTP storage and FTP hosting:

```bash
docker run -p 8080:8080 -e STORAGE_CONNECTORS=ftp -e HOSTING_CONNECTORS=ftp,download silexlabs/silex-platform
```

More info:

* [Docs](https://docs.silex.me/en/dev/run)
* [Env vars are described below](#environment-variables)
* [Docker Hub](https://hub.docker.com/r/silexlabs/silex-platform)
* [Dockerfile](Dockerfile)
* [What is storage and hosting](https://docs.silex.me/en/dev/connect)

## Contributing

To run the code locally:

```bash
npm install
npm start
```

Or for development:

```bash
npm install
npm run dev
```

## Environment variables

You can set the following environment variables to customize the instance:

| Name | Description | Type | Default value |
|------|-------------| ---- |---------------|
| `STORAGE_CONNECTORS` | List of storage connectors to enable | `ftp` or `gitlab` or `gitlab2` or `fs` | `ftp` |
| `HOSTING_CONNECTORS` | List of hosting connectors to enable | `ftp` or `gitlab` or `gitlab2` or `fs` or `download` | `ftp,download` |
| `SILEX_FS_ROOT` | Root folder for the file system storage | string | current directory + `/silex/storage/` |
| `SILEX_FS_HOSTING_ROOT` | Root folder for the file system hosting | string | current directory + `/silex/hosting/` |
| `FTP_STORAGE_PATH` | Path to the FTP storage | string | `` |
| `FTP_HOSTING_PATH` | Path to the FTP hosting | string | `` |
| `GITLAB_DISPLAY_NAME` | Display name for the Gitlab storage | string | `Gitlab` |
| `GITLAB_DOMAIN` | Domain of the Gitlab server, e.g `https://gitlab.com` | string | required with gitlab connector |
| `GITLAB_CLIENT_ID` | Client ID for the Gitlab OAuth | string | required with gitlab connector |
| `GITLAB_CLIENT_SECRET` | Client secret for the Gitlab OAuth | string | required with gitlab connector |
| `GITLAB2_DISPLAY_NAME` | Display name for the 2nd Gitlab storage | string | `Gitlab2` |
| `GITLAB2_DOMAIN` | Domain of the 2nd Gitlab server, e.g `https://gitlab.com` | string | required |
| `GITLAB2_CLIENT_ID` | Client ID for the 2nd Gitlab OAuth | string | required |
| `GITLAB2_CLIENT_SECRET` | Client secret for the 2nd Gitlab OAuth | string | required |
| `BREVO_API_KEY` | The API key for the Brevo service (onboarding, optional) | string | - |
| `BREVO_API_URL` | The API URL for the Brevo service (onboarding, optional) | string | - |
| `BREVO_LIST_ID` | The list ID for the Brevo service (onboarding, optional) | string | - |

## For Your Gitlab App 

If you want to use Gitlab as a storage provider - i.e add `gitlab` to the `STORAGE_CONNECTORS` env var - you need to create a Gitlab app:

In gitlab:

- see https://docs.gitlab.com/ee/integration/oauth_provider.html#create-a-user-owned-application
- Your redirect url should be : `https://[yourdomain]/api/connector/login/callback`
- Check only the `api` scope

Then set the following env vars when you start Silex platform:

- `GITLAB_CLIENT_ID` with the `Application ID`
- `GITLAB_CLIENT_SECRET` with the `Secret`

## Support

[Please use the main Silex repository](https://github.com/silexlabs/Silex/) for issues, [documentation](https://docs.silex.me/), and [community support](https://community.silex.me/).
