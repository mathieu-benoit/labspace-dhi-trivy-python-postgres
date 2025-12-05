# Mirror DHI images
FIXME

::variableDefinition[org]{prompt="What is your Docker Organization?"}

Go to the $$org$$ Docker Hub organization

    https://hub.docker.com/orgs/$$org$$/repositories

Click Hardened Images and Catalog.

Type python in the search bar to find the python hardended image.

Select the python image.

Review the Guide for implementing the image.

Go to images/tags.

Search by `3.13`.

Select Python 3.13.x (dev) - the tag is `3.13-alpine3.22-dev`.

Review packages, specifications, vulnerabilities, and attestations for the hardened image.

Mirror the python repository to your organziation with the name dhi-python(already done).

Select the `dhi-python` repository in your organization.

Select the `customize image` button.

Select `Python 3.13.x (dev) alpine 3.22`.

Select to add the `curl` package.

Review other customizations available.

Start customization build with tag `curl` (already done).

Mirror the postgres DHI image to dhi-postgres (already done).