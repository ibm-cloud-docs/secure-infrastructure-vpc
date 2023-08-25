---

copyright:
   years: 2023
lastupdated: "2023-08-25"

keywords:

subcollection: secure-infrastructure-vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see an update to my Kubernetes version?
{: #ts-kube-ver-mismatch}
{: troubleshoot}

The Red Hat OpenShift Container Platform on VPC landing zone log shows a Kubernetes version change.
{: shortdesc}

You clicked **Validate** in projects, or clicked **Apply plan** in {{site.data.keyword.bplong_notm}}, or ran the `ibmcloud schematics plan` command. You then saw a message about a new version of Kubernetes, as in the following command-line example:
{: tsSymptoms}

```terraform
kube_version = "4.12.16_openshift" -> "4.12.20_openshift"

Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to undo or respond to these changes.
```

The new version is detected because the Kubernetes master node is updated outside of Terraform and the Terraform state is out of date with that version. The Kubernetes version is ignored in the module code, so the infrastructure will not be changed.
{: tsCauses}

You can ignore the message. The message identifies only that drift exists in the versions. After the changes are applied, the state will be refreshed.
{: tsResolve}
