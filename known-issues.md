---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-19"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}


## Error when you delete a deployable architecture
{: #ki-delete-rg}

When you try to destroy a deployable architecture, you might see this error about resource groups:

> Error: [ERROR] Error Deleting resource group: Resource groups with active or pending reclamation instances can't be deleted. Use the CLI commands "ibmcloud resource service-instances --type all" and "ibmcloud resource reclamations" to check for remaining instances, then delete the instances and try again.

The error occurs because of a delay in cleaning up deleted volumes. The delay prevents the deletion of the resource group.

The volume is cleaned up within 24 hours, so you can try again to delete the deployable architecture after one day. If you need immediate cleanup, [create a support case](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-help-support).

- Attach the output of the following command: `ibmcloud resource service-instances --type all -g <resource-group>`
- Ask the BSS team to clean up the volumes that are listed.

## Intermittent provisioning error with clusters and worker pools
{: #ki-provider-retry}

An intermittent error occurs when you provision a cluster or worker pool. The Terraform command fails with a `409` error that indicates that the resource exists. However, when a resource with the same name doesn't exist, the error might be caused by the retry logic of the IBM Terraform Provider. In this case, the provision request completes successfully, but the provider retries the command and then passes the error.

### Cluster error workaround
{: #ki-provider-retry-cluster-err}

If you provision a cluster, you might receive an error that's similar to the following error:

```hcl
Error: Request failed with status code: 409, ServerErrorResponse: {"incidentID":"4941c546-2cce-4a5f-879f-03bce223c555","code":"E0007","description":"A cluster with the same name already exists. Choose another name.","type":"Provisioning"}
│
│   with ibm_container_vpc_cluster.cluster,
│   on main.tf line 53, in resource "ibm_container_vpc_cluster" "cluster":
│   53: resource "ibm_container_vpc_cluster" "cluster" {
│
╵}
```

Check that the cluster is created in your account. If it is, you can issue the following command to import it into your state file.

```sh
terraform import 'ibm_container_vpc_cluster.cluster' "<my cluster ID>"
```

Replace `<my cluster ID>` with the ID of your cluster.

### Worker pool error workaround
{: #ki-provider-retry-worker-err}

If you provision a worker pool, you might receive an error that's similar to the following error:

```hcl
│ Error: Request failed with status code: 409, ServerErrorResponse: {"incidentID":"eeb16087-7eff-4b89-99e9-c3707350bf80,eeb16087-7eff-4b89-99e9-c3707350bf80","code":"E0d39","description":"A worker pool with the same name already exists within the cluster. Choose another name.","type":"Provisioning"}
│
│   with module.ocp_all_inclusive.module.ocp_base.ibm_container_vpc_worker_pool.pool["transit"],
│   on .terraform/modules/ocp_all_inclusive.ocp_base/main.tf line 252, in resource "ibm_container_vpc_worker_pool" "pool":
│  252: resource "ibm_container_vpc_worker_pool" "pool" {
│
╵}
```

Check that the worker pool is created in your account. If it is, you can issue the following command to import the pools into your state file.

```sh
terraform import 'module.ocp_all_inclusive.module.ocp_base.ibm_container_vpc_worker_pool.pool["transit"]' "<my cluster name>/<worker pool id>"
```

 Replace `<my cluster name>/<worker pool id>` with the cluster and worker pool information.

## VSI on VPC landing zone QuickStart variation fails projects validation
{: #ki-vsiqs-validation-fail}

With the QuickStart variation of the VSI on VPC landing zone that you deploy to {{site.data.keyword.cloud_notm}} projects, the configuration fails validation during the the Code Risk Analyzer scan. The QuickStart variation is designed to deploy quickly for demonstration and development and doesn't claim any controls for the scan.

## Service ID API keys and {{site.data.keyword.redhat_openshift_notm}}
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](/docs/account?topic=account-userapikey&interface=terraform#create_user_key-api-terra). For more information, see [Ensuring that the API key or infrastructure credentials owner has the correct permissions](/docs/openshift?topic=openshift-access-creds) in the {{site.data.keyword.redhat_openshift_notm}} Cloud docs.

## Authentication with trusted profiles not supported in some landing zones
{: #ki-trusted-profile}

When you deploy a deployable architecture with {{site.data.keyword.cloud_notm}} projects, you select an authentication method to deploy your configuration.

Currently, the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone supports authenticating only with an API key and not a service ID API key (see previous known issue). Trusted profile is not supported as an authentication method for this landing zone.

## Supported {{site.data.keyword.compliance_short}} rules
{: #ki-scc-goals}

The deployable architectures support a set of security controls, and those controls are listed with the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog.

If you use {{site.data.keyword.compliance_short}} or the Code Risk Analyzer plug-in for {{site.data.keyword.cloud_notm}} to scan your deployed resources against another security profile, you might see failures in the results.

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
- Red Hat OpenShift Container Platform on VPC landing zone deployable architectures:`override_json_string` = `'{"clusters": []}'`
