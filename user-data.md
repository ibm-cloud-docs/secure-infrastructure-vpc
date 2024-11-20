---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-20"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding user data to your VSI on VPC landing zone deployable architecture
{: #user-data}

You can specify optional user data that automatically performs common configuration tasks or runs scripts on your VSI on VPC landing zone deployable architecture. For more information, see [docs](https://cloud.ibm.com/docs/vpc?topic=vpc-user-data).
{: shortdesc}

To set user data for the VSIs, you need to pass a map to the optional `user_data` input variable.

## Example
{: #user-data-example}

When using the `user_data` variable in your configuration, it's essential to provide the content in the correct format for it to be properly recongnized by the terraform. 

The `user_data` variable should contain data formatted as follows:

```hcl
{
  management = {
    user_data = <<-EOT
#cloud-config
# vim: syntax=yaml
write_files:
- content: |
    # This is management

  path: /etc/sysconfig/management
EOT
  }
  workload = {
    user_data = <<-EOT
#cloud-config
# vim: syntax=yaml
write_files:
- content: |
    # This is workload

  path: /etc/sysconfig/workload
EOT
  }
}
```
{: codeblock}

- Keys to the map should be the same as the vpc names.
- Value for the `user_data` parameter in the object should follow YAML formatting.
- Use `<<-EOT` and `EOT` to enclose your `user_data` content to ensure it's passed as multi-line string.
