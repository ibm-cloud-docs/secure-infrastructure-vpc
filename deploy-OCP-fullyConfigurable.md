---

Copyright:
   years: 2025
lastupdated: "2025-10-01"

keywords: secure-infrastructure-vpc
subcollection: secure-infrastructure-vpc
content-type: tutorial
completion-time: 4h

---

{{site.data.keyword.attribute-definition-list}}

# Deploy {{site.data.keyword.redhat_openshift_notm}} Container platform on IBM Cloud by using the Standard - Integrated setup with configurable services
{: #tutorialConfigurableOCP}
{: toc-content-type="tutorial"}
{: toc-completion-time="4h"}

In this tutorial, you will learn to deploy a {{site.data.keyword.redhat_openshift_notm}} Container platform on IBM Cloud by using the **[Experimental] Standard - Integrated setup with configurable services** variation.

You can customize and have more control on the deployment but still begin with a stable, pre-configured foundation. It has the following features.

- Full control over architecture parameters

    You can refine the cluster setup to meet specific performance, scalability, or compliance needs. These parameters are required to adjust compute resources, configure network, manage storage options, and more.

- Well-chosen defaults

    You can review the default configurations and can modify them to suit your use-case. These defaults are optimized for general use cases to reduce the need for manual intervention during initial setup. These defaults guide values for fields that are related to the underlying infrastructure.

- Integrated IBM Cloud services

    You can integrate the deployment with IBM Cloud services such as monitoring, logging, and security to enhance productivity and operational efficiency. These integrations are pre-configured and help to develop an end to end solution rapidly.

Thus it strikes a balance between ease of use and technical depth, making it suitable for both novice and experienced developers.

## Before you begin
{: #before-fullyConfigurableOCP}

Before you begin, you need the following prerequisites:

* An IBM Cloud account.
* An API key. For the purposes of this tutorial, create an API key https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key.
* For more information about the permissions, you can check the **Permissions** tab for each of the variations.

In this tutorial, choose the **[Experimental] Standard - Integrated setup with configurable services** variation to deploy a {{site.data.keyword.redhat_openshift_notm}} cluster.

## Create a project for deployment
{: #CreateAProject-fullyConfigurableOCP}
{: step}

You need to add or reuse existing projects for a deployment. To create or reuse the existing projects,

1. Log in to your IBM Cloud account. Search for the **Landing zone for containerized applications with {{site.data.keyword.redhat_openshift_notm}}** deployment architecture product tile in the **Catalog** and click it.

1. To quickly deploy a {{site.data.keyword.redhat_openshift_notm}} cluster, select the **[Experimental] Standard - Integrated setup with configurable services** variation.

   This deployment architecture has a set of integrated services that help you to provision a {{site.data.keyword.redhat_openshift_notm}} cluster. Before you go ahead with this deployment, review the **Architecture overview**, **Components**, **Permissions**, and **Security & Compliance** section for details. Review the **Summary** and the resources that the deployable architecture creates for you.

1. Click **Configure and deploy**. To associate a project, you can choose to:

   - **Create a new project**: Create a new project.

   - **Add to existing project**: Select an existing project.

   For this tutorial, create a new project, enter the **Name**, **Description**, **Configuration name**, **Region**, and the **Resource group** where you want to create a project.

   Projects are based on [Infrastructure as Code (IaC) approach to deployments](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects).

   Click **Next**.

## Add or remove deployable architectures
{: #AddRemoveDA-fullyConfigurableOCP}
{: step}

To build robust applications that serve at production level, you need a strong platform and a secure infrastructure. You can add or remove deployable architectures to align with your infrastructure, networking, security, compliance, and observability requirements.
This architecture provides you with an array of deployable architectures for Cloud management like **Cloud automation for Cloud Logs**, **Cloud automation for Key Protect** and many more.

To illustrate an observability use case, select the following deployable architectures,

1. **Cloud automation for Cloud Monitoring**

1. **Cloud automation for Cloud Logs**

1. **Cloud automation for Activity Tracker Event Routing**

Remove **Cloud automation for Key Protect**, **Cloud automation for Security and Compliance Center Workload Protection** and **Cloud automation for Secrets Manager** deployable architectures.

Click **Add to Project**.

## Configure the deployment
{: #ConfigDeploy-fullyConfigurableOCP}
{: step}

To provision a {{site.data.keyword.redhat_openshift_notm}} container platform with integrated set of configurable services, you need to configure a set of required and review defaults for optional inputs.

1. Review the configured values in the **Details** section. These values identify the deployment architecture like the name, and version of the deployment.

   Click **Next** to proceed.

1. [Create an API Key](/docs/secure-enterprise?topic=secure-enterprise-authorize-project#create-new-secret) or provide an existing API key in the **Security** section for authentication. Review the default value for **Security and Compliance Center controls** and update to align with the compliance requirements.

1. Provide values for the required inputs like prefix of resources, cluster name, region of deployment, and many more in the **Inputs** section.

    The deployment creates resources and the `prefix` value helps identify the configured resources.

    To enable **Cloud Monitoring**, set `enable_platform_metrics`. You can configure one instance of the IBM Cloud Monitoring service per region to collect platform metrics in that location.

    To manage **Cloud Logs** that are generated by IBM Cloud services in a region, you must create a tenant in each region that you operate. Pass a list of regions to create a tenant to the `logs_routing_tenant_regions` field, for example, `["us-south", "us-east"]`.

1. Optional. Toggle **Optional Inputs** to configure values like resource group, cluster name, operating system installed on the worker nodes, list of subnets for VPC, and more in the **Inputs** section. These fields are pre-configured with intelligent default values. However, you must review these values. You can update these default values to align with your requirements to fine-tune the deployment for greater precision and control.


## Validate the configuration and deploy
{: #ValidateDeployment-fullyConfigurableOCP}
{: step}

You need to validate, approve, and deploy all the resources sequentially.

To automatically approve and deploy these configurations in the required order, you can go to the Project Dashboard, then go to **Manage** and then click **Settings** and turn on the **Auto-deploy**. {: tip}

1. Click **Validate** to run a pre-deployment check for your variation. This process takes some time based on your configurations and resources.

1. After validation runs, add a comment, and click **Approve**. When you approve the configuration, click **Deploy**.

1. Go to the **Activity** section in the project dashboard to monitor the status of the deployment. You can view real-time logs and updates to the configurations. The deployment time depends on your configurations and resources.

