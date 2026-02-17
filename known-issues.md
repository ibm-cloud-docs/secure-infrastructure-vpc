---

copyright:
  years: 2023, 2026
lastupdated: "2026-02-13"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}

For a list of common known issues see:
- [Known issues with Red Hat OpenShift on IBM Cloud / IBM Cloud Kubernetes Service when using terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-known-issues)

## OpenShift VPC cluster deployed by landing zone in warning state
{: #ki-ocp-cos-vpe-warning}

If you deployed an OpenShift VPC cluster using a landing zone deployable architecture version less than 7.2.2 and you notice the cluster has gone into a `Warning` state, it might be due to a known issue where the virtual private endpoint (VPE) for Cloud Object Storage that is created by the deployable architecture is conflicting with the ones automatically created by VPC clusters which results in worker nodes not being able to reach the Cloud Object Storage direct endpoint.

To confirm this:
1. Run the command `ibmcloud ks cluster get --cluster <cluster_name_or_id>`
2. Confirm that the `Status` is: `Some Cluster Operators are down-level and need to be updated, see 'https://ibm.biz/rhos_clusterversion_ts'`
3. Follow the steps in the link which will ask you to run the command `oc get clusterversion` on your cluster.
4. If you have hit the issue due to the conflicting virtual private endpoints for Cloud Object Storage, you will see something following status:
    ```
    $ oc get clusterversion
    NAME      VERSION   AVAILABLE   PROGRESSING   SINCE   STATUS
    version             False       True          14h     Unable to apply 4.16.28: the cluster operator image-registry is not available
   ```

### Workaround
{: #ki-workaround-ocp-cos-vpe-warning}
Upgrade to version 7.2.2 or later where you will see the expected destroy of the landing zone created virtual private endpoint for Cloud Object Storage and its associated reserved IP.

## Unsupported attribute error after interrupted apply or destroy
{: #ki-unsupported-attribute}

If a Terraform `apply` or `destroy` operation is interrupted, you might see an "Unsupported attribute" error at the next Terraform operation. Typically, this error occurs when a `destroy` operation is cancelled or has an unexpected error.

```text
Error: Unsupported attribute
   on .terraform/modules/landing_zone/dynamic_values/config_modules/vsi/vsi.tf line 20, in module "vsi_subnets":
   20:   subnet_zone_list = var.vpc_modules[each.value.vpc_name].subnet_zone_list
     ├────────────────
     │ each.value.vpc_name is "management"
     │ var.vpc_modules is object with 3 attributes

This object does not have an attribute named "subnet_zone_list".
```
{: screen}

The error occurs because the interrupted operation leaves key resources in a partial deployment state and other resources are configured to use these missing components.

### Workaround
{: #ki-unsupported-workaround}

To complete the `destroy` operation that is stuck in this state, try to decouple the resources that depend on the missing components. To decouple the resources, use the `override` feature, for example by setting the `override_json_string` input variable.

- VSI on VPC landing zone deployable architectures that are missing their VPC components: `override_json_string` = `'{"vsi": []}'`
- Red Hat OpenShift Container Platform on VPC landing zone deployable architectures: `override_json_string` = `'{"clusters": []}'`

## Changing prefix value recreates subnets and address prefixes
{: #ki-vpc-prefix-change-recreate}

If you deployed Landing Zone VPC infrastructure and later update the `prefix` input value, Terraform might plan to destroy and recreate subnet and VPC address prefix resources.

This behavior occurs because Landing Zone VPC module use the `prefix` value as part of the internal Terraform resource keys. When the prefix changes, Terraform treats the existing resources as different objects and plans replacement instead of renaming them in place.

You might notice output similar to:

```text
resource "ibm_is_vpc_address_prefix" will be destroyed
(because key is not in for_each map)
```
{: screen}

### Impact
{: #ki-vpc-prefix-change-impact}

Subnets and address prefixes can be recreated.

Dependent resources might also be replaced depending on configuration.

This affects all solutions that consume the Landing Zone VPC module.

### Workaround
{: #ki-vpc-prefix-change-workaround}

Avoid changing the `prefix` value after the infrastructure is deployed. Treat the prefix as an immutable identifier for an existing environment when possible.

If a different prefix is required, expect a disruptive update where Terraform can recreate address prefixes, subnets, and dependent resources. Plan maintenance windows and impact accordingly before applying the change.
