---

copyright:
   years: 2023
lastupdated: "2024-05-23"

keywords:
subcollection: secure-infrastructure-vpc
content-type: tutorial
services: vpc, security-groups
account-plan: paid
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}

# Exposing an application in your deployable architecture to the internet
{: #access-public-app}
{: toc-content-type="tutorial"}
{: toc-services="vpc, security-groups"}
{: toc-completion-time="1h"}

In this tutorial, you use a public {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} to allow access over the public internet to an app that runs on your VSI on VPC landing zone deployable architecture.
{: shortdesc}

The load balancer can distribute traffic among multiple application server instances that are running in the VPC (in the workload VSIs). It forwards traffic only to instances that respond correctly to periodic health checks. For more information about load balancing, see the [overview](/docs/vpc?topic=vpc-nlb-vs-elb) to Load balancers for VPC.



## Before you begin
{: #prereqs-public-app}

- Deploy an instance of a VSI on VPC landing zone deployable architecture. For more information, see [Deploying a landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-deploy).
- Create a web application on a workload VSI in your deployable architecture.

## Create a load balancer
{: #alb-public-app}
{: step}

Create a public {{site.data.keyword.alb_full}}.

1.  In the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, click the Menu icon ![Menu icon](../icons/icon_hamburger.svg), and then click **VPC Infrastructure > Load balancers**.
1.  On the Load balancers page, click **Create**.
1.  Specify the settings for your load balancer:
    - Load balancer type: **Application Load Balancer (ALB)**
    - Location: Select the geography and region where you provisioned your VPC resources
    - Details:
        - Name: `<your_prefix>-web-lb`, where `<your_prefix>` is any string of lowercase alphanumeric characters and hyphens.
        - Resource group: Select or create a group. For example, `<your_prefix>-workload-rg`.
        - Virtual private cloud: Select your VPC.
        - Type: **Public**.
        - DNS type: **Public**.
        - Subnets: The VSI that is running your application. For example, `<your_prefix>-workload-vsi-zone-1`.
    - Backend pool:

        Click **Create pool** and specify the following information to create a back-end pool.
        - Name: `<your_prefix>-backend-pool`.
        - Pool protocol: HTTP
        - Session stickiness: Select whether all requests during a user's session are sent to the same instance.
        - Proxy Protocol: **Disabled**.
        - Method: Select how you want the load balancer to distribute traffic across the instances in the pool. If you don't have other requirements, select **Round robin**.
        - Health Check:
              - Health Port: `80`
              - Use the default settings for all other options.
1.  Click Create to create the back-end pool.
1.  Click **Attach server** in the Server instances column of the Back-end pools table.

1.  Select the VPC devices tab
    1.  Add the VSI that is in the subnet the VSI that is running your application (for example, `<your_prefix>-workload-vsi-zone-1`).
    1.  Select an instance. If an instance has multiple interfaces, make sure that you select the correct IP address.
    1.  Specify port `80`.

    You can assign multiple VSIs here if you want to distribute the load.
1.  In the Front-end listeners section, click **Create listener**.
    1.  Set the listener port to `80`. Use the default settings for all other options.
    1.  Click Create to create the front-end listener.
1.  In the security groups section, clear all settings except the one labeled `<your_prefix>-workload`.
1.  After you finish creating pools and listeners, click **Create load balancer**.

## Update security to allow external traffic
{: #security-public-app}
{: step}

To allow access to your load balancer, complete the following steps:

1.  Click the Menu icon ![Menu icon](../icons/icon_hamburger.svg), and then click **VPC Infrastructure > Security groups**.
1.  Find the `<your_prefix>-workload` security group that you want to attach your load balancer to.
1.  Add the following inbound rule to that security group:
    - Protocol: `TCP`
    - Port: Port Range:
        - Min: `80`
        - Max: `80`
    - Source Type: `Any`

To allow internet access to the load balancer, complete the following steps. For more information, see [Creating a network ACL](/docs/vpc?topic=vpc-acl-create-ui&interface=ui).

1.  Click the Menu icon ![Menu icon](../icons/icon_hamburger.svg), and then click **VPC Infrastructure > Access control lists.
1.  Find the ACL named `<your-prefix>-workload-acl`.
1.  Create an inbound rule with the following settings:
    - Allow or deny: `Allow`
    - Protocol: `TCP`
    - Source:
        - Type: `Any`
        - Port: `Any`
    - Destination:
        - Type: `IP or CIDR`: `10.40.10.0/24`
        - Port: `Port range`
            - Port min: `80`
            - Port max: `80`
    - Priority: `Set to top`
1.  Create an outbound rule with the following settings:
    - Allow or deny: Allow
    - Protocol: `TCP`
    - Source:
        - Type: `IP or CIDR`: `10.40.10.0/24`
        - Port: `Port range`
            - Port min: `80`
            - Port max: `80`
    - Destination:
        - Type: `Any`
        - Port: `Any`
    - Priority: `Set to top`

It can take several minutes for your load balancer to finish provisioning. Wait until the status is `Active` in Load balancers for VPC in the console. You might need to refresh the page periodically.
{: tip}

## Verify internet access to your server
{: #check-public-app}
{: step}

Now that your load balancer is configured, verify that it routes traffic to your app.

1.  Retrieve the fully qualified domain name of your load balancer:
    1.  Click the Menu icon ![Menu icon](../icons/icon_hamburger.svg), and then click **VPC Infrastructure > Load balancers**.
    1.  Select your load balancer.
    1.  Copy the value of the **Hostname**.
1.  Paste the hostname in a browser and check whether your app responds.

You can also test connectivity by issuing the curl command `curl http://<value of the Hostname>`.
{: tip}

### Solving connectivity issues
{: #tshoot-vpn-public-app}

If you have connectivity issues through your load balancer, check out the troubleshooting topics in the VPN docs. For example, [Why is traffic not reaching my back-end members?](/docs/vpc?topic=vpc-troubleshoot-lb-traffic-member&interface=ui)

## Summary
{: #summary-public-app}

You configured your VSI on VPC landing zone deployable architecture that hosts a web application to allow traffic from the public internet through the {{site.data.keyword.alb_full}}. Your app is now accessible from any browser on the public internet through the fully qualified domain name of the load balancer.

## Next steps
{: #next-steps-public-app}

Learn more about how you can further extend your deployable architecture.

- Learn how to [share your deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-share-custom).
- Read about the [global load balancing](/docs/cis?topic=cis-configure-glb#glb-details) capabilities with {{site.data.keyword.cis_full_notm}} (CIS).


