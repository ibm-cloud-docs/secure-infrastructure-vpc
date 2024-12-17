---

copyright:
  years: 2023, 2024
lastupdated: "2024-12-17"
lasttested: "2024-02-12"

keywords:
subcollection: secure-infrastructure-vpc
content-type: tutorial
services: vpc, openshift, secrets-manager, dl, schematics
account-plan: paid

---

{{site.data.keyword.attribute-definition-list}}

# Connect to a VPC landing zone by using a client-to-site VPN
{: #connect-landingzone-client-vpn}
{: toc-content-type="tutorial"}
{: toc-services="vpc, openshift, secrets-manager, dl, schematics"}
{: toc-completion-time="2h"}

This tutorial dives into the fastest option to get up and running with a [client VPN for VPC](/docs/vpc?topic=vpc-vpn-client-to-site-overview) connectivity. Rather than doing manual steps, you set up an automated way to create a client-to-site VPN connection to one or more landing zones in your account by using {{site.data.keyword.client-to-site-vpn}} deployable architecture from Community Catalog.
{: shortdesc}

## Objectives
{: #solution-connect-client-vpn-objectives}

- Create a client-to-site VPN connection between the private VPC network and clients by using {{site.data.keyword.client-to-site-vpn}} deployable architecture from Community Catalog.{: term}.

### Problem
{: #solution-connect-client-vpn-problem}

Let's say that you deployed the [Red Hat OpenShift Container Platform on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-ocp-95fccffc-ae3b-42df-b6d9-80be5914d852-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjcmVmZXJlbmNlX2FyY2hpdGVjdHVyZQ%3D%3D){: external} deployable architecture. In the {{site.data.keyword.cloud_notm}} console, you can see that the cluster is created and working correctly. When you try to access the Red Hat OpenShift web console on the management cluster, you see this error:

> It is not possible to access the Red Hat OpenShift console because the cluster is accessible only on the management VPC’s private network, which is locked down and not accessible from the internet.

You might also have connectivity issues to the VPC's private networks if you deploy the [VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vpc-9fc0fa64-27af-4fed-9dce-47b3640ba739-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjcmVmZXJlbmNlX2FyY2hpdGVjdHVyZQ%3D%3D){: external}, [VSI on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjcmVmZXJlbmNlX2FyY2hpdGVjdHVyZQ%3D%3D){: external}, or the [Red Hat OpenShift Container Platform on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-ocp-95fccffc-ae3b-42df-b6d9-80be5914d852-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjcmVmZXJlbmNlX2FyY2hpdGVjdHVyZQ%3D%3D){: external} deployable architecture.

For example, you ping the network but it times out:

```bash
ping 10.0.0.1
PING 10.0.0.1 (10.0.0.1): 56 data bytes
ping: sendto: Host is down
Request timeout for icmp_seq 0
ping: sendto: Host is down
Request timeout for icmp_seq 1
ping: sendto: Host is down
Request timeout for icmp_seq 2
ping: sendto: Host is down
Request timeout for icmp_seq 3
^C
--- 10.0.0.1 ping statistics ---
5 packets transmitted, 0 packets received, 100.0% packet loss
```
{: pre}

How can you securely access that private network to complete operations on resources within these VPCs?

### Solution
{: #solution-connect-client-vpn-solution}

Establish secure connections to a private VPC network:

- [Client-to-site VPN server and VPN Client](/docs/vpc?topic=vpc-vpn-client-to-site-overview) - Configure a VPN client application on your device to create a secure connection to your VPC network that uses {{site.data.keyword.cloud_notm}} VPN for VPC. The {{site.data.keyword.cloud_notm}} VPN server service has high availability mode for production use and is managed by {{site.data.keyword.IBM_notm}}.

## Deploying Cloud automation for {{site.data.keyword.client-to-site-vpn}} with projects
{: #deploy-client-to-site-vpn}

1. From the {{site.data.keyword.cloud_notm}} catalog, search for Cloud automation for {{site.data.keyword.client-to-site-vpn}}.
1. Add it to an existing project or create a project.
1. Complete the next steps depending on how you plan to use the deployable architecture:
   - [Configure](/docs/secure-enterprise?topic=secure-enterprise-config-project) it in your project and [deploy](/docs/secure-enterprise?topic=secure-enterprise-deploy-project)
   - You can [stack deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack) together in a project to create a robust end-to-end solution architecture. You don't need to code Terraform to connect the member deployable architectures within the stack. As you configure input values in a member deployable architecture, you can reference inputs or outputs from another member to link the deployable architectures together. After you deploy the deployable architectures in your stack, you can add the stack to a private catalog to easily share it with others in your organization.

A deployable architecture is infrastructure as code (IaC) that's designed for easy deployment, scalability, and modularity. In this case, the deployable architecture represents a repeatable way to create client-to-site VPN connections for more than one landing zone in your org. It also simplifies how others in your company can set up more VPN connections for their landing zones.

## Configure the OpenVPN client
{: #solution-connect-client-vpn-openvpn}
{: step}

After the VPN server cloud resources are deployed, set up the OpenVPN client on devices that will access your landing zone.

1.  Download the OpenVPN profile from the VPN server

    - By using the {{site.data.keyword.cloud_notm}} console:
        1.  Click the **Navigation menu** icon ![Navigation menu icon](../icons/icon_hamburger.svg "Menu"), and then click **VPC Infrastructure** > **VPNs** in the **Network** section to open the VPNs for VPC page.
        1.  Click the **Client-to-site servers** tab and select the client-to-site VPN server that you created.
        1.  Click the **Clients** tab. Then, click **Download client profile**.

      Or

    - By using the {{site.data.keyword.cloud_notm}} CLI:

      ```sh
      ibmcloud is vpn-server-client-configuration VPN_SERVER --file client2site-vpn.ovpn
      ```
      {: pre}

      Look for the `VPN_SERVER` ID in the output of the Terraform apply from the validation step. If you don't find it there, follow the previous steps to download the profile and look in the `<vpn_server>.ovpn` file.
1.  Set up the client:

    You can follow the steps in [Setting up a VPN client](/docs/vpc?topic=vpc-setting-up-vpn-client).
    {: tip}

    1.  Download and install the OpenVPN client application from https://openvpn.net.
    1.  Open the OpenVPN client application, and import the `client2site-vpn.ovpn` file.
    1.  Enter one of the {{site.data.keyword.cloud_notm}} email addresses that was configured to access the VPN as the user ID.
1.  Go to [https://iam.cloud.ibm.com/identity/passcode](https://iam.cloud.ibm.com/identity/passcode) in your browser to generate a passcode. Copy the passcode.
1.  Return to the OpenVPN client application and paste the one-time passcode. Then, import the `client2site-vpn.ovpn` certificate file.

### Using client certificates rather than one-time passcodes
{: #connect-client-vpn-certs}

If you want to configure client certs on the VPN rather than using a one-time-passcode, follow the instructions in the [Managing VPN server and client certifications](/docs/vpc?topic=vpc-client-to-site-authentication#creating-cert-manager-instance-import) section of the client-to-site documentation.

## Test access to the Red Hat OpenShift web console
{: #connect-client-vpn-rh}
{: step}

If your landing zone includes a Red Hat OpenShift cluster, you can now test that you have access to the web console.

1.  Open https://{DomainName}/kubernetes/clusters in your browser.
1.  Select the cluster details for the management cluster in your landing zone.
1.  Click **OpenShift Web Console** in the upper right to access your Red Hat OpenShift web console.
1.  Repeat steps (2) and (3) to test connectivity to the landing zone’s workload cluster.

## Test your VPN connection
{: #connect-client-vpn-connection}
{: step}

On the device that has the OpenVPN client, ping the `10.*` network (which is in your management VPC).

```bash
ping 10.0.0.1
PING 10.0.0.1 (10.0.0.1): 56 data bytes
64 bytes from 10.0.0.1: icmp_seq=0 ttl=64 time=19.920 ms
64 bytes from 10.0.0.1: icmp_seq=1 ttl=64 time=19.301 ms
64 bytes from 10.0.0.1: icmp_seq=2 ttl=64 time=14.490 ms
64 bytes from 10.0.0.1: icmp_seq=3 ttl=64 time=20.896 ms
64 bytes from 10.0.0.1: icmp_seq=4 ttl=64 time=13.938 ms
^C
--- 10.0.0.1 ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 13.938/17.709/20.896/2.904 ms
```

If you see no timeouts or other errors, your local workstation has connectivity to the VPC’s private network.

### Solving connectivity issues
{: #connect-client-vpn-connectivity}

In the following error, OpenVPN has an active connection, but can't reach a server on your private VPN subnet. Check the local network that your device connects through. Some newer routers allocate IP addresses in `10.*` range rather than `192.168.*`.

```text
error: dial tcp: lookup YOUR_SERVER_URL on 10.0.0.1:53:
read udp 10.0.0.2:0->10.0.0.1:53:
i/o timeout- verify you have provided the correct host and port and that the server is currently running.
```
{: screen}

## Summary
{: #connect-client-vpn-summary}

Automating the creation of client-to-site VPN connections to your secure landing zones is straightforward when you use the capabilities of deployable architectures on {{site.data.keyword.cloud_notm}}.
