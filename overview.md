---

copyright:
   years: 2023
lastupdated: "2023-06-13"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Overview of VPC landing zone deployable architectures
{: #overview}

The VPC landing zone [deployable architectures](#x10293733){: term} are a preconfigured set of infrastructure as code (IaC) assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture.
{: shortdesc}

Three deployable architectures are included: VPC landing zone, VSI on VPC landing zone, and Red Hat OpenShift Container Platform on VPC landing zone. You can use the deployable architectures to create a secure and customizable [Virtual Private Cloud](#x4585403){: term} (VPC) environment.

The deployable architectures can help you meet the requirements and follow the best practices of {{site.data.keyword.cloud_notm}} Framework for Financial Services. For more information, see [Getting started with {{site.data.keyword.cloud_notm}} for Financial Services](/docs/framework-financial-services). For more information about deployable architectures, read about [infrastructure architectures](/docs/overview?topic=overview-secure-enterprise#define-architecture) in "Running secure enterprise workloads on {{site.data.keyword.cloud_notm}}".

## VPC landing zone
{: #overview-vpc}


You can use the VPC landing zone to deploy a simple {{site.data.keyword.cloud_notm}} VPC infrastructure without any compute resources, such as Virtual Server Instances (VSIs) or Red Hat OpenShift clusters. You can also use it as a base on which to deploy your own compute resources.

The VPC landing zone is also a modular solution. You can use this architecture as the base for your compute resources. In fact, the other landing zone deployable architectures use the VPC landing zone as the base for their resources.

This deployable architecture supports these features:

- Compliance: Aligns with the [VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about)
- VPCs: Creates a VPC-based topology based on two VPCs by default
- Subnets: Defines multiple subnets in the VPC to define IP ranges and organize resources within the network.
- Public gateways: Includes public gateways that provide connectivity between resources in a VPC and the public internet.
- Access control lists (ACLs): Creates ACLs and define rules for allowing or denying traffic between subnets within a VPC.
- Transit gateways: Creates a transit gateway to connect the two default VPCs that the deployable architecture creates.
- Security groups: Creates security groups to control inbound and outbound traffic to resources within the VPC.
- Key management: Adds key management by integrating the {{site.data.keyword.keymanagementservicefull_notm}} service or the {{site.data.keyword.hscrypto}}. These key management services help you create, manage, and use encryption keys to protect your sensitive data.
- Edge networking: Isolates and speeds traffic to the public internet by using an edge VPC in a specific location, if enabled
- Flow Logs and Activity tracking: Integrates these services to enhance the observability and auditing of your VPC infrastructure.


For more information about the components of VPCs, see [VPC concepts](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-concepts).

## VSI on VPC landing zone
{: #overview-vsi}

The VSI on VPC landing zone deployable architecture deploys Virtual Server Instances (VSIs) on {{site.data.keyword.cloud_notm}}. This deployable architecture provides you with secure and customizable compute resources for running your applications and services.

This deployable architecture supports these features:

- All the features of the VPC landing zone deployable architecture
- VSIs: Creates and configures one or more virtual servers in an {{site.data.keyword.cloud_notm}} VPC for workloads.
- Subnets for VSIs: Configures the subnets for the VSIs, and specifies which subnets the instances are deployed in.
- Security groups: Associates security groups with the VSIs to control inbound and outbound traffic to instances.
- SSH keys: Provisions and manages SSH keys for the VSIs so that you can securely administer the instances.

<!-- 04/06/2023: not now - Integration with existing VPCs: Deploys the VSIs in an existing VPC so that you can integrate with your existing cloud-based networking architecture. -->

For more information about the concepts of using the VSI on VPC landing zone deployable architecture, see this [reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi) from the {{site.data.keyword.cloud_notm}} for Financial Services docs.

## Red Hat OpenShift Container Platform on VPC landing zone
{: #overview-ocp}

The Red Hat OpenShift Container Platform on VPC landing zone deployable architecture provides the tools to deploy a Red Hat OpenShift Container Platform cluster on {{site.data.keyword.cloud_notm}}.

The deployable architecture deploys a Red Hat OpenShift cluster in a single VPC that is used to manage the platform in a VPC on {{site.data.keyword.cloud_notm}}. The VPC is a multi-zoned, multi-subnet implementation that keeps your VPC secure and highly available. The deployable architecture provides the tools to deploy a Red Hat OpenShift Container Platform cluster on {{site.data.keyword.cloud_notm}}.

This deployable architecture supports these features:

- All the features of the VPC landing zone deployable architecture
- Red Hat OpenShift Container Platform: Create a cluster in an {{site.data.keyword.cloud_notm}} VPC. You can specify the version and cluster size.
- Worker pools: Creates worker pools in a container platform. With worker pools, you can group and manage worker nodes with similar configurations, such as compute resources and availability zones.
- Subnets for containers: Configures subnets for the cluster, and specifies the subnets to deploy the worker nodes in.
- Private and public endpoints: Configures private and public endpoints for the cluster.
- Ingress configuration: Configures the ingress controller for the cluster. The controller is responsible for routing external traffic to the appropriate services within the cluster.

For more information about the concepts of using Red Hat OpenShift Container Platform on VPC, see this [reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-openshift) from the {{site.data.keyword.cloud_notm}} for Financial Services docs.
