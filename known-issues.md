---

copyright:
   years: 2023
lastupdated: "2023-05-08"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}

## Cannot delete SSH key in VSI on VPC landing zone
{: #ki-vsi-sshkey-error}

After the initial deployment of the VSI on VPC landing zone, subsequent `plan` or `apply` (deploy) commands incorrectly indicate that the public SSH key will be destroyed. When you try to apply the changes, the command fails with the following error:

> Error deleting SSH Key : SSH-key still in use

The issue applies to version 3.5.0 and later of the VSI on VPC landing zone. A fix is planned for an upcoming version.

## Intermittent failures when you run apply or click deploy
{: #ki-policy-404}

You might experience a `404` `policy_not_found` error when the deployable architecture attempts to create an authorization policy. The failure is intermittent. Retry the command, for example, by clicking **Deploy** in projects or **Apply plan** in Schematics, or by running the apply command again.

## Service ID API keys and Red Hat OpenShift
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key). For more information, see [Ensuring that the API key or infrastructure credentials owner has the correct permissions](/docs/openshift?topic=openshift-access-creds#owner_permissions) in the Red Hat OpenShift Cloud Docs.

## Authentication with trusted profiles not supported in some landing zones
{: #ki-trusted-profile}

When you deploy a deployable architecture with {{site.data.keyword.cloud_notm}} projects, you select an authentication method to deploy your configuration. 

Currently, the Red Hat OpenShift Container Platform on VPC landing zone supports authenticating only with an API key and not a service ID API key (see previous known issue). Trusted profile is not supported as an authentication method for this landing zone.

## Supported {{site.data.keyword.compliance_short}} rules
{: #ki-scc-goals}

The deployable architectures support a set of security controls, and those controls are listed with the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog.
If you use {{site.data.keyword.compliance_short}} or the Code Risk Analyzer plug-in for {{site.data.keyword.cloud_notm}} to scan your deployed resources against another security profile, you might see failures in the results.
