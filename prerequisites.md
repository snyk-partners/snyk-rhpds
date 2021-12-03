# Pre-requisites!
## Please complete these prior to the workshop! 

In this workshop, you will use of the following tools. Create free accounts for:
- Snyk
- Quay.io
- GitHub

You'll also need access to a few developer tools.
- Any IDE (we use VS Code)
- Docker
- OpenShift CLI (oc)

You'll receive an e-mail the day before the workshop adding you to the OpenShift Workshop organization in Snyk. Keep an eye out! 

### Generate the GitHub Repo from the Template
1. Navigate to the [goof-rhpds repo](https://github.com/snyk-partners/goof-rhpds) on GitHub, then click "Use this Template" to generate it in your account.

![Repo Template](images/github-template.png)

2. Clone the Repo to your workstation. Be sure to replace your GitHub username in the command or set the GITHUB_USER environment variable.

```sh
git clone https://github.com/$GITHUB_USER/snyk-rhpds
```

### Create a Snyk Account and Install the Snyk CLI

1. [Create a Snyk Account](https://app.snyk.io/login?utm_campaign=RHPDS&utm_medium=Partner&utm_source=Red-Hat) for yourself, or use your existing Snyk account. 

2. Visit the Snyk documentation to [Install the Snyk CLI](https://docs.snyk.io/features/snyk-cli/install-the-snyk-cli) and install it on your system. 

3. After installing, verify your installation by running: 

```sh
snyk --version
```

### Install Docker on your workstation

You'll need docker tools to build and push container images. 

1. Visit [Get Docker](https://docs.docker.com/get-docker/) and install Docker on your system. 

> This workshop guide uses Docker to build and push container images. If you prefer buildah and skopeo we recommend proficiency with those tools as the workshop guide is written for docker and staff might not be able to support you. 

2. Verify Docker installed correctly.

```sh
docker --version
```

### Create a Quay Account and Repo

1. Create an account or sign in to [Quay.io](https://quay.io).

2. Once signed in, create a Repository on Quay.io. We recommend calling it `goof`.

![Create Repo](images/quay-repo.png)

3. Save your Quay Username as an Environment Variable to use it in future steps. 

> Your Quay Username is displayed in the upper right corner of Quay.io. 

![Quay Avatar](./images/quay-user-avatar.png)

```sh
QUAY_USER=<<your_quay_id>>
```

3. In this module you’ll push container images to Quay.io. Log in to Quay.io by running the following command:

```sh
docker login quay.io -u $QUAY_USER
```

Enter your password when prompted.

### Install the OpenShift CLI
TODO: Write this!
1. Install oc
2. oc login
