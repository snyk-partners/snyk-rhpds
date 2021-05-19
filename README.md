# Snyk on OpenShift for RHPDS 
## About this Workshop
Welcome! This workshop demonstrates how to use [Snyk Container](https://snyk.io/product/container-vulnerability-management/) to identify security and configuration risks in a sample application deployed into OpenShift. 

The steps below guide you through:
1. Reviewing the security and configuration scan results in the Snyk UI,
2. Finding and applying a secure base image using Snyk’s upgrade guidance.

> Note: This workshop is intended to be used with the Red Hat Partner Demo System (RHPDS). For a non-RHPDS version, check out the [Red Hat Patterns in the Snyk Academy](https://solutions.snyk.io/partner-workshops/red-hat).

> Note: To complete this workshop you'll need access to the Snyk NFR instance for Red Hat. Contact [Dave Meurer](mailto:dmeurer@redhat.com) or [Tomas Gonzalez](mailto:tomas@snyk.io) to request access.

#TODO: #2 ADD UTMs? 

## Your demo environment
Your OpenShift environment includes a Project assigned to you. Complete the workshop in the Project assigned to your user. Your Project includes:

- A deployment of Snyk's intentionally vulnerable demo app, [Goof](https://github.com/snyk-partners/redhat-academy).  
- The Snyk Kubernetes Monitor, deployed by the Snyk Operator.

Goof is externally exposed using a Route. Navigate to Networking > Routes > Goof to interact with it.

 Note: The Snyk Monitor uses Secrets for the Integration ID and registry credentials as shown in the [Snyk Operator Installation Docs](https://support.snyk.io/hc/en-us/articles/360006548317-Install-the-Snyk-controller-with-OpenShift-4-and-OperatorHub). In this workshop, RHPDS created these for you. 

# Let's get started!
## Before you begin: Understand how the Snyk Monitor works
The Snyk Monitor integrates with OpenShift to test running workloads and identify security vulnerabilities and configuration risks that might make the workload less secure. 

It communicates with the OpenShift API to determine which workloads are running, scans them, and reports results back to Snyk. The following workloads can be scanned:
- Deployment
- DeploymentConfig
- ReplicaSets
- DaemonSets
- StatefulSets
- Jobs
- CronJobs
- ReplicationControllers
- Pods

To import workloads into Snyk, users can select workloads in the Snyk UI, or import them automatically using annotations. These options are as described in [Adding Kubernetes workloads for security scanning](https://support.snyk.io/hc/articles/360003947117#UUID-a0526554-0943-3363-6977-7a11f766ede2).

## Part 1: Reviewing the Goof Deployment Scan Results
The Goof Deployment has been imported using the annotation method described above. You can see the workload annotation by navigating to the Deployments. 

#TODO: #3 Add Image of Deployments in OCP Console

In this section, you'll review the scan results within the Snyk UI. 

1. Sign into Snyk, then navigate to the Projects tab. 

![Snyk Projects Tab](./images/snyk-projects-tab.png)

2. Locate the Project associated with your OpenShift Project. Each item is named according to its OpenShift metadata as follows: Project/kind/name.

![Imported Projects](./images/imported-projects.png)

3. Expand the project list to see a list of the images used by the workload. For workloads with multiple images, the top row aggregates the count of vulnerabilities across all images.

![Project List](./images/project-list.png)

4. Click the workload link to see details around the security posture of the workload configuration. For information on what we test for, visit [viewing project details and test results](https://support.snyk.io/hc/en-us/articles/360003916178-Viewing-project-details-and-test-results).

![Workload Configuration](./images/workload-config.png)

5. Return to the Projects Tab, now click the image name to view a list of its vulnerabilities. Scroll down to see the list of issues, ordered by Snyk's [Priority Score](https://support.snyk.io/hc/en-us/articles/360009884837). Each card represents a vulnerability in the image, and displays:
    - The issue type, and informative links to the [Snyk Intel DB](https://snyk.io/product/vulnerability-database/), CVE, and CWE
    - The direct and/or indirect dependency that introduced the vulnerability,
    - Details on the path and possible remediation,
    - If available, [relative importance](https://support.snyk.io/hc/en-us/articles/360013304357) from the Linux distribution's upstream tracker.

> Configuration information from the workload contributes to the Priority Score, based on the idea that a vulnerability in a workload that is poorly configured scores higher than the same vulnerability in a well configured one. For more information visit [Snyk Priority Score and Kubernetes](https://support.snyk.io/hc/en-us/articles/360010906897-Snyk-Priority-Score-and-Kubernetes). 

![Issue Card](./images/issue-card.png)

6. Switch to the Dependencies Tab to see the container's Dependency Tree.

![Dependency Tree](./images/dependency-tree.png)

7. Return to the Issues Tab. To give us a head start remediating the vulnerabilities in Goof, Snyk presents base image upgrade guidance grouped by how likely they are to be compatible with our application. 
    - Minor upgrades are the most likely to be compatible with little work, 
    - Major upgrades can introduce breaking changes depending on image usage,
    - Alternative architecture images are shown for more technical users to investigate.

![Upgrade Guidance](./images/upgrade-guidance.png)

In Part 2 we'll use this upgrade guidance to apply a more secure base image to Goof. 

## Part 2: Acting on Snyk Upgrade Guidance
A benefit of using Snyk Monitor to monitor running workloads is that once imported into the UI, Snyk continues to monitor the workload, identifying security issues as new images are deployed and the workload configuration changes.

In this section, you use Snyk Container's Base Image Upgrade Guidance and Snyk Infrastructure as Code (IaC) to address the security and configuration issues identified in Part 1 of the workshop.

> Note: You'll need a login to Quay.io to complete this Part.

> Note: EVERYTHING BELOW HERE IS WIP WHILE WE FINALIZE

### Fix Container Issues:
- Get the CLI Token from the Snyk UI
- Set the SNYK_TOKEN environment variable
- Install Snyk (npm i snyk)
- Git Clone the Code
- Build the Container as is
- snyk container test
- Review upgrade guidance
- Apply Base Image
- Push to Registry
- Update the YAML in OpenShift
- Bounce the Application in OpenShift
- Revisit Snyk UI to verify new results

TODO: #1 Explore using OpenShift internal registry? Requires adding Image Pull Secrets where the Snyk Monitor can access them. 

### Fix Configuration Issues:
- Snyk IaC test
- Edit the YAML in the OpenShift UI

# Conclusion

You reached the end of this workshop! This is one example of how Snyk guides developers through remediating vulnerabilities. There is much more we didn’t show, from our CI/CD integrations, CLI, API, and integrations into the Quay Registry. If you’re interested in other Snyk capabilities, check out the other [Red Hat Workshops in the Snyk Academy](https://solutions.snyk.io/partner-workshops/red-hat)!

