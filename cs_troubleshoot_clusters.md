---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Troubleshooting clusters and worker nodes
{: #cs_troubleshoot_clusters}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting your clusters and worker nodes.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}

## Unable to create a cluster due to permission errors
{: #cs_credentials}

{: tsSymptoms}
When you create a new Kubernetes cluster, you receive an error message similar to one of the following.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a
Pay-As-You-Go account that is linked to an IBM Cloud infrastructure (SoftLayer)
account term or that you have used the {{site.data.keyword.containerlong_notm}}
CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.Bluemix_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
The cluster could not be configured with the registry. Make sure that you have the Administrator role for {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
You do not have the correct permissions to create a cluster. You need the following permissions to create a cluster:
*  **Super User** role for IBM Cloud infrastructure (SoftLayer).
*  **Administrator** platform management role for {{site.data.keyword.containerlong_notm}} at the account level.
*  **Administrator** platform management role for {{site.data.keyword.registrylong_notm}} at the account level. Do not limit policies for {{site.data.keyword.registryshort_notm}} to the resource group level. If you started to use {{site.data.keyword.registrylong_notm}} before 4 October 2018, ensure that you [enable {{site.data.keyword.Bluemix_notm}} IAM policy enforcement](/docs/services/Registry/registry_users.html#existing_users).

For infrastructure-related errors, {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go accounts that were created after automatic account linking was enabled are already set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio. You can purchase infrastructure resources for your cluster without additional configuration. If you have a valid Pay-As-You-Go account and receive this error message, you might not be using the correct IBM Cloud infrastructure (SoftLayer) account credentials to access infrastructure resources.

Users with other {{site.data.keyword.Bluemix_notm}} account types must configure their accounts to create standard clusters. Examples of when you might have a different account type are:
* You have an existing IBM Cloud infrastructure (SoftLayer) account that predates your {{site.data.keyword.Bluemix_notm}} platform account and want to continue to use it.
* You want to use a different IBM Cloud infrastructure (SoftLayer) account to provision infrastructure resources in. For example, you might set up a team {{site.data.keyword.Bluemix_notm}} account to use a different infrastructure account for billing purposes.

If you use a different IBM Cloud infrastructure (SoftLayer) account to provision infrastructure resources, you might also have [orphaned clusters](#orphaned) in your account.

{: tsResolve}
The account owner must set up the infrastructure account credentials properly. The credentials depend on what type of infrastructure account you are using.

1.  Verify that you have access to an infrastructure account. Log in to the [{{site.data.keyword.Bluemix_notm}} console![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/) and from the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon"), click **Infrastructure**. If you see the infrastructure dashboard, you have access to an infrastructure account.
2.  Check if your cluster uses a different infrastructure account than the one that comes with your Pay-As-You-Go account.
    1.  From the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon"), click **Containers > Clusters**.
    2.  From the table, select your cluster.
    3.  In the **Overview** tab, check for an **Infrastructure User** field.
        * If you do not see the **Infrastructure User** field, you have a linked Pay-As-You-Go account that uses the same credentials for your infrastructure and platform accounts.
        * If you see an **Infrastructure User** field, your cluster uses a different infrastructure account than the one that came with your Pay-As-You-Go account. These different credentials apply to all clusters within the region.
3.  Decide what type of account you want to have to determine how to troubleshoot your infrastructure permission issue. For most users, the default linked Pay-As-You-Go account is sufficient.
    *  Linked Pay-As-You-Go {{site.data.keyword.Bluemix_notm}} account: [Verify that the API key is set up with the correct permissions](cs_users.html#default_account). If your cluster is using a different infrastructure account, you must unset those credentials as part of the process.
    *  Different {{site.data.keyword.Bluemix_notm}} platform and infrastructure accounts: Verify that you can access the infrastructure portfolio and that [the infrastructure account credentials are set up with the correct permissions](cs_users.html#credentials).
4.  If you cannot see the cluster's worker nodes in your infrastructure account, you might check whether the [cluster is orphaned](#orphaned).

<br />


## Firewall prevents running CLI commands
{: #ts_firewall_clis}

{: tsSymptoms}
When you run `ibmcloud`, `kubectl`, or `calicoctl` commands from the CLI, they fail.

{: tsCauses}
You might have corporate network policies that prevent access from your local system to public endpoints via proxies or firewalls.

{: tsResolve}
[Allow TCP access for the CLI commands to work](cs_firewall.html#firewall). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).


## Firewall prevents cluster from connecting to resources
{: #cs_firewall}

{: tsSymptoms}
When the worker nodes cannot connect, you might see various different symptoms. You might see one of the following messages when kubectl proxy fails or you try to access a service in your cluster and the connection fails.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

If you run kubectl exec, attach, or logs, you might see the following message.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

If kubectl proxy succeeds, but the dashboard is not available, you might see the following message.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
You might have another firewall set up or customized your existing firewall settings in your IBM Cloud infrastructure (SoftLayer) account. {{site.data.keyword.containerlong_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. Another reason might be that the worker nodes are stuck in a reloading loop.

{: tsResolve}
[Allow the cluster to access infrastructure resources and other services](cs_firewall.html#firewall_outbound). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

<br />



## Unable to view or work with a cluster
{: #cs_cluster_access}

{: tsSymptoms}
* You are not able to find a cluster. When you run `ibmcloud ks clusters`, the cluster is not listed in the output.
* You are not able to work with a cluster. When you run `ibmcloud ks cluster-config` or other cluster-specific commands, the cluster is not found.


{: tsCauses}
In {{site.data.keyword.Bluemix_notm}}, each resource must be in a resource group. For example, cluster `mycluster` might exist in the `default` resource group. When the account owner gives you access to resources by assigning you an {{site.data.keyword.Bluemix_notm}} IAM platform role, the access can be to a specific resource or to the resource group. When you are given access to a specific resource, you don't have access to the resource group. In this case, you don't need to target a resource group to work with the clusters you have access to. If you target a different resource group than the group that the cluster is in, actions against that cluster can fail. Conversely, when you are given access to a resource as part of your access to a resource group, you must target a resource group to work with a cluster in that group. If you don't target your CLI session to the resource group that the cluster is in, actions against that cluster can fail.

If you cannot find or work with a cluster, you might be experiencing one of the following issues:
* You have access to the cluster and the resource group that the cluster is in, but your CLI session is not targeted to the resource group that the cluster is in.
* You have access to the cluster, but not as part of the resource group that the cluster is in. Your CLI session is targeted to this or another resource group.
* You don't have access to the cluster.

{: tsResolve}
To check your user access permissions:

1. List all of your user permissions.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Check if you have access to the cluster and to the resource group that the cluster is in.
    1. Look for a policy that has a **Resource Group Name** value of the cluster's resource group and a **Memo** value of `Policy applies to the resource group`. If you have this policy, you have access to the resource group. For example, this policy indicates that a user has access to the `test-rg` resource group:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Look for a policy that has a **Resource Group Name** value of the cluster's resource group, a **Service Name** value of `containers-kubernetes` or no value, and a **Memo** value of `Policy applies to the resource(s) within the resource group`. If you this policy, you have access to clusters or to all resources within the resource group. For example, this policy indicates that a user has access to clusters in the `test-rg` resource group:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. If you have both of these policies, skip to Step 4, first bullet. If you don't have the policy from Step 2a, but you do have the policy from Step 2b, skip to Step 4, second bullet. If you do not have either of these policies, continue to Step 3.

3. Check if you have access to the cluster, but not as part of access to the resource group that the cluster is in.
    1. Look for a policy that has no values besides the **Policy ID** and **Roles** fields. If you have this policy, you have access to the cluster as part of access to the entire account. For example, this policy indicates that a user has access to all resources in the account:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Look for a policy that has a **Service Name** value of `containers-kubernetes` and a **Service Instance** value of the cluster's ID. You can find a cluster ID by running `ibmcloud ks cluster-get <cluster_name>`. For example, this policy indicates that a user has access to a specific cluster:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. If you have either of these policies, skip to the second bullet point of step 4. If you do not have either of these policies, skip to the third bullet point of step 4.

4. Depending on your access policies, choose one of the following options.
    * If you have access to the cluster and to the resource group that the cluster is in:
      1. Target the resource group. **Note**: You can't work with clusters in other resource groups until you untarget this resource group.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Target the cluster.
          ```
          ibmcloud ks cluster-config <cluster_name_or_ID>
          ```
          {: pre}

    * If you have access to the cluster but not to the resource group that the cluster is in:
      1. Do not target a resource group. If you already targeted a resource group, untarget it:
        ```
        ibmcloud target -g none
        ```
        {: pre}
        **Note**: This command fails because no resource group that is named `none` exists. However, the current resource group is automatically untargeted when the command fails.

      2. Target the cluster.
        ```
        ibmcloud ks cluster-config <cluster_name_or_ID>
        ```
        {: pre}

    * If you do not have access to the cluster:
        1. Ask your account owner to assign an [{{site.data.keyword.Bluemix_notm}} IAM platform role](cs_users.html#platform) to you for that cluster.
        2. Do not target a resource group. If you already targeted a resource group, untarget it:
          ```
          ibmcloud target -g none
          ```
          {: pre}
          **Note**: This command fails because no resource group that is named `none` exists. However, the current resource group is automatically untargeted when the command fails.
        3. Target the cluster.
          ```
          ibmcloud ks cluster-config <cluster_name_or_ID>
          ```
          {: pre}

<br />


## Accessing your worker node with SSH fails
{: #cs_ssh_worker}

{: tsSymptoms}
You cannot access your worker node by using an SSH connection.

{: tsCauses}
SSH by password is unavailable on the worker nodes.

{: tsResolve}
Use a Kubernetes [`DaemonSet` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) for actions that you must run on every node, or use jobs for one-time actions that you must run.

<br />


## Bare metal instance ID is inconsistent with worker records
{: #bm_machine_id}

{: tsSymptoms}
When you use `ibmcloud ks worker` commands with your bare metal worker node, you see a message similar to the following.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
The machine ID can become inconsistent with the {{site.data.keyword.containerlong_notm}} worker record when the machine experiences hardware issues. When IBM Cloud infrastructure (SoftLayer) resolves this issue, a component can change within the system that the service does not identify.

{: tsResolve}
For {{site.data.keyword.containerlong_notm}} to re-identify the machine, [reload the bare metal worker node](cs_cli_reference.html#cs_worker_reload). **Note**: Reloading also updates the machine's [patch version](cs_versions_changelog.html).

You can also [delete the bare metal worker node](cs_cli_reference.html#cs_cluster_rm). **Note**: Bare metal instances are billed monthly.

<br />


## Unable to modify or delete infrastructure in an orphaned cluster
{: #orphaned}

{: tsSymptoms}
You cannot perform infrastructure-related commands on your cluster, such as:
* Adding or removing worker nodes
* Reloading or rebooting worker nodes
* Resizing worker pools
* Updating your cluster

You cannot view the cluster worker nodes in your IBM Cloud infrastructure (SoftLayer) account. However, you can update and manage other clusters in the account.

Further, you verified that you have the [proper infrastructure credentials](#cs_credentials).

{: tsCauses}
The cluster might be provisioned in an IBM Cloud infrastructure (SoftLayer) account that is no longer linked to your {{site.data.keyword.containerlong_notm}} account. The cluster is orphaned. Because the resources are in a different account, you do not have the infrastructure credentials to modify the resources.

Consider the following scenario to understand how clusters might become orphaned.
1.  You have an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account.
2.  You create a cluster named `Cluster1`. The worker nodes and other infrastructure resources are provisioned into the infrastructure account that comes with your Pay-As-You-Go account.
3.  Later, you find out that your team uses a legacy or shared IBM Cloud infrastructure (SoftLayer) account. You use the `ibmcloud ks credential-set` command to change the IBM Cloud infrastructure (SoftLayer) credentials to use your team account.
4.  You create another cluster named `Cluster2`. The worker nodes and other infrastructure resources are provisioned into the team infrastructure account.
5.  You notice that `Cluster1` needs a worker node update, a worker node reload, or you just want to clean it up by deleting it. However, because `Cluster1` was provisioned into a different infrastructure account, you cannot modify its infrastructure resources. `Cluster1` is orphaned.
6.  You follow the resolution steps in the following section, but do not set your infrastructure credentials back to your team account. You can delete `Cluster1`, but now `Cluster2` is orphaned.
7.  You change your infrastructure credentials back to the team account that created `Cluster2`. Now, you no longer have an orphaned cluster!

<br>

{: tsResolve}
1.  Check which infrastructure account the region that your cluster is in currently uses to provision clusters.
    1.  Log in to the [{{site.data.keyword.containerlong_notm}} clusters console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/containers-kubernetes/clusters).
    2.  From the table, select your cluster.
    3.  In the **Overview** tab, check for an **Infrastructure User** field. This field helps you determine if your {{site.data.keyword.containerlong_notm}} account uses a different infrastructure account than the default.
        * If you do not see the **Infrastructure User** field, you have a linked Pay-As-You-Go account that uses the same credentials for your infrastructure and platform accounts. The cluster that cannot be modified might be provisioned in a different infrastructure account.
        * If you see an **Infrastructure User** field, you use a different infrastructure account than the one that came with your Pay-As-You-Go account. These different credentials apply to all clusters within the region. The cluster that cannot be modified might be provisioned in your Pay-As-You-Go or a different infrastructure account.
2.  Check which infrastructure account was used to provision the cluster.
    1.  In the **Worker Nodes** tab, select a worker node and note its **ID**.
    2.  Open the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon") and click **Infrastructure**.
    3.  From the infrastructure navigation pane, click **Devices > Device List**.
    4.  Search for the worker node ID that you previously noted.
    5.  If you do not find the worker node ID, the worker node is not provisioned into this infrastructure account. Switch to a different infrastructure account and try again.
3.  Use the `ibmcloud ks credential-set` [command](cs_cli_reference.html#cs_credentials_set) to change your infrastructure credentials to the account that the cluster worker nodes are provisioned in, which you found in the previous step.
    **Note**: If you no longer have access to and cannot get the infrastructure credentials, you must open an {{site.data.keyword.Bluemix_notm}} support case to remove the orphaned cluster.
4.  [Delete the cluster](cs_clusters.html#remove).
5.  If you want, reset the infrastructure credentials to the previous account. Note that if you created clusters with a different infrastructure account than the account that you switch to, you might orphan those clusters.
    * To set credentials to a different infrastructure account, use the `ibmcloud ks credential-set` [command](cs_cli_reference.html#cs_credentials_set).
    * To use the default credentials that come with your {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account, use the `ibmcloud ks credential-unset` [command](cs_cli_reference.html#cs_credentials_unset).

<br />


## `kubectl` commands time out
{: #exec_logs_fail}

{: tsSymptoms}
If you run commands such as `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward`, or `kubectl logs`, you see the following message.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
The OpenVPN connection between the master node and worker nodes is not functioning properly.

{: tsResolve}
1. If you have multiple VLANs for a cluster, multiple subnets on the same VLAN, or a multizone cluster, you must enable [VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning) for your IBM Cloud infrastructure (SoftLayer) account so your worker nodes can communicate with each other on the private network. To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](cs_users.html#infra_access), or you can request the account owner to enable it. To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan-spanning-get` [command](/docs/containers/cs_cli_reference.html#cs_vlan_spanning_get). If you are using {{site.data.keyword.BluDirectLink}}, you must instead use a [Virtual Router Function (VRF)](/docs/infrastructure/direct-link/subnet-configuration.html#more-about-using-vrf). To enable VRF, contact your IBM Cloud infrastructure (SoftLayer) account representative.
2. Restart the OpenVPN client pod.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. If you still see the same error message, then the worker node that the VPN pod is on might be unhealthy. To restart the VPN pod and reschedule it to a different worker node, [cordon, drain, and reboot the worker node](cs_cli_reference.html#cs_worker_reboot) that the VPN pod is on.

<br />


## Binding a service to a cluster results in same name error
{: #cs_duplicate_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, you see the following message.

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Multiple service instances might have the same name in different regions.

{: tsResolve}
Use the service GUID instead of the service instance name in the `ibmcloud ks cluster-service-bind` command.

1. [Log in to the region that includes the service instance to bind.](cs_regions.html#bluemix_regions)

2. Get the GUID for the service instance.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Output:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Bind the service to the cluster again.
  ```
  ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## Binding a service to a cluster results in service not found error
{: #cs_not_found_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, you see the following message.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
To bind services to a cluster, you must have the Cloud Foundry developer user role for the space where the service instance is provisioned. In addition, you must have the {{site.data.keyword.Bluemix_notm}} IAM Editor platform access to {{site.data.keyword.containerlong}}. To access the service instance, you must be logged in to the space where the service instance is provisioned.

{: tsResolve}

**As the user:**

1. Log in to {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Target the org and the space where the service instance is provisioned.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Verify that you are in the right space by listing your service instances.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Try binding the service again. If you get the same error, then contact the account administrator and verify that you have sufficient permissions to bind services (see the following account admin steps).

**As the account admin:**

1. Verify that the user who experiences this problem has [Editor permissions for {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access).

2. Verify that the user who experiences this problem has the [Cloud Foundry developer role for the space](/docs/iam/mngcf.html#updating-cloud-foundry-access) where the service is provisioned.

3. If the correct permissions exists, try assigning a different permission and then re-assigning the required permission.

4. Wait a few minutes, then let the user try to bind the service again.

5. If this does not resolve the problem, then the {{site.data.keyword.Bluemix_notm}} IAM permissions are out of sync and you cannot resolve the issue yourself. [Contact IBM support](/docs/get-support/howtogetsupport.html#getting-customer-support) by opening a support case. Make sure to provide the cluster ID, the user ID, and the service instance ID.
   1. Retrieve the cluster ID.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Retrieve the service instance ID.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## Binding a service to a cluster results in service does not support service keys error
{: #cs_service_keys}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, you see the following message.

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
Some services in {{site.data.keyword.Bluemix_notm}}, such as {{site.data.keyword.keymanagementservicelong}} do not support the creation of service credentials, also referred to as service keys. Without the support of service keys, the service is not bindable to a cluster. To find a list of services that support the creation of service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/apps/reqnsi.html#accser_external).

{: tsResolve}
To integrate services that do not support service keys, check if the service provides an API that you can use to access the service directly from your app. For example, if you want to use {{site.data.keyword.keymanagementservicelong}}, see the [API reference ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/apidocs/kms?language=curl).

<br />


## After a worker node updates or reloads, duplicate nodes and pods appear
{: #cs_duplicate_nodes}

{: tsSymptoms}
When you run `kubectl get nodes`, you see duplicate worker nodes with the status **NotReady**. The worker nodes with **NotReady** have public IP addresses, while the worker nodes with **Ready** have private IP addresses.

{: tsCauses}
Older clusters listed worker nodes by the cluster's public IP address. Now, worker nodes are listed by the cluster's private IP address. When you reload or update a node, the IP address is changed, but the reference to the public IP address remains.

{: tsResolve}
Service is not disrupted due to these duplicates, but you can remove the old worker node references from the API server.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Accessing a pod on a new worker node fails with a timeout
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
You deleted a worker node in your cluster and then added a worker node. When you deployed a pod or Kubernetes service, the resource cannot access the newly created worker node, and the connection times out.

{: tsCauses}
If you delete a worker node from your cluster and then add a worker node, the new worker node might be assigned the private IP address of the deleted worker node. Calico uses this private IP address as a tag and continues to try to reach the deleted node.

{: tsResolve}
Manually update the reference of the private IP address to point to the correct node.

1.  Confirm that you have two worker nodes with the same **Private IP** address. Note the **Private IP** and **ID** of the deleted worker.

  ```
  ibmcloud ks workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.10.8
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.10.8
  ```
  {: screen}

2.  Install the [Calico CLI](cs_network_policy.html#adding_network_policies).
3.  List the available worker nodes in Calico. Replace <path_to_file> with the local path to the Calico configuration file.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Delete the duplicate worker node in Calico. Replace NODE_ID with the worker node ID.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reboot the worker node that was not deleted.

  ```
  ibmcloud ks worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


The deleted node is no longer listed in Calico.

<br />




## Pods fail to deploy because of a pod security policy
{: #cs_psp}

{: tsSymptoms}
After creating a pod or running `kubectl get events` to check on a pod deployment, you see an error message similar to the following.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[The `PodSecurityPolicy` admission controller](cs_psp.html) checks the authorization of the user or service account, such as a deployment or Helm tiller, that tried to create the pod. If no pod security policy supports the user or service account, then the `PodSecurityPolicy` admission controller prevents the pods from being created.

If you deleted one of the pod security policy resources for [{{site.data.keyword.IBM_notm}} cluster management](cs_psp.html#ibm_psp), you might experience similar issues.

{: tsResolve}
Make sure that the user or service account is authorized by a pod security policy. You might need to [modify an existing policy](cs_psp.html#customize_psp).

If you deleted an {{site.data.keyword.IBM_notm}} cluster management resource, refresh the Kubernetes master to restore it.

1.  [Log in to your account. Target the appropriate region and, if applicable, resource group. Set the context for your cluster](cs_cli_install.html#cs_cli_configure).
2.  Refresh the Kubernetes master to restore it.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Cluster remains in a pending State
{: #cs_cluster_pending}

{: tsSymptoms}
When you deploy your cluster, it remains in a pending state and doesn't start.

{: tsCauses}
If you just created the cluster, the worker nodes might still be configuring. If you already wait for a while, you might have an invalid VLAN.

{: tsResolve}

You can try one of the following solutions:
  - Check the status of your cluster by running `ibmcloud ks clusters`. Then, check to be sure that your worker nodes are deployed by running `ibmcloud ks workers <cluster_name>`.
  - Check to see whether your VLAN is valid. To be valid, a VLAN must be associated with infrastructure that can host a worker with local disk storage. You can [list your VLANs](/docs/containers/cs_cli_reference.html#cs_vlans) by running `ibmcloud ks vlans <zone>` if the VLAN does not show in the list, then it is not valid. Choose a different VLAN.

<br />


## Pods remain in pending state
{: #cs_pods_pending}

{: tsSymptoms}
When you run `kubectl get pods`, you can see pods that remain in a **Pending** state.

{: tsCauses}
If you just created the Kubernetes cluster, the worker nodes might still be configuring.

If this cluster is an existing one:
*  You might not have enough capacity in your cluster to deploy the pod.
*  The pod might have exceeded a resource request or limit.

{: tsResolve}
This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

If you just created the Kubernetes cluster, run the following command and wait for the worker nodes to initialize.

```
kubectl get nodes
```
{: pre}

If this cluster is an existing one, check your cluster capacity.

1.  Set the proxy with the default port number.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Open the Kubernetes dashboard.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Check if you have enough capacity in your cluster to deploy your pod.

4.  If you don't have enough capacity in your cluster, resize your worker pool to add more nodes.

    1.  Review the current sizes and machine types of your worker pools to decide which one to resize.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Resize your worker pools to add more nodes to each zone that the pool spans.

        ```
        ibmcloud ks worker-pool-resize <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Optional: Check your pod resource requests.

    1.  Confirm that the `resources.requests` values are not larger than the worker node's capacity. For example, if the pod request `cpu: 4000m`, or 4 cores, but the worker node size is only 2 cores, the pod cannot be deployed.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  If the request exceeds the available capacity, [add a new worker pool](cs_clusters.html#add_pool) with worker nodes that can fulfill the request.

6.  If your pods still stay in a **pending** state after the worker node is fully deployed, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) to further troubleshoot the pending state of your pod.

<br />


## Containers do not start
{: #containers_do_not_start}

{: tsSymptoms}
The pods deploy successfully to clusters, but the containers do not start.

{: tsCauses}
Containers might not start when the registry quota is reached.

{: tsResolve}
[Free up storage in {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />


## Pods repeatedly fail to restart or are unexpectedly removed
{: #pods_fail}

{: tsSymptoms}
Your pod was healthy but unexpectedly gets removed or gets stuck in a restart loop.

{: tsCauses}
Your containers might exceed their resource limits, or your pods might be replaced by higher priority pods.

{: tsResolve}
To see if a container is being killed because of a resource limit:
<ol><li>Get the name of your pod. If you used a label, you can include it to filter your results.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Describe the pod and look for the **Restart Count**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>If the pod restarted many times in a short period of time, fetch its status. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Review the reason. For example, `OOM Killed` means "out of memory," indicating that the container is crashing because of a resource limit.</li>
<li>Add capacity to your cluster so that the resources can be fulfilled.</li></ol>

<br>

To see if your pod is being replaced by higher priority pods:
1.  Get the name of your pod.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Describe your pod YAML.

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  Check the `priorityClassName` field.

    1.  If there is no `priorityClassName` field value, then your pod has the `globalDefault` priority class. If your cluster admin did not set a `globalDefault` priority class, then the default is zero (0), or the lowest priority. Any pod with a higher priority class can preempt, or remove, your pod.

    2.  If there is a `priorityClassName` field value, get the priority class.

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  Note the `value` field to check your pod's priority.

4.  List existing priority classes in the cluster.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  For each priority class, get the YAML file and note the `value` field.

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  Compare your pod's priority class value with the other priority class values to see if it is higher or lower in priority.

7.  Repeat steps 1 to 3 for other pods in the cluster, to check what priority class they are using. If those other pods' priority class is higher than your pod, your pod is not provisioned unless there is enough resources for your pod and every pod with higher priority.

8.  Contact your cluster admin to add more capacity to your cluster and confirm that the right priority classes are assigned.

<br />


## Cannot install a Helm chart with updated configuration values
{: #cs_helm_install}

{: tsSymptoms}
When you try to install an updated Helm chart by running `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, you get the `Error: failed to download "ibm/<chart_name>"` error message.

{: tsCauses}
The URL for the {{site.data.keyword.Bluemix_notm}} repository in your Helm instance might be incorrect.

{: tsResolve}
To troubleshoot your Helm chart:

1. List the repositories currently available in your Helm instance.

    ```
    helm repo list
    ```
    {: pre}

2. In the output, verify that the URL for the {{site.data.keyword.Bluemix_notm}} repository, `ibm`, is `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * If the URL is incorrect:

        1. Remove the {{site.data.keyword.Bluemix_notm}} repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Add the {{site.data.keyword.Bluemix_notm}} repository again.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * If the URL is correct, get the latest updates from the repository.

        ```
        helm repo update
        ```
        {: pre}

3. Install the Helm chart with your updates.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all the available commands and flags.
-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`.
{: tip}

