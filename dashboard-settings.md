---

Copyright:
  years: 2018
lastupdated: "2018-10-17"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Settings
{: #dashboard-settings}

Manage your {{site.data.keyword.databases-for-elasticsearch_full}} service through these available settings.

## Scaling Resources

The _Scale Resources_ panel shows the current size and resource allocation for your deployment. You can manage the available resources for your deployment by adjusting the groups of resources. 

**Storage** - Storage shows the amount of disk space that is allocated to your service. Each member gets an equal share of the allocated space. Your data is replicated across three data members in the Elasticsearch cluster, so the total amount of storage you use is approximately three times the size of your data set.

The minimum storage of an Elasticsearch deployment is 15360 MB, adjustable in step sizes of 3072 MB. This equates to an initial size of 5120 MB per member with 1024 MB increments available.

You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: .tip} 

**Memory** - If you find that your queries and database activity suffer from performance issues due to a lack of memory, you can scale the amount of RAM allocated to your service. Your Elasticsearch deployment runs with three members in a cluster, so the amount of memory you allocate to the service is split between all three members. Adding memory to the total allocation adds memory to the members equally.

The minimum RAM for an Elasticsearch deployment is 3072 MB, adjustable in step sizes of 384 MB.  This equates to an initial allocation of 1028 MB per member with 128 MB increments available.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: .tip}

### Scaling via the UI

Adjust the slider to increase or decrease the resources that are allocated to your service. The slider controls how much memory or disk is allocated per member. The UI currently uses a coarser-grained resolution of 8 GB increments for disk and 1 GB increments for memory. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

### Scaling via the {{site.data.keyword.cloud_notm}} CLI cloud databases plug-in

Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment":  
`ibmcloud cdb deployment-groups example-deployment`

This produces the output:
```
Group   member
Count   3
|
+   Memory
|   Allocation              3072mb
|   Allocation per member   1024mb
|   Minimum                 3072mb
|   Step Size               384mb
|   Adjustable              true
|
+   Disk
|   Allocation              15360mb
|   Allocation per member   5120mb
|   Minimum                 15360mb
|   Step Size               3072mb
|   Adjustable              true
```

The deployment has three members, with 3072 MB of RAM and 15360 MB of disk allocated in total. The "per member" allocation is 1024 MB of RAM and 5120 MB of disk. The minimum value is the lowest the total allocation can be set. The step size is the smallest amount by which the total allocation can be adjusted.

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set, in MB. For example, to scale the memory of the "example-deployment" to 2048 MB of RAM for each memory member (for a total memory of 6144 MB), you use the command:  
`ibmcloud cdb deployment-groups-set example-deployment member --memory 6144`

### Scaling via the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

You can find more examples in the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl).

## Setting the admin Password

{{site.data.keyword.databases-for-elasticsearch}} is provisioned with a default admin user. You can set or change the the admin password of your deployment by using _Update Password_.

A new, randomly generated password appears, or you can type your own password into the field. To regenerate another password, click the icon to the right of the field. When you click *Update Password*, you are asked to confirm before the change is applied. 

**Note:** Changing the password changes the credentials that you or any other services that use the admin user to connect. It can cause these services to disconnect and experience downtime.

## Whitelisting

If you want to restrict access to your databases, you can whitelist specific IP addresses or ranges of IP addresses on your deployment. When no IP addresses are in the whitelist, the whitelist is disabled and the deployment accepts connections from any system on the internet.

Even if not explicitly whitelisted, {{site.data.keyword.cloud_notm}} management services are still able to connect.
{: .tip}

### IP addresses

The *IP* field can take a single complete IPv4 address or IPv6 address with or without a netmask. Without a netmask, incoming connections must come from exactly that IP address. 

Although the *IP* field allows for IPv6, no deployments are currently available to IPv6 networking and so these addresses cannot be filtered on.
{: tip}

### Netmasks

To allow a connection from a specified range of IP addresses, use a netmask. The IP address must be fully specified. That means entering, for example, 192.168.1.0/24 rather than 192.168.1/24.

### Description

The *Description* can be any user-significant text for identifying the whitelist entry - a customer name, project identifier, or employee number, for example. The description field is required.

### Removal

To remove an IP address or netmask from the Whitelist, click *Remove*.
When all entries on the whitelist are removed, the whitelist is disabled and all IP addresses are accepted by the TCP access portals.