---

copyright:
  years: 2023
lastupdated: "2025-01-16"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding a VSI to your VPC landing zone deployable architecture
{: #ext-with-vsi}

You can add a VSI to a VPC landing zone deployable architecture that you already deployed. The deployable architecture adds a VSI in each VSI subnet of the landing zone VPC.
{: shortdesc}

## Before you begin
{: #prereq-with-vsi}

You need your VPC ID, subnets, and boot volume encryption key from your existing VPC landing zone deployable architecture. You can find in different ways, depending on how you are deployed the architecture.

- Find all the information with projects:
    1.  Click the **Navigation menu** icon, and then click **[Projects](/projects){: external}**.
    1.  Select the project that is associated with the VPC landing zone deployable architecture.
    1.  In the **Configurations** tab, select the deployable architecture where you want to deploy the VSI.
    1.  Find the IDs in the Outputs section:
        - Find the `vpc_data` output. Copy the `vpc_id` of the VPC.
        - Find the `subnet_data` output. Copy the names of the subnets where you want to deploy the VSI.
        - Find the `key_map` output. Copy the `crn` of the key that you want to use.

    Or

- Find the VPC ID and subnet names from the console:
    1.  Click the **Navigation menu** icon, and then click **VPC Infrastructure** > **VPCs** in the **Network** section.
    1.  Click the VPC that is associated with the VPC landing zone deployable architecture.
    1.  In the **Virtual private cloud details** section, copy the `VPC ID` where you want to deploy the VSI.
    1.  In the **Subnets in this VPC** section, copy the subnet names that you want to use.
- Find the boot volume encryption key from the API:
    1.  Use the **Retrieve a key** method of the Key Protect API. For more information, see [Retrieving a key](/docs/key-protect?topic=key-protect-retrieve-key).

## Deploying the VSI on your VPC landing zone deployable architecture
{: #deploy-with-vsi}

 You add the VSI by creating another deployable architecture on the existing VPC landing zone.

1.  Go to the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external} and find the VPC landing zone deployable architecture.
1.  In the VPC landing zone details page, select the **Standard** variation.
1.  Select **Extends VPC landing zone**.
1.  Click **Review deployment options** and select an option.
1.  Complete the information required to deploy the VSI. You need the VPC ID, subnets, and boot volume encryption key that you copied in [Before you begin](#prereq-with-vsi).

## Next steps
{: #next-with-vsi}

[Sharing a customized deployable architecture to your enterprise](/docs/secure-enterprise?topic=secure-enterprise-share-custom).
