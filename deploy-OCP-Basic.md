---

Copyright:
   years: 2025
lastupdated: "2025-10-01"

keywords: secure-infrastructure-vpc
subcollection: secure-infrastructure-vpc
content-type: tutorial
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}

# Deploy {{site.data.keyword.redhat_openshift_notm}} Container platform on IBM Cloud by using the QuickStart basic and simple variation
{: #tutorialQuickStartOCPFCforPOC}
{: toc-content-type="tutorial"}
{: toc-completion-time="1h"}

In this tutorial, you will learn to deploy a {{site.data.keyword.redhat_openshift_notm}} Container platform on IBM Cloud by using the **QuickStart Basic and simple** variation.

## Before you begin
{: #beforeQuickStartOCPFCforPOC}

Before you begin, you need the following prerequisites:

* An IBM Cloud account.
* An API key. For the purposes of this tutorial, create an API key https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key.
* For more information about the permissions, you can check the **Permissions** tab for each of the variations.

Make sure that you have a clear understanding of the purpose of deployment to select a variation type that fulfills your requirement. When you select a variation and begin building on it, you cannot switch or scale it down to another variation. For instance, if your goal is to quickly deploy {{site.data.keyword.redhat_openshift_notm}} without worrying much about security and compliance, you might choose the **QuickStart - Basic and simple** variation. However, when you deploy this architecture, you cannot upgrade it to other variation.
{: important}

In this tutorial, choose the **Quickstart - Basic and simple** variation to deploy a {{site.data.keyword.redhat_openshift_notm}} cluster.

## Create a project for deployment
{: #CreateAProject-ForQuickStartOCP-FC}
{: step}

You need to add or reuse existing projects for a deployment. To create or reuse the existing projects,

1. Log in to your IBM Cloud account. Search for the **Landing zone for containerized applications with {{site.data.keyword.redhat_openshift_notm}}** deployment architecture product tile in the **Catalog** and click it.

1. To quickly deploy a {{site.data.keyword.redhat_openshift_notm}} cluster, select the **QuickStart - Basic and simple** variation.

   This deployment architecture has a set of pre-defined default values that helps you to quickly provision a {{site.data.keyword.redhat_openshift_notm}} cluster. Before you go ahead with this deployment, review the **Architecture overview** and **Permissions** section for details. Review the **Summary** and the resources that the deployable architecture creates for you.

1. Click **Configure and deploy**. To associate a project, you can choose to:

   - **Create a new project**: Create a new project and click **Create**.

   - **Add to existing project**: Select an existing project and click **Add**.

   For this tutorial, create a new project, enter the **Name**, **Description**, **Configuration name**, **Region**, and the **Resource group** where you want to create a project.

   Projects are based on [Infrastructure as Code (IaC) approach to deployments](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects).

## Configure the deployment
{: #ConfigureDeployment-ForQuickStartOCP-FC}
{: step}

To quickly provision a {{site.data.keyword.redhat_openshift_notm}} container platform, you need to provide values of the required inputs. Review the well-chosen defaults for the fields in the following sections.

1. Review the configured values in the **Details** section. These values identify the deployment architecture like the name, and version of the deployment.

   Click **Next** to proceed.

1. [Create an API Key](/docs/secure-enterprise?topic=secure-enterprise-authorize-project#create-new-secret) or provide an existing API key in the **Security** section for authentication. Review the default value for **Security and Compliance Center controls** and update to align with the compliance requirements.

   Click **Next** to continue.

1. Provide values for the required inputs that suits best for your use-case in the **Inputs** section.

   The *prefix* helps you to identify your resources after deployment (e.g test).

   The *region* defaults to `us-south`. You can select any region where you want to deploy the resources.
   
   The *size* determines the number of availability zones, worker nodes per zone, and the machine type used for the Red Hat OpenShift cluster.
   The size defaults to `mini`. By default, the architecture provisions a **two-zone VPC**, forming the foundation for the OpenShift cluster. The cluster comprises a single worker pool that is distributed across these zones, with **one worker node per zone** in the `mini` configuration.

   However, you must [review all the combinations for small, medium, and large sizes](https://github.com/terraform-ibm-modules/terraform-ibm-base-ocp-vpc/blob/main/solutions/quickstart/DA_docs.md) to handle the workloads in the most optimum manner.

1. Optional. Toggle **Optional Inputs** to configure values like resource group, cluster name, operating system installed on the worker nodes, and more in the **Inputs** section. These fields are pre-configured with intelligent default values. However you must review these values. You can update these default values to align with your requiremnet to fine-tune the deployment for greater precision and control.

   Click **Save** to store your configuration.

## Validate the configuration and deploy
{: #ValidateDeployment-ForQuickStartOCP-FC}
{: step}

You need to validate, approve, and deploy all the resources sequentially.

To automatically approve and deploy these configurations in the required order, you can go to the Project Dashboard, then go to **Manage** and then click **Settings** and turn on the **Auto-deploy**. {: tip}

1. Click **Validate** to run a pre-deployment check for your variation. This process takes some time based on your configurations and resources.

1. After validation runs, add a comment, and click **Approve**. When you approve the configuration, click **Deploy**.

1. Go to the **Activity** section in the project dashboard to monitor the status of the deployment. You can view real-time logs and updates to the configurations. The deployment time depends on your configurations and resources.

