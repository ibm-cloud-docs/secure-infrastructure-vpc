---

Copyright:
   years: 2025
lastupdated: "2025-11-11"

keywords: secure-infrastructure-vpc
subcollection: secure-infrastructure-vpc
content-type: tutorial
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Deploy application on {{site.data.keyword.redhat_openshift_notm}} cluster on {{site.data.keyword.cloud}}
{: #tutorialDeployAppOpenShift}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial provides step-by-step instructions to deploy an application to a {{site.data.keyword.redhat_openshift_notm}} cluster running on {{site.data.keyword.cloud_notm}}. You can use [Landing zone for containerized applications with OpenShift deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ocp-ra-qs) to deploy the cluster if you don't have an existing cluster.

## Before you begin
{: #beforeDeployAppOpenShift}

Before you deploy the application, you must meet the following prerequisites:

1. A {{site.data.keyword.redhat_openshift_notm}} cluster provisioned on {{site.data.keyword.cloud_notm}}.

1. Install {{site.data.keyword.redhat_openshift_notm}} CLI (`oc`). Refer the instructions [here](/docs/openshift?topic=openshift-cli-install).

1. An application to deploy. This tutorial uses a sample [Node.js app](https://github.com/IBM-Cloud/openshift-node-app), but you can use any other application in your preferred language, as long as you have a `Dockerfile` in the root of your repository to containerize it.


## Check cluster connectivity
{: #checkClusterConnectivity}
{: step}

Navigate to your cluster’s Overview tab in the Cloud Console and go to the Networking section to verify which endpoints are enabled.


### Public endpoint enabled
If the public endpoint is enabled, proceed to step 2.

### Private-only cluster
If your cluster has only private endpoint enabled, you won’t be able to access it directly via the CLI. To connect securely, deploy a  [client-to-site VPN Server](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-client-to-site-overview) in the same VPC as the cluster. This setup allows your local device to connect to the VPC network using an OpenVPN client. For deployment instructions refer this tutorial [client-to-site-vpn server](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-connect-landingzone-client-vpn). Once the VPN connection is established, proceed to step 2.


## Access cluster using oc CLI
{: #accessClusterForDemoApp}
{: step}

Connect with your cluster via CLI to deploy the application.

1. From the Cluster Overview page, open the OpenShift web console.

1. Click your account name to get the log in command on the OpenShift web console.

1. Click **Display Token** and copy the CLI command shown in the format `oc login --token=*********** --server=<server_address>` and run it in your terminal.


## Deploy the application
{: #deployDemoApp}
{: step}


To deploy the application, execute the following steps:

1. To define a project name, run the following command:
   ```bash
   export MYPROJECT=<your-initials>-openshiftapp
   ```

1. To create and switch to a new project, run the following command:
   ```bash
   oc new-project $MYPROJECT
   ```

1. To deploy application using Docker build strategy, run the following command:
   ```bash
   oc new-app https://github.com/IBM-Cloud/openshift-node-app --name=$MYPROJECT --strategy=docker --as-deployment-config
   ```

1. Wait for the build to complete and check status. To check deployment status, run the following command:
   ```bash
   oc status
   ```
   It might take few minutes for the application to build and deploy pods. It builds and pushes the image to internal registry and starts the main application pod.

1. To verify if the application pod is running, run the following command to list all the pods. Initially application pod might be in `ContainerCreating` phase but it should change in `Running` state in some time.
   ```bash
   oc get pods
   ```

   You might see two additional pods with `build` and `deploy` at the end of their name. They are OpenShift internal pods used to build and start your application pod.

1. To expose the service externally with an `HTTP` route, execute following command:
   ```bash
   oc expose service/$MYPROJECT
   ```

   This creates a `http` route for your application service.

1. To get the route hostname run the following command:
   ```bash
   oc get route/$MYPROJECT
   ```

   This is the hosting route to access your application in the browser.

1. To access your application, visit `http://<hosting_route>:80` in your browser.

1. To create an `HTTPS` route, run the following command:
   ```bash
   oc create route edge $MYPROJECT-https --service=$MYPROJECT --port=3000
   ```

1. To fetch the `HTTPS` route, run the following command:
   ```bash
   oc get route/$MYPROJECT-https
   ```
   This is the secure hosting route to access your application in the browser.

1. Access your application securely over `https` by visiting `https://secure_hosting_route` in your browser.

## Clean up resources
{: #deleteDemoProjectResources}
{: step}

To clean up the resources used to deploy the application, run the following commands:

1. Delete all resource objects created for the application.
   ```bash
   oc delete all --selector app=$MYPROJECT
   ```

1. Delete the Project.
   ```bash
   oc delete project $MYPROJECT
   ```
