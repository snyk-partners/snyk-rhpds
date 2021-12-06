# Pre-requisites!
## Please complete these prior to the workshop! 

In this workshop, you will use of the following tools. Create free accounts for:
- Snyk
- Quay.io

You'll also need access to a few developer tools.
- Any IDE of your choosing (we recommend VS Code)
- Docker
- Snyk CLI

The Snyk Kubernetes Monitor, used in this workshop, is a paid Snyk component. You can request access to an environment for this workshop by reaching out to the DDC Conference support channel on Slack and tagging Remko De Knikker or Oliver Rodriguez.

### Set up your cluster to run the workshop
To perform this workshop on your environment you'll need to set up your cluster.
1. Install the Snyk Kubernetes Monitor using the instructions to install it from [OperatorHub](https://docs.snyk.io/snyk-container/image-scanning-library/kubernetes-workload-and-image-scanning/install-the-snyk-controller-with-openshift-4-and-operatorhub) or using [Helm](https://docs.snyk.io/products/snyk-container/image-scanning-library/kubernetes-workload-and-image-scanning/install-the-snyk-controller-with-helm).

2. Deploy the vulnerable demo application from the [goof-rhpds repo](https://github.com/snyk-partners/goof-rhpds) using oc.

```sh
oc create -f manifests
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

> This workshop guide uses Docker to build and push container images. If you prefer buildah and skopeo install those instead but we recommend proficiency with those tools as workshop staff might not be able to support you. 

2. Verify Docker installed correctly.

```sh
docker --version
```

### Create a Quay.io Account and Authenticate with your Docker client

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
