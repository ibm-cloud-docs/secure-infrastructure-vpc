---

copyright:
   years: 2023
lastupdated: "2023-12-07"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Overriding key management in a landing zone deployable architecture
{: #override-kms}

The landing zone deployable architectures provide key management features by integrating {{site.data.keyword.keymanagementservicelong_notm}} or {{site.data.keyword.hscrypto}}. To make more advanced modifications to the key management that are not possible in most inputs, you use an override input.
{: shortdesc}

To override the values, you pass the JSON `key_management` object in an override input variable. For deployments with projects or {{site.data.keyword.bplong_notm}}, the values are added in the `override_json_string` input. Both {{site.data.keyword.keymanagementservicelong_notm}} and {{site.data.keyword.hscrypto}} are supported in the [landing zone deployable architectures](/docs/secure-infrastructure-vpc).

## Example key management overrides
{: #override-kms-use-cases}

These examples identify some common use cases to include a key management service (KMS) in your deployable architecture.

- [Create a {{site.data.keyword.keymanagementserviceshort}} instance and keys](#new-kms-new-keys)
- [Create keys in an existing KMS](#existing-kms-new-keys)
- [Use existing keys with no KMS](#no-kms-existing-keys)
- [Use existing keys and create more keys](#existing-and-new-keys)
- [Editing a project configuration](#override-kms-howto-projects)

 For a full list of supported key management attributes in the landing zone deployable architecture, see the `key_management` object in the [Inputs section](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone#inputs) of the `terraform-ibm-landing-zone` repository in GitHub.

## Create a {{site.data.keyword.keymanagementserviceshort}} instance and keys
{: #new-kms-new-keys}

This example describes the default configuration and doesn't require an override value. However, the example illustrates that you can override values that are not available in other input variables (for example, the key name). This override example creates an instance of IBM Key Protect named test-kms and creates keys that are named `slz-slz-key`, `slz-atracker-key`, `slz-roks-key`, and `slz-vsi-volume-key` in the `test-kms` instance.

1.  Copy the following JSON.

    ```json
    {
      "key_management": {
        "keys": [
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-slz-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-atracker-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-roks-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-vsi-volume-key",
            "root_key": true
          }
        ],
        "name": "test-kms",
        "resource_group": "testrg"
      }
    }
    ```
    {: codeblock}

1.  Follow the steps in the [Editing the project configuration](#override-kms-howto-projects) section to modify your deployable architecture.

## Create keys in an existing KMS
{: #existing-kms-new-keys}

This override example uses an existing {{site.data.keyword.keymanagementservicelong_notm}} instance that is called `test-kms`. You can use an existing instance by setting `use_data` to `true`. The rest of the example creates four keys in a key ring that is named `slz-slz-ring` in the existing instance.

To use {{site.data.keyword.hscrypto}} instead of {{site.data.keyword.keymanagementserviceshort}}, pass `"use_hs_crypto": true` instead of `"use_data": true`.
{: tip}

1.  Copy the following JSON.

    ```json
    {
      "key_management": {
        "keys": [
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-slz-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-atracker-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-roks-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-vsi-volume-key",
            "root_key": true
          }
        ],
        "name": "test-kms",
        "resource_group": "testrg",
        "use_data": true
      }
    }
    ```

1.  Follow the steps in the [Editing the project configuration](#override-kms-howto-projects) section to modify your deployable architecture.

## Use existing keys with no KMS
{: #no-kms-existing-keys}

To use existing keys without pulling in an existing KMS instance, include the CRN of the key. The CRN can refer to a key in the account that is deploying the deployable architecture or in a different account. The attributes for existing keys are `name` and `existing_key_crn`. When you use an existing key CRN, you must have an authentication policy that allows `block-storage`, `cloud-object-storage`, and `secrets-manager` to access the KMS in the external account. For more information, see [Using authorizations to grant access between services](https://cloud.ibm.com/docs/account?topic=account-serviceauth&interface=ui)

Make sure to omit `key_management.name` and `key_management.resource_group` when you don't want the deployable architecture to create a KMS.
{: tip}

1.  Copy the following JSON.

    ```json
    {
      "key_management": {
        "keys": [
          {
            "name": "slz-slz-key",
            "existing_key_crn": "crn:v1:bluemix:public:kms:ca-tor:a/testaccountid:15658dcb-7434-4ff7-961a-79cae8d9baca:key:6128229e-1bbb-4c25-827c-c97f077fb585"
          },
          {
            "name": "slz-atracker-key",
            "existing_key_crn": "crn:v1:bluemix:public:kms:ca-tor:a/testaccountid:15658dcb-7434-4ff7-961a-79cae8d9baca:key:9157cce8-3c1a-42be-b67f-0103426bc147"
          },
          {
            "name": "slz-roks-key",
            "existing_key_crn": "crn:v1:bluemix:public:kms:ca-tor:a/testaccountid:7ccfb2a6-4d7d-4e85-af35-9548cda719d0:key:682c774b-c781-4bbb-b3b7-3ddf1831ab2f"
          },
          {
            "name": "slz-vsi-volume-key",
            "existing_key_crn": "crn:v1:bluemix:public:kms:ca-tor:a/testaccountid:15658dcb-7434-4ff7-961a-79cae8d9baca:key:8c2ac54e-ac85-47da-8d03-06acb7f60ab3"
          }
        ]
      }
    }
    ```

1.  Follow the steps in the [Editing the project configuration](#override-kms-howto-projects) section to modify your deployable architecture.

## Use existing keys and create more keys
{: #existing-and-new-keys}

This example both creates keys and also uses an existing key. A {{site.data.keyword.keymanagementserviceshort}} instance that is called `test-kms` is created with three keys that are called `slz-slz-key`, `slz-atracker-key`, and `slz-roks-key`. In addition, the existing `slz-vsi-volume-key` key is used.

In this example, the existing key is identified by CRN to pull in the key without the KMS instance. Alternatively, you can set `"use_data": true` as in the [Create keys in an existing KMS](#existing-kms-new-keys) example. When you use an existing key CRN, you must have an authentication policy that allows `block-storage`, `cloud-object-storage`, and `secrets-manager` to access the KMS in the external account. For more information, see [Using authorizations to grant access between services](https://cloud.ibm.com/docs/account?topic=account-serviceauth&interface=ui)

1.  Copy the following JSON.

    ```json
    {
      "key_management": {
        "keys": [
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-slz-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-atracker-key",
            "root_key": true
          },
          {
            "key_ring": "slz-slz-ring",
            "name": "slz-roks-key",
            "root_key": true
          },
          {
            "name": "slz-vsi-volume-key",
            "existing_key_crn": "crn:v1:bluemix:public:kms:ca-tor:a/testaccountid:15658dcb-7434-4ff7-961a-79cae8d9baca:key:8c2ac54e-ac85-47da-8d03-06acb7f60ab3"
          }
        ],
        "name": "test-kms",
        "resource_group": "testrg"
      }
    }
    ```

1.  Follow the steps in the [Editing the project configuration](#override-kms-howto-projects) section to modify your deployable architecture.

## Editing the project configuration
{: #override-kms-howto-projects}

Use the following steps with any of the examples to override the configuration of your deployable architecture.

1.  Copy the JSON example. Modify it to match your needs.
1.  Add the JSON to the **override_json_string** input variable of your deployable architecture:

    1.  Go to the [Projects](/projects) page, and select a project.
    1.  Go to the **Configurations** tab, and select a deployable architecture configuration.
    1.  Click **Edit**.
    1.  In the **Configuration** section, click the **Optional** panel.
    1.  Find **override_json_string** and click **Edit JSON**.
    1.  Paste in this JSON that you copied earlier and click **Save.**
1.  Validate and approve your changes.
