---

copyright:
   years: 2023
lastupdated: "2023-04-28"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}

## Intermittent failures when you run apply or click deploy
{: #ki-policy-404}

You might experience a `404` `policy_not_found` error when the deployable architecture attempts to create an authorization policy. The failure is intermittent. Retry the command, for example, by clicking **Deploy** in projects or **Apply plan** in Schematics, or by running the apply command again. For more information, see [Intermittently getting error retrieving authorizationPolicy](https://github.com/IBM-Cloud/terraform-provider-ibm/issues/4413).

## Service ID API keys and Red Hat OpenShift
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key).

## Supported {{site.data.keyword.compliance_short}} goals
{: #ki-scc-goals}

The deployable architectures support a set of security controls, and those controls are listed with the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog.
If you use {{site.data.keyword.compliance_short}} or the Code Risk Analyzer plug-in for {{site.data.keyword.cloud_notm}} to scan your deployed resources against another security profile, you might see failures in the results.

## Authentication with trusted profiles not supported in some landing zones
{: #ki-trusted-profile}

When you deploy a deployable architecture with {{site.data.keyword.cloud_notm}} projects, you select an authentication method to deploy your configuration. Currently, the Red Hat OpenShift Container Platform on VPC landing zone supports authenticating only with an API key. Trusted profile is not supported as an authentication method for this landing zone.
