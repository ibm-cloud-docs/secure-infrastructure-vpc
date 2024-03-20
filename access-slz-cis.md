---

copyright:
   years: 2023, 2024
lastupdated: "2024-03-19"

keywords:
subcollection: secure-infrastructure-vpc
content-type: tutorial
services: vpc, cis, secrets-manager, vsi
account-plan: paid
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}

# Configuring access to an application through {{site.data.keyword.cis_short}}
{: #access-cis-app}
{: toc-content-type="tutorial"}
{: toc-services="vpc, cis, secrets-manager, vsi"}
{: toc-completion-time="1h"}

In this tutorial, you use {{site.data.keyword.cis_full_notm}} ({{site.data.keyword.cis_short_notm}}) to access an application that is deployed on a VSI in a VPC landing zone deployable architecture over the internet. You set up the required infrastructure and learn how to configure {{site.data.keyword.cis_short_notm}} to access the application that is deployed in the deployable architecture.
{: shortdesc}

{{site.data.keyword.cis_short_notm}} provides the global load balancer, caching, web application firewall, and page rules to secure your applications and enhance their reliability and performance.

## Objective
{: #goals-cis-app}

- Assign the necessary access for an operator to access the VPC environment, including deploying application on the VSIs located in the workload VPC.
- Expose the application to the internet through a VPC load balancer so that anyone can access the apps. The load balancer can distribute traffic among multiple application server instances that are running in the VPC (in the workload VSIs), and by forwarding traffic only to instances that are working correctly.
- Use {{site.data.keyword.secrets-manager_full_notm}} to manage the Transport Layer Security (TLS) certificate for all incoming HTTPS requests.
- Configure {{site.data.keyword.cis_short}} to access your application. {{site.data.keyword.cis_full_notm}}, powered by Cloudflare, provides a fast, highly performant, reliable, and secure internet service on {{site.data.keyword.cloud_notm}}.
- Use a global load balancer between regions to implement high availability, increase resiliency and reduce latency.

## Before you begin
{: #prereqs-cis-app}

- Deploy an instance of a VPC landing zone deployable architecture. For more information, see [Deploying a landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-deploy).
- Make sure that your  landing zone deployable architecture has a working virtual server (VSI).
- Create an instance of {{site.data.keyword.cis_short}} and configure it with an active domain name. You can use the {{site.data.keyword.cis_short_notm}} [Terraform module](https://github.com/terraform-ibm-modules/terraform-ibm-cis/) to create and configure the instance.

## Assign operator access
{: #dns-cis-app}
{: step}

Because network access to the VPC landing zone topology is locked down, you need to assign operator access to the VSIs through the management VPC.

1.  Review the [options](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-connect-landingzone-site-vpn#solution-connect-site-vpn-objectives) to assign operator access.
1.  Complete one of the following approaches:

    - The [client-to-site VPN](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-connect-landingzone-client-vpn)
    - The [site-to-site VPN](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-connect-landingzone-site-vpn)

In this tutorial, you expose one of the VSIs in the management VPC as a "jump-box". You access the workload VPC and deploy your application in the workload VSIs located in the workload VPC.

Assign operator access to every VSI where you are deploying your application.

## Deploy your application in the workload VSI
{: #deploy-cis-app}
{: step}

1.  Access the workload VSI (for example <your_prefix>-workload-server-001) through the management VPC.

    1.  Access the ACL of workload VPC and create an [inbound and outbound rule](/docs/vpc?topic=vpc-acl-create-ui&interface=ui) to enable SSH communication (port 22) for management VPC.
    1.  Similarly, create an inbound and outbound rule to enable SSH in the ACL of management VPC for the workload VPC.
    1.  Copy the SSH key (used to log in to management VSI) from your system to the management server.
    1.  Log in to the management server and do SSH to the workload VSI using its reserved IP and the SSH key that you copied earlier.
    1.  Verify that you are logged in to the workload VSI.
1.  Deploy the application.

    For more information about deploying the application, see [Install and configure a web server on the virtual server instances](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region#vpc-multi-region-install-configure-web-server-vsis) in Deploy isolated workloads across multiple locations and zones. That tutorial refers to nginx, but you deploy an Apache server on the workload VSI on port `80`.

    1.  Run the following commands to install the Apache server:

        ```sh
        apt-get update
        ```
        {: pre}

        ```sh
        apt-get install apache2 --yes
        ```
        {: pre}

    1.  Run the following command to verify the installation:

        ```sh
        curl http://localhost:80
        ```
        {: pre}

1.  Repeat these steps to deploy the application on other workload VSIs.

## Create certificates in {{site.data.keyword.secrets-manager_short}}
{: #create-cert-cis-app}
{: step}

1.  {{site.data.keyword.secrets-manager_full_notm}} is used to order or import, and then manage, the certificate for your domain. Follow the steps in [Secure with HTTPS](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region#vpc-multi-region-6) to create and authorize a {{site.data.keyword.secrets-manager_short}} instance.
1.  Add the certificate authority and DNS provider:

    1.  Open the {{site.data.keyword.secrets-manager_short}} service and select `Public Certificates` under Secrets engines.
    1.  Configure your certificate authority with the **Let's Encrypt** certificate authority engine.
    1.  Under **DNS provider**, select your {{site.data.keyword.cis_short_notm}} instance.
1.  Order a public certificate in {{site.data.keyword.secrets-manager_short}}, as explained in [Alternative 1: Proxy, traffic flows through {{site.data.keyword.cis_short_notm}}](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region#vpc-multi-region-15).

## Attach the public VPC load balancer
{: #attach-lb-cis-app}
{: step}

Now that your application is deployed on the workload VSIs, configure the load balancer to allow access to the app over the public internet.

1.  Create a public Application Load Balancer for VPC.

    1.  Follow this [tutorial](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-access-public-app) to attach a public VPC load balancer.
      - For the Front-end listener, use Listener protocol `HTTPS`, port `443`, and attach the {{site.data.keyword.secrets-manager_short}} instance and the public-certificates that you created in the previous step.
      - Follow the steps in [Update security to allow external traffic](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-access-public-app#security-public-app) for port `80` and port `443`.
2.  Verify access.

    1.  Retrieve the hostname of your load balancer.
        1.  Go to **VPC Infrastructure > Load Balancers**
        2.  Select your load balancer
    1.  Copy the value of **Hostname**
    1.  Paste `https://<hostname>` in a browser and check whether your app responds.

## Configure a DNS record in the {{site.data.keyword.cis_short_notm}} instance
{: #configure-dns-cis-app}
{: step}

Create a DNS CNAME record to allow clients to look up your domain (for example, `example.com`) and resolve `xxx-REGION.lb.appdomain.cloud`.

1.  In {{site.data.keyword.cis_short_notm}}, open the **Reliability** page and choose **DNS**.
1.  Scroll to DNS Records and create a record.

    - Type: **CNAME**
    - Name: **vpc-lb**
    - TTL: **Automatic**
    - Alias Domain Name: **VPC load balancer Hostname**.
    - Add a DNS CNAME record for **vpc-lb**

1.  Repeat these steps to add a DNS record for the other region.

## Access the application over HTTPS through {{site.data.keyword.cis_short_notm}}
{: #validate-cis-app}
{: step}

To verify that the application is available over the internet, go to `https://vpc-lb.<your_domain>` (for example `https://vpc-lb.example.com`) in a browser.

## Create the global load balancer
{: #create-glb-cis-app}
{: step}

In this section, you configure {{site.data.keyword.cis_short_notm}} to distribute the load between regions. You configure the global load balancer in the {{site.data.keyword.cis_short_notm}} instance to use the new CNAME records.

Before you create the global load balancer, you configure a health check to validate the availability of your application and define origin pools that point to the VPC load balancers.

### Configure Health Check
{: #health-check-cis-app}

A health check helps gain insight into the availability of pools so that traffic can be routed to the healthy ones. These checks periodically send HTTPS requests and monitor the responses.

1.  In the {{site.data.keyword.cis_short_notm}} dashboard, go to **Reliability** > **Global Load Balancers**.
1.  Select **Health checks** and click **Create**.
1.  Set **Name** to `vpc-lb`.
1.  Select `HTTPS` for **Monitor Type**.
1.  Leave **Port** set to the default.
1.  Set **Path** to `/`.
1.  Click **Create**.

### Define origin pools
{: #define-pools-cis-app}

A pool is a group of origin VSIs or load balancers that traffic is intelligently routed to when attached to a global load balancer. You can define location-based pools and configure {{site.data.keyword.cis_short_notm}} to redirect users to the closest VPC load balancer based on the geographical location of the user requests.

1.  Select **Origin pools** and click **Create**.
2.  Enter a name for the pool: `vpc-lb`.
3.  Set **Origin Name** to `vpc-lb`.
4.  Set **Origin Address** to the new CNAME records that you specified earlier.
5.  Select **Existing health check** and select the health check you just created.
6.  Select a **Health check region** that is close to your location.
7.  Click **Save**.
8.  Repeat these steps for other regions.

Wait for few seconds until the health status of your origin pool displays `healthy`.

### Configure the global load balancer
{: #config-glb-cis-app}

Now that the origin pools are defined, you can complete the configuration of the load balancer.

1.  Select **Load balancers** and click **Create**.
1.  Keep the default values for **Enable** (`On`) and **Proxy** (`Off`).
1.  Specify the name for the global load balancer as `vpc-lb-glb`. This name forms the initial characters in the subdomain to access your application (for example, https://`vpc-lb-glb`.`<your_domain>`).
1.  Set **TTL**, **Traffic Steering**, **Session affinity** to settings that work for your application.
1.  Click **Add route**.
1.  Select the **Region**: `Default`.
1.  Select the origin pool that you just created.
1.  Click **Add**.
1.  Click **Create**.
1.  Access `https://vpc-lb-glb.<your_domain>`  (for example, `example.com`) in a browser to verify that the global load balancer is available.

## Expand the tutorial
{: #next-steps-cis-app}

You accessed your workload that is deployed in the landing zone through {{site.data.keyword.cis_short_notm}}.

You can configure [more features](/docs/cis?topic=cis-manage-your-ibm-cis-for-optimal-security) such as WAF, DDoS in {{site.data.keyword.cis_short_notm}} to make it more secure. For more information, read the following topics:

- [Managing {{site.data.keyword.cis_short_notm}} for optimal security](/docs/cis?topic=cis-manage-your-ibm-cis-for-optimal-security)
- [Deploy and configure {{site.data.keyword.cis_short_notm}} using terraform](https://github.com/terraform-ibm-modules/terraform-ibm-cis/)

<!-- static website DA doesn't exist in production. Breaking the link so that it doesn't show up in the link checker report

- [Deploying architecture configurations from a project](/docs/secure-enterprise?topic=secure-enterprise-project-getting-started)

-->
