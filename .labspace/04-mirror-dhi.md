# Mirror DHI images

FIXME - do we still want the custom image with `curl`?

## First thing to get started, please provide your Docker org name

::variableDefinition[org]{prompt="What is your Docker Organization?"}

## Mirror a DHI Python image repo

Go to the [Python DHI page](https://hub.docker.com/orgs/$$org$$/hardened-images/catalog/dhi/python) and click on the `Mirror to repository` button.

In the opened pop-up set the name of the destination repository to `dhi-python`.

Click on Mirror. In a few minutes you'll see all available Python DHI tags in your `dhi-python` repository in the Docker Hub. Mirrored repositories work like any other repository in your Docker Hub organization.

## Mirror a DHI Postgres image repo

Go to the [Postgres DHI page](https://hub.docker.com/orgs/$$org$$/hardened-images/catalog/dhi/postgres) and click on the `Mirror to repository` button.

In the opened pop-up set the name of the destination repository to `dhi-postgres`.

Click on Mirror. In a few minutes you'll see all available Postgres DHI tags in your `dhi-postgres` repository in the Docker Hub. Mirrored repositories work like any other repository in your Docker Hub organization.

## Login with docker

In order to use Docker Scout to analyze the image during this lab, you will need to be logged in. Make sure that you are logged in with Docker:
```bash
docker login
```

You should see the following message:
```bash no-run-button no-copy-button
Login Succeeded
```

If not, follow the instructions to complete login.