---

copyright:
   years: 2023
lastupdated: "2023-12-12"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting help and support for the VPC landing zone deployable architectures
{: #help-support}

If you experience an issue or have questions when you deploy a VPC landing zone deployable architecture, you can use the following resources before you open a support case.
{: shortdesc}

- Review the [FAQs](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-faqs).
- ![{{site.data.keyword.cloud_notm}} icon](../icons/ibm-cloud-16.svg "IBM Cloud icon") Check the status of the {{site.data.keyword.cloud_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
- ![GitHub icon](../icons/logo-github-16.svg "GitHub icon") Review the [GitHub issues](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/issues){: external} to see whether other users experienced the same problem.
- Review the [troubleshooting documentation](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ts-deploy) to troubleshoot and resolve common issues.

<!-- - ![Slack icon](../icons/logo-slack-16.svg "Slack icon") Join the [Cloud Dev Tools @ IBM](https://ic-devops-slack-invite.us-south.devops.cloud.ibm.com/){: external} Slack community and reach out to us. -->

If you still can't resolve the problem, you can open a support case. For more information, see [Creating support cases](/docs/get-support?topic=get-support-open-case). If you're looking to provide feedback, see [Submitting feedback](/docs/overview?topic=overview-feedback).

## Providing support case details
{: #support-case-details}

To ensure that the support team can start investigating your case to provide a timely resolution, include details from the {{site.data.keyword.bpshort}} logs:

1.  Find the {{site.data.keyword.bpshort}} log:
    1.  In the {{site.data.keyword.cloud_notm}} console, go to **Schematics** > **Workspaces** > **deployable architecture instance**.
    1.  From the workspace **Activity** page, select the {{site.data.keyword.bpshort}} apply action that failed.
    1.  Click **Jobs** to see the detailed log output.
1.  Provide errors from the {{site.data.keyword.bpshort}} log:
    1.  In the log file, find the last action that {{site.data.keyword.bpshort}} started before the error occurred. For example, in the following log output, {{site.data.keyword.bpshort}} tried to run a copy script in the `instances_module` module by using the Terraform `null_resource`.

        ```text
        2021/05/24 05:03:41 Terraform apply | module.instances_module.module.compute_remote_copy_rpms.null_resource.remote_copy[0]: Still creating... [5m0s elapsed]
        2021/05/24 05:03:41 Terraform apply |
        2021/05/24 05:03:42 Terraform apply |
        2021/05/24 05:03:42 Terraform apply |
        2021/05/24 05:03:42 Terraform apply | Error: timeout - last error: ssh: rejected: connect failed (Connection timed out)
        ```
        {: screen}

    1.  Paste the errors into the case details.

1.  Provide the architecture name, source URL, and version from the log:

    1.  In the log file, find the architecture information. In the following example, you see the `Related Workspace`, `sourcerelease`, and `sourceurl`:

        ```sh
        2023/04/06 18:11:43 Related Workspace: name=deploy-arch-ibm-slz-ocp-04-06-2023, sourcerelease=(not specified), sourceurl=https modules/terraform-ibm-landing-zone/archive/v3.1.2.tar.gz, folder=terraform-ibm-landing-zone-3.1.2/patterns/roks
        ```
        {: screen}

    1.  Copy the architecture information and paste it into the case details.

## Routing your support case
{: #support-case-routing}

To route your support case correctly to speed up resolution, select the applicable product when you open the case.

### Routing when you can't deploy successfully
{: #support-routing-no-deploy}

If you can't deploy your deployable architecture, open a support case with the most likely cause of the issue:

- If you identified the service that you think is causing the error from the {{site.data.keyword.bpshort}} log, use the name of that service as the product name in the case.
- If you can't identify the error from the {{site.data.keyword.bpshort}} log, use the name of the deployable architecture as it is listed in the IBM Cloud catalog.

### Routing when you deployed successfully
{: #support-routing-deploy}

If you successfully deployed, yet have an issue with a service in the deployable architecture, open a support case and use the name of that service.
