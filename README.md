# Snyk on OpenShift for RHPDS 
This workshop demonstrates how to use the Snyk Operator to identify security and configuration risks in an application deployed into OpenShift. We’ll guide you through deploying the Goof application and Snyk Operator into OpenShift, importing the deployment and reviewing the scan results in the Snyk UI, then remediating a vulnerability using Snyk’s upgrade guidance.

There are two demo paths in this workshop, a long and a short path. 
- The short path covers importing OpenShift workloads into Snyk and viewing results.
- The long path covers vulnerability remediation in addition to the short path. 

## Step 0: Prepare for the long demo
To fix vulnerabilities, you need the code for the Goof app and to build a container to deploy into OpenShift. Follow these steps to create GitHub and Quay repos to use the rest of the workshop. 
### Create a GitHub Repo
1. Generate the GitHub Repo for the Goof application from the [RedHat-Academy template](https://github.com/snyk-partners/redhat-academy).
### Create a Quay Repo
1. Sign into Quay, and create a Repository to host the Goof application’s containers
2. Configure Quay build triggers to build when code is pushed to the develop branch.

With GitHub and Quay ready, you’re ready to continue!

## Step 1: Deploy Goof into OpenShift
Follow these steps to deploy Goof into OpenShift from the developer perspective. 

1. Click “Add” and select the YAML option.
2. Create the deployment by using the YAML below and replacing the indicated fields. If demonstrating remediation, replace the Quay Repo with the one created in Step 0.

```YAML 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: username-goof #Replace username with your Lab username
spec:
  replicas: 1
  selector:
    matchLabels:
      app: goof
      tier: frontend
  template:
    metadata:
      labels:
        app: goof
        tier: frontend
    spec:
      containers:
        - name: goof
          image: quay.io/snyk-demo/goof:PROD #Replace with your Quay Repo if showing remediation
          resources:
            requests:
              cpu: 100m
              memory: 100Mi
          ports:
            - containerPort: 3001
            - containerPort: 9229
          env:
            - name: DOCKER
              value: "1"
```

3. Create the Service by using the YAML below and replacing the indicated fields.

```YAML
apiVersion: v1
kind: Service
metadata:
  name: username-goof #replace username with your Lab username
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3001
    name: "http"
  - protocol: TCP
    port: 9229
    targetPort: 9229
    name: "debug"
  selector:
    app: goof
    tier: frontend
```

4. Return to the Topology view in the Developer perspective and wait for Goof to launch. 
- Goof is externally exposed using a route. Navigate to Networking > Routes > Goof to see the app and start interacting with it. 

You successfully deployed Goof to OpenShift! It looks harmless enough, doesn’t it? Next, you’ll deploy the Snyk Operator to scan Goof for risks it might introduce into your environment. 

## Step 2: Deploy Snyk Operator 
The Snyk Operator integrates with OpenShift to identify vulnerabilities and misconfigurations that might make the workload less secure. Follow these steps to deploy it from OperatorHub.

> Note: Normally you create a namespace and secrets for the Snyk Integration ID and registry credentials as shown in the [Snyk Operator Installation Docs](https://support.snyk.io/hc/en-us/articles/360006548317-Install-the-Snyk-controller-with-OpenShift-4-and-OperatorHub). RHPDS created these for you. 

1. Navigate to OperatorHub and search for Snyk. Install the Snyk Operator.
2. Verify the installation succeeded by navigating to Installed Operators.
3. From the Subscription tab, create an Operator Subscription for the Snyk Controller.
    - Leave all defaults enabled to deploy Snyk with cluster scope. Note it is possible to deploy with namespace scope if you only want to scan a specific namespace.
4. In the Operator details, create an instance of the Snyk Monitor custom resource.
5. Once created, ensure it deployed correctly from the OpenShift developer perspective.

## Step 3: Scan the Goof Deployment
Follow these steps to import the Goof deployment into Snyk and review the results. 

> Note: you must have access to the Red Hat NFR Snyk instance to complete these steps.

1. Sign into Snyk, navigate to the Projects page, click Add Projects, and select Kubernetes.
2. In the import screen, find the Goof workloads, then click Add selected workloads.
3. Return to the projects tab. Wait for the import to complete then click the link for the project.
4. Review the vulnerability cards, ordered by Snyk’s Priority Score. Each card shows:
    - The direct and/or indirect dependency that introduced the vulnerability,
    - Details on the path, possible remediation, and the vulnerable function.
    - More information from the Snyk Intel DB and references to external disclosures.

> For more information around interpreting results, visit [Snyk Priority Score and Kubernetes](). 

This is the end of the short demo. If you performed Step 0, continue to Step 4 to show how to start addressing the vulnerabilities and issues in Goof. 

## Step 4: Remediating Vulnerabilities
If you performed Step 0, Quay re-builds the container for Goof each time code is pushed to the GitHub repo. Here’s a visual representation of the current delivery pipeline:

TODO: Insert Pipeline Image
<< Pipeline Image >>
Goof Repo (GitHub) -> Quay (via Build Triggers) -> OpenShift
<< /Pipeline Image >>

These steps demonstrate one way to fix vulnerabilities with Snyk. You’ll integrate Snyk into the GitHub repository and open Pull Requests to upgrade vulnerable dependencies. The delivery pipeline will update the running Deployment.

1. In Snyk, configure your GitHub account by navigating to Integrations -> GitHub.
2. Import the Goof repo by going to Projects -> Add Projects -> GitHub.
- Select the goof repository you generated from the template in Step 0. 
3. After the import completes, review the files Snyk scanned. Notice the risks in the repository are broken down by the components of Goof that make up the running Deployment:
    - The `package.json` file shows risks introduced by open source dependencies,
    - The `Dockerfile` shows risks introduced by the container base image used,
    - The `yaml` projects show configuration risks in the deployment manifest files.
4. Navigate into the `Dockerfile` project and scroll down to the list of vulnerabilities. Notice these are the same vulnerabilities shown in the OpenShift deployment.
5. Review base image upgrade guidance in the top of the project page. Base image recommendations are grouped by how likely they are to be compatible with Goof:
    - Minor upgrades are the most likely to be compatible with little work,
    - Major upgrades can introduce breaking changes depending on image usage,
    - Alternative architecture images are shown for more technical users to investigate.
6. For one of the suggested base images, click the button to “Open a Fix PR”. 
7. Review the PR in GitHub. Notice the CI job running as part of the PR checks. This simulates unit, functional, and regressions tests that might run as part of this PR. 
8. Once all checks complete, merge the PR. This triggers the Quay build.
9. In Quay, wait until the build completes successfully.
10. Return to the OpenShift developer perspective. Bounce the deployment to force Goof to download the latest container image from Quay.
11. Once the application is live, return to Snyk. Notice the vulnerability counts for the GitHub Repo and OpenShift project reflect the new counts.

This demonstrates how, by upgrading the base image in the Goof application to a more secure one, developers are able to address hundreds of vulnerabilities at once, giving them a better starting point to kick off more focused remediation efforts. Our applicaton is still not 100% secure, but it's better now than when you started.

# Conclusion

You reached the end of this workshop! This is one example of how Snyk guides developers through remediating vulnerabilities by proactively kicking off Pull Requests with potential fixes. There is much more we didn’t show, from our CI/CD integrations, CLI, API, and integrations into the Quay Registry. If you’re interested in other Snyk capabilities, check out the other [Red Hat Workshops in the Snyk Academy](https://solutions.snyk.io/partner-workshops/red-hat)!

