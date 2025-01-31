---

copyright:
  years: 2023
lastupdated: "2025-01-31"

keywords:

subcollection: secure-infrastructure-vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting issues when upgrading to version 6.8.1
{: #ts-migration}
{: troubleshoot}

When upgrading to version 6.8.1 of the VSI on VPC landing zone or {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architectures, extra steps are excuated behind the scenes. The following is a guide on how to troubleshoot if any of the extra steps fail.
{: shortdesc}

You noticed that there was a failed job with the title `Terraform commands failed` in the IBM Schematics workspace associated with your projects configuration with an error in the logs that looked like this:
{: tsSymptoms}

```log
2025/01/30 15:37:57 [34mStarting command: terraform1.9 state mv ....
...
...
...
2025/01/30 15:38:01 [1m[31mTerraform STATE error: Terraform STATE errorexit status 1[39m[0m
2025/01/30 15:38:01 [1m[31mCould not execute job: Error : Terraform STATE errorexit status 1[39m[0m
```

When **upgrading** to version 6.8.1 (fresh installs are not applicable here), there are some extra steps that run as part of the validation phase:
{: tsCauses}

- Ansible
    - An ansible playbook inspects your terraform state and determines whether any migration is needed.
    - To view the logs of the ansible playbook:
        - Navagate to https://cloud.ibm.com/automation/schematics/ansible
        - Find the applicable playbook. It will be named in the format of: `<projects config ID>-validate-migration-script`
        - Logs can be seen under the `Action history` section
- Migration job
    - If the ansible playbook detected that a migration is required, it will trigger a job in the Schematcis workspace associated with your project configuration to execute some migration commands.
    - If this job fails, you should **not** proceed with the upgrade to v6.8.1. Even though the projects validation process will proceed to permform a terraform plan after the failed job, and mark validation as successful if the plan passes, you should not proceed to deploy it.

[Create a case](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-help-support#support-case-details) in the {{site.data.keyword.cloud_notm}} support center and include the logs of the failed job. Ensure to mention the name of the deployable architecture you are using (VSI on VPC landing zone or {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone), and the version you are trying to upgrade from.
{: tsResolve}
