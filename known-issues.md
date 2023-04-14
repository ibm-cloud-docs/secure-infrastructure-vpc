---

copyright:
   years: 2023
lastupdated: "2023-04-14"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}


## Service ID API keys and Red Hat OpenShift
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key).

## Supported {{site.data.keyword.compliance_short}} goals
{: #ki-scc-goals}

The deployable architectures support a set of security controls, and those controls are listed with the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog.
If you use {{site.data.keyword.compliance_short}} or the Code Risk Analyzer plug-in for {{site.data.keyword.cloud_notm}} to scan your deployed resources against another security profile, you might see failures in the results.

## Authentication with trusted profiles not supported
{: #ki-trusted-profile}

When you deploy a deployable architecture with {{site.data.keyword.cloud_notm}} projects, you select an authentication method to deploy your configuration. Currently, deployable architectures support authenticating only with an API key. Trusted profile is not supported as an authentication method.
