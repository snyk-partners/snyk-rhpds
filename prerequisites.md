# Pre-requisites!
## Please complete these prior to the workshop! 

In this workshop, you will use of the following tools. Create free accounts for:
- Snyk
- Quay.io
- GitHub

You'll also need access to a few developer tools.
- Any IDE (we use VS Code)
- OpenShift CLI (oc)

You'll receive an e-mail the day before the workshop adding you to the IBM DDC Conference organization in Snyk. Keep an eye out! 

### Generate the GitHub Repo from the Template
1. Navigate to the [goof-rhpds repo](https://github.com/snyk-partners/goof-rhpds) on GitHub, then click "Use this Template" to generate it in your account.

![Repo Template](images/github-template.png)

2. Clone the Repo to your workstation. Be sure to replace your GitHub username in the command or set the GH_USER environment variable.

```sh
git clone https://github.com/$GH_USER/snyk-rhpds
```

### Create a Snyk Account

1. [Create a Snyk Account](https://app.snyk.io/login?utm_campaign=RHPDS&utm_medium=Partner&utm_source=Red-Hat) for yourself, or use your existing Snyk account. 

2. Send a request to the #channel Slack channel and tag Remko to request access to the Snyk Org for this Workshop. 

### Create a Quay Account and Repo

1. Create an account or sign in to [Quay.io](https://quay.io).

### Install the OpenShift CLI
#TODO: Finish the Cluster Request step 2 in case user needs to request access. 
1. Install oc on your workstation by following the [installation steps](https://docs.openshift.com/container-platform/4.9/cli_reference/openshift_cli/getting-started-cli.html).
2. oc login
