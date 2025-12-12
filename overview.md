---

copyright:
  years: 2023
lastupdated: "2025-12-12"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Overview of landing zone deployable architectures
{: #overview}

The [deployable architectures](#x10293733){: term} are a preconfigured set of infrastructure as code (IaC) assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture.
{: shortdesc}

The landing zone deployable architectures include Cloud foundation for VPC, Landing zone for containerized applications with OpenShift and Landing zone for applications with virtual servers. You can use the deployable architectures to create a secure and customizable [Virtual Private Cloud](#x4585403){: term} (VPC) environment.

For more information about deployable architectures, read about [infrastructure architectures](/docs/overview?topic=overview-secure-enterprise#define-architecture) in "Running secure enterprise workloads on {{site.data.keyword.cloud_notm}}".


## Cloud foundation for VPC
{: #overview-vpc}

The Cloud foundation for VPC deploys a simple {{site.data.keyword.cloud_notm}} VPC infrastructure without any compute resources, such as Virtual Server Instances (VSIs) or Red Hat OpenShift clusters.

You can use this architecture as the base for your compute resources. Other landing zone deployable architectures use the Cloud foundation for VPC as the base to deploy their resources.

The Cloud foundation for VPC is also a modular solution. The Cloud foundation for VPC creates network topology that uses two default VPCs, each containing multiple subnets to organize resources and define IP ranges. A transit gateway connects these VPCs to facilitate communication. An optional edge VPC in a specific location isolates and accelerates public internet traffic. IBM Cloud Object Storage is employed for Flow Logs and Activity Tracker to help ensure infrastructure observability and auditing.

You can deploy this architecture that uses various ways of configurations and thus can be employed for various use-cases. The following table outlines these **Variations** and use-cases they can suffice.

|Type of variation | Best suited for |
|---------------|-------|
|[Standard (Integrated setup with configurable services)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vpc-fully-configurable)|Ideal for users who seek flexibility to provide a dependable foundation. It grants complete control over architecture parameters, featuring optimized defaults that facilitate a fully functional Virtual Private Cloud (VPC) environment along with seamless integration of IBM Cloud services, eliminating the need for manual configuration.|
|[Standard - Financial Services edition](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vpc-ra) |Ideal for production workloads. It adheres to financial services compliance standards. It offers a validated configuration, which is designed to align with the IBM Cloud Framework for Financial Services, to help ensure all necessary regulatory requirements are met.|
{: caption="Cloud foundation for VPC Variations"}

For more information about Financial Services Framework, see [Getting started with {{site.data.keyword.cloud_notm}} for Financial Services](/docs/framework-financial-services).

## Landing zone for containerized applications with OpenShift
{: #overview-ocp}

The Landing zone for containerized applications with OpenShift deployable architecture deploys a Red Hat OpenShift Container Platform cluster on {{site.data.keyword.cloud_notm}} in a VPC environment.

This deployment architecture can create a customizable cluster, specifying its version and size. It offers features like worker pools for managing nodes with similar configurations, subnet configuration for worker node deployment, and endpoint settings for private and public access. Additionally, it includes ingress configuration to manage external traffic routing to internal services within the cluster.

You can deploy this architecture that uses various ways of configurations and thus can be employed for various use-cases. The following table outlines these **Variations** and use-cases they can suffice.

|Type of variation | Best suited for |
|---------------|-------|
|[Landing zone for containerized applications with OpenShift - QuickStart (Basic and simple)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ocp-ra-qs)|Ideal for rapid deployment for demonstration and development purposes without extensive configuration. It creates a fully customizable Virtual Private Cloud (VPC) environment in a single region.|
|[Landing zone for containerized applications with OpenShift - Standard (Integrated setup with configurable services)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ocp-fully-configurable)|Ideal for users seeking flexibility, providing a robust foundation to cater to production workloads. It grants complete control over architecture parameters, featuring optimized defaults that facilitate a fully functional OpenShift cluster and seamless integration with IBM Cloud services, eliminating the need for manual configuration|
|[Landing zone for containerized applications with OpenShift - QuickStart (Financial Services edition)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ocp-ra)|Ideal for rapid deployment for demonstration and development purposes without extensive configuration. It offers a single Red Hat OpenShift cluster within a secure VPC, tailored for your workloads that adheres to the Financial Services reference architecture.|
|[Landing zone for containerized applications with OpenShift - Standard (Financial Services edition)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-roks-ra-qs) |Ideal for production workloads. It can create secure and compliant Red Hat OpenShift Container Platform workload clusters within a Virtual Private Cloud (VPC) network that adheres to the Financial Services reference architecture.|
{: caption="Landing zone for containerized applications with OpenShift variations"}

For more information about the concepts of using Red Hat OpenShift Container Platform on VPC, see this [reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-openshift) from the {{site.data.keyword.cloud_notm}} for Financial Services docs.

## Landing zone for applications with virtual servers
{: #overview-vsi}

The Landing zone for applications with virtual servers deployment architecture  deploys Virtual Server Instances (VSIs) on {{site.data.keyword.cloud_notm}}. This deployable architecture provides you with secure and customizable compute resources for running your applications and services.

You can deploy this architecture that uses various ways of configurations and thus can be employed for various use-cases. The following table outlines these **Variations** and use-cases they can suffice.

|Type of variation | Best suited for |
|---------------|-------|
|[Landing zone for applications with virtual servers - QuickStart](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vsi-ra-qs) |Ideal for rapid deployment for demonstration and development purposes without extensive configuration. It sets up a fully customizable Virtual Private Cloud (VPC) environment in a single region.|
|[Landing zone for applications with virtual servers - QuickStart - Standard (Integrated setup with configurable services)](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-lz-vsi-fully-configurable-ra)|Ideal for users seeking flexibility, providing a robust foundation to cater to production workloads. It grants complete control over architecture parameters, featuring optimized defaults that facilitate a fully functional virtual server instance and seamless integration with IBM Cloud services, eliminating the need for manual configuration|
|[Landing zone for applications with virtual servers - Standard](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vsi-ra) |Ideal for users who seek a robust, secure, and customizable infrastructure based on the Financial Services reference architecture. It sets up virtual servers within a Virtual Private Cloud (VPC) across multiple zones to deploy workloads in a compliant, and scalable environment.|
{: caption="Landing zone for applications with virtual servers variations"}

For more information about the concepts of using this deployment architecture, see this [reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi) from the {{site.data.keyword.cloud_notm}} for Financial Services docs.
