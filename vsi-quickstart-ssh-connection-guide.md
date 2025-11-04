---

copyright:
  years: 2025
lastupdated: "2025-11-03"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connect to a Virtual Server Instance (VSI) using SSH
{: #connect-vsi-using-ssh}

This guide helps you to connect to your {{site.data.keyword.cloud_notm}} [Virtual Server Instance (VSI)](docs/vpc?topic=vpc-about-advanced-virtual-servers) that is deployed using the [Deployment Architecture Quickstart - Basic and simple](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global) using [SSH](https://www.openssh.org/).
{: shortdesc}

## Prerequisites
{: #prereq-with-vsi}

- Your Virtual Server Instance (VSI) has been successfully deployed.
- Your Virtual Server Instance (VSI) is assigned a [floating IP](/docs/vpc?topic=vpc-fip-about) address.
- [ibmcloud](/docs/cli?topic=cli-install-ibmcloud-cli) CLI tool is installed.
- [jq](https://jqlang.org/) command-line JSON processor is installed.

## Connect to an VSI using SSH
{: #connect-with-vsi}

To connect to IBM Cloud VSI using SSH, execute the following steps:

1. You need your Workspace ID from Projects UI. Navigate to **Configurations** tab and click on your configuration. Copy the Workspace ID under **Workspace**.

1. To set environment variables in your terminal, execute the following command.

      ```bash
      # Set your workspace ID
      WORKSPACE_ID="<YOUR_WORKSPACE_ID_HERE>"  # example: "us-south.workspace.projects-service.8f617fb9"
      ```

1. Run the following command to extract the VSI name and `floating_ip` address.

      ```bash
      ibmcloud schematics output --id $WORKSPACE_ID -o JSON | jq -r '.[0].output_values[] | select(.fip_list) | .fip_list.value[0] | "VSI Name: \(.name)\nFloating IP: \(.floating_ip)"'
      ```

1. If you are using an existing SSH private key, skip this step. Run the following command to extract the SSH private key and save it as    `vsi-private-key.pem` with secure `400` permissions.

      ```bash
      ibmcloud schematics output --id $WORKSPACE_ID -o JSON > /tmp/ws_output.json && KEY_FILE="vsi-private-key.pem" && jq -r '.[0].output_values[] | select(.ssh_private_key) | .ssh_private_key.value' /tmp/ws_output.json > "$KEY_FILE" && chmod 400 "$KEY_FILE" && echo "Private Key saved to: $(pwd)/$KEY_FILE" && rm /tmp/ws_output.json
      ```
     The command displays the file path where the key is saved.

1. Choose the corresponding `username` based on the operating system from the following table.

      | Operating System | Default Username |
      |-----------------|------------------|
      | Ubuntu | `ubuntu` |
      | Redhat | `vpcuser` |
      | Debian | `vpcuser` |
      | Fedora | `core` |
      | CentOS | `vpcuser` |
      {: caption="List user names for operating systems}

1. Run the following command to connect to your VSI.

      ```bash
      ssh -i vsi-private-key.pem <username>@<floating_ip>
      ```

      For different operating systems, replace the placeholders with your actual values in the following examples.

      For Ubuntu:
      ```bash
      ssh -i vsi-private-key.pem ubuntu@150.240.160.54
      ```

      For Redhat, Debian, and CentOS:
      ```bash
      ssh -i vsi-private-key.pem vpcuser@150.240.160.54
      ```

      For Fedora:
      ```bash
      ssh -i vsi-private-key.pem core@150.240.160.54
      ```

1. When you connect for the first time you will see the following message about host authenticity.

      ```bash 
      The authenticity of host '150.240.160.54 (150.240.160.54)' can't be established.
      ECDSA key fingerprint is SHA256:...
      Are you sure you want to continue connecting (yes/no)?
      ```

      Type `yes` and press Enter to continue.
      
      Now you should be successfully connected to your IBM Cloud VSI with the welcome message:

      ```bash
      ==========================================
      Welcome to Your IBM Cloud VSI!
      ==========================================
      Server Information:
      - Hostname: sky-da-qs-vsi-69f8-001
      - IP Address: 10.10.10.4
      - OS: Debian GNU/Linux 13 (trixie)
      ```

Your Virtual Server Instance (VSI) is now connected and ready for use. You can utilize this virtual machine for development, hosting applications, running services, or any other workload based on your requirements.
