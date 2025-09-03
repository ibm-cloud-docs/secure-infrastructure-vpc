---

copyright:
  years: 2023
lastupdated: "2025-09-03"

keywords:

subcollection: secure-infrastructure-vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}



# FAQs for landing zone deployable architectures
{: #faqs}

FAQs for the VPC landing zone deployable architectures. To find all FAQs for {{site.data.keyword.cloud}}, see our [FAQ library](/docs/faqs).
{: shortdesc}

## What is a deployable architecture?
{: #what-is-da}
{: faq}

A deployable architecture is a combination of capabilities from one or more technologies that solve a customer-defined problem, and it can have one or more reference architectures based on the customer business needs. For more information about deployable architectures, see [What are modules and deployable architectures?](/docs/secure-enterprise?topic=secure-enterprise-understand-module-da) and read about [infrastructure architectures](/docs/overview?topic=overview-secure-enterprise#define-architecture) in "Running secure enterprise workloads on IBM Cloud".

## What is infrastructure as code?
{: #what-is-iac}
{: faq}

Infrastructure as code (IaC) is code to manage and provision infrastructure (for example, networks, virtual machines, load-balancers, clusters, services, and connection topology) in a descriptive model rather than by using manual processes.

With IaC, code defines your infrastructure, specifying your resources and their configuration. Your infrastructure code is treated the same as app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring. The landing zone deployable architectures use [Terraform](https://www.terraform.io/){: external} to specify the infrastructure and [{{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-getting-started) to manage the deployment.

## How do I estimate costs?
{: #what-is-project-cost}
{: faq}

You can view and estimate of starting costs for a variation of the deployable architecture from the {{site.data.keyword.cloud_notm}} catalog details page. When you deploy by using {{site.data.keyword.cloud}} projects, the starting costs for the project are estimated from the validation window after your changes to the configuration are saved and validated.

## Are new releases compatible with previous ones?
{: #what-is-compatible}
{: faq}

Changes adhere to semantic versioning, with releases labeled as `{major}.{minor}.{patch}`. For more information, see the [release compatibility](https://terraform-ibm-modules.github.io/documentation/#/versioning){: external} in the {{site.data.keyword.cloud_notm}} Terraform modules documentation.
