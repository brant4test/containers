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




# Popular topics for {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Keep up with what's going on in {{site.data.keyword.containerlong}}. Learn about new features to explore, a trick to try out, or some popular topics that other developers are finding useful in the now.
{:shortdesc}

## Popular topics in November 2018
{: #nov18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in November 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>1 November</td>
<td>Highly available masters in clusters that run Kubernetes version 1.11</td>
<td>In a single zone, your master is highly available and includes replicas on separate physical hosts for your Kubernetes API server, etcd, scheduler, and controller manager to protect against an outage such as during a cluster update. If your cluster is in a multizone-capable zone, your highly available master is also spread across zones to help protect against a zonal failure.<br>For actions that you must take, see [Updating to highly available cluster masters](cs_versions.html#ha-masters). These preparation actions apply:<ul>
<li>If you have a firewall or custom Calico network policies.</li>
<li>If you are using host ports `2040` or `2041` on your worker nodes.</li>
<li>If you used the cluster master IP address for in-cluster access to the master.</li>
<li>If you have automation that calls the Calico API or CLI (`calicoctl`), such as to create Calico policies.</li>
<li>If you use Kubernetes or Calico network policies to control pod egress access to the master.</li></td>
</tr>
</tbody></table>

## Popular topics in October 2018
{: #oct18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in October 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>25 October</td>
<td>[Zone available in Milan](cs_regions.html)</td>
<td>Welcome Milan, Italy as a new zone for paid clusters in the EU Central region. Previously, Milan was available only for free clusters. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for this zone and the other zones within the region that your cluster is in.</td>
</tr>
<tr>
<td>22 October</td>
<td>[New London multizone location, `lon05`](cs_regions.html#zones)</td>
<td>The London multizone metro city replaces `lon02` zone with the new `lon05` zone, a zone with more infrastructure resources than `lon02`. Create new multizone clusters with `lon05`. Existing clusters with `lon02` are supported, but new multizone clusters must use `lon05` instead.</td>
</tr>
<tr>
<td>05 October</td>
<td>Integration with {{site.data.keyword.keymanagementservicefull}}</td>
<td>You can encrypt the Kubernetes secrets in your cluster by [enabling {{site.data.keyword.keymanagementserviceshort}} (beta)](cs_encrypt.html#keyprotect).</td>
</tr>
<tr>
<td>04 October</td>
<td>[{{site.data.keyword.registrylong}} is now integrated with {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry/iam.html#iam)</td>
<td>You can use {{site.data.keyword.Bluemix_notm}} IAM to control access to your registry resources, such as pulling, pushing, and building images. When you create a cluster, you also create a registry token so that the cluster can work with your registry. Therefore, you need the registry **Administrator** platform management role to create a cluster. To enable {{site.data.keyword.Bluemix_notm}} IAM for your registry account, see [Enabling policy enforcement for existing users](/docs/services/Registry/registry_users.html#existing_users).</td>
</tr>
<tr>
<td>01 October</td>
<td>[Resource groups](cs_users.html#resource_groups)</td>
<td>You can use resource groups to separate your {{site.data.keyword.Bluemix_notm}} resources into pipelines, departments, or other groupings to help assign access and meter usage. Now, {{site.data.keyword.containerlong_notm}} supports creating clusters in the `default` group or any other resource group that you create!</td>
</tr>
</tbody></table>

## Popular topics in September 2018
{: #sept18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in September 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>25 September</td>
<td>[New zones available](cs_regions.html)</td>
<td>Now you have even more options of where to deploy your apps!
<ul><li>Welcome to San Jose as two new zones in the US South region, `sjc03` and `sjc04`. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for this zone and the others within the region that your cluster is in.</li>
<li>With two new `tok04` and `tok05` zones, you can now [create multizone clusters](cs_clusters_planning.html#multizone) in Tokyo in the AP North region.</li></ul></td>
</tr>
<tr>
<td>05 September</td>
<td>[Zone available in Oslo](cs_regions.html)</td>
<td>Welcome Oslo, Norway as a new zone in the EU Central region. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for this zone and the others within the region that your cluster is in.</td>
</tr>
</tbody></table>

## Popular topics in August 2018
{: #aug18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in August 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>31 August</td>
<td>{{site.data.keyword.cos_full_notm}} is now integrated with {{site.data.keyword.containerlong}}</td>
<td>Use Kubernetes-native persistent volume claims (PVC) to provision {{site.data.keyword.cos_full_notm}} to your cluster. {{site.data.keyword.cos_full_notm}} is best used for read-intensive workloads and if you want to store data across multiple zones in a multizone cluster. Start by [creating an {{site.data.keyword.cos_full_notm}} service instance](cs_storage_cos.html#create_cos_service) and [installing the {{site.data.keyword.cos_full_notm}} plug-in](cs_storage_cos.html#install_cos) on your cluster. </br></br>Not sure what storage solution might be the right one for you? Start [here](cs_storage_planning.html#choose_storage_solution) to analyze your data and choose the appropriate storage solution for your data. </td>
</tr>
<tr>
<td>14 August</td>
<td>Update your clusters to Kubernetes versions 1.11 to assign pod priority</td>
<td>After you update your cluster to [Kubernetes version 1.11](cs_versions.html#cs_v111), you can take advantage of new capabilities, such as increased container runtime performance with `containerd` or [assigning pod priority](cs_pod_priority.html#pod_priority).</td>
</tr>
</tbody></table>

## Popular topics in July 2018
{: #july18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in July 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>30 July</td>
<td>[Bring your own Ingress controller](cs_ingress.html#user_managed)</td>
<td>Do you have very specific security or other custom requirements for your cluster's Ingress controller? If so, you might want to run your own Ingress controller instead of the default.</td>
</tr>
<tr>
<td>10 July</td>
<td>Introducing multizone clusters</td>
<td>Want to improve cluster availability? Now you can span your cluster across multiple zones in select metro areas. For more information, see [Creating multizone clusters in {{site.data.keyword.containerlong_notm}}](cs_clusters_planning.html#multizone).</td>
</tr>
</tbody></table>

## Popular topics in June 2018
{: #june18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in June 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>13 June</td>
<td>The `bx` CLI command name is changing to `ic` CLI</td>
<td>When you download the latest version of the {{site.data.keyword.Bluemix_notm}} CLI, you now run commands by using the `ic` prefix instead of `bx`. For example, list your clusters by running `ibmcloud ks clusters`.</td>
</tr>
<tr>
<td>12 June</td>
<td>[Pod security policies](cs_psp.html)</td>
<td>For clusters that run Kubernetes 1.10.3 or later, you can
configure pod security policies to authorize who can create and update pods in {{site.data.keyword.containerlong_notm}}.</td>
</tr>
<tr>
<td>06 June</td>
<td>[TLS support for the IBM-provided Ingress wildcard subdomain](cs_ingress.html#wildcard_tls)</td>
<td>For clusters created on or after 6 June 2018, the IBM-provided Ingress subdomain TLS certificate is now a wildcard certificate and can be used for the registered wildcard subdomain. For clusters created before 6 June 2018, TLS certificate will be updated to a wildcard certificate when the current TLS certificate is renewed.</td>
</tr>
</tbody></table>

## Popular topics in May 2018
{: #may18}


<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in May 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>24 May</td>
<td>[New Ingress subdomain format](cs_ingress.html)</td>
<td>Clusters created after 24 May are assigned an Ingress subdomain in a new format: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. When you use Ingress to expose your apps, you can use the new subdomain to access your apps from the internet.</td>
</tr>
<tr>
<td>14 May</td>
<td>[Update: Deploy workloads on GPU bare metal worldwide](cs_app.html#gpu_app)</td>
<td>If you have a [bare metal graphics processing unit (GPU) machine type](cs_clusters_planning.html#shared_dedicated_node) in your cluster, you can schedule mathematically intensive apps. The GPU worker node can process your app's workload across both the CPU and GPU to increase performance.</td>
</tr>
<tr>
<td>03 May</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>Does your team need a little extra help to know which image to pull in your app containers? Try out the Container Image Security Enforcement beta to verify container images before you deploy them. Available for clusters that run Kubernetes 1.9 or later.</td>
</tr>
<tr>
<td>01 May</td>
<td>[Deploy the Kubernetes dashboard from the console](cs_app.html#cli_dashboard)</td>
<td>Did you ever want to access the Kubernetes dashboard with one click? Check out the **Kubernetes Dashboard** button in the {{site.data.keyword.Bluemix_notm}} console.</td>
</tr>
</tbody></table>




## Popular topics in April 2018
{: #apr18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in April 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>17 April</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Install the {{site.data.keyword.Bluemix_notm}} Block Storage [plug-in](cs_storage_block.html#install_block) to save persistent data in block storage. Then, you can [create new](cs_storage_block.html#add_block) or [use existing](cs_storage_block.html#existing_block) block storage for your cluster.</td>
</tr>
<tr>
<td>13 April</td>
<td>[New tutorial to migrate Cloud Foundry apps to clusters](cs_tutorials_cf.html#cf_tutorial)</td>
<td>Do you have a Cloud Foundry app? Learn how to deploy the same code from that app to a container that runs in a Kubernetes cluster.</td>
</tr>
<tr>
<td>05 April</td>
<td>[Filtering logs](cs_health.html#filter-logs)</td>
<td>Filter out specific logs from being forwarded. Logs can be filtered out for a specific namespace, container name, log level, and message string.</td>
</tr>
</tbody></table>

## Popular topics in March 2018
{: #mar18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in March 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>16 March</td>
<td>[Provision a bare metal cluster with Trusted Compute](cs_clusters_planning.html#shared_dedicated_node)</td>
<td>Create a bare metal cluster that runs [Kubernetes version 1.9](cs_versions.html#cs_v19) or later, and enable Trusted Compute to verify your worker nodes against tampering.</td>
</tr>
<tr>
<td>14 March</td>
<td>[Secure sign-in with {{site.data.keyword.appid_full}}](cs_integrations.html#appid)</td>
<td>Enhance your apps that are running in {{site.data.keyword.containerlong_notm}} by requiring users to sign in.</td>
</tr>
<tr>
<td>13 March</td>
<td>[Zone available in São Paulo](cs_regions.html)</td>
<td>Welcome São Paulo, Brazil as a new zone in the US South region. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for this zone and the others within the region that your cluster is in.</td>
</tr>
<tr>
<td>12 March</td>
<td>[Just joined the {{site.data.keyword.Bluemix_notm}} with a Trial account? Try out a free Kubernetes cluster!](container_index.html#clusters)</td>
<td>With a Trial [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/registration/), you can deploy one free cluster for 30 days to test out Kubernetes capabilities.</td>
</tr>
</tbody></table>

## Popular topics in February 2018
{: #feb18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in February 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>27 February</td>
<td>Hardware virtual machine (HVM) images for worker nodes</td>
<td>Increase the I/O performance of your workloads with HVM images. Activate on each existing worker node by using the `ibmcloud ks worker-reload` [command](cs_cli_reference.html#cs_worker_reload) or the `ibmcloud ks worker-update` [command](cs_cli_reference.html#cs_worker_update).</td>
</tr>
<tr>
<td>26 February</td>
<td>[KubeDNS autoscaling](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS now scales with your cluster as it grows. You can adjust the scaling rations by using the following command: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 February</td>
<td>View the web console for [logging](cs_health.html#view_logs) and [metrics](cs_health.html#view_metrics)</td>
<td>Easily view log and metric data on your cluster and its components with an improved web UI. See your cluster detail page for access.</td>
</tr>
<tr>
<td>20 February</td>
<td>Encrypted images and [signed, trusted content](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>In {{site.data.keyword.registryshort_notm}}, you can sign and encrypt images to ensure their integrity when you store the images in your registry namespace. Run your container instances by using only trusted content.</td>
</tr>
<tr>
<td>19 February</td>
<td>[Set up the strongSwan IPSec VPN](cs_vpn.html#vpn-setup)</td>
<td>Quickly deploy the strongSwan IPSec VPN Helm chart to connect your {{site.data.keyword.containerlong_notm}} cluster securely to your on-premises data center without a Virtual Router Appliance.</td>
</tr>
<tr>
<td>14 February</td>
<td>[Zone available in Seoul](cs_regions.html)</td>
<td>Just in time for the Olympics, deploy a Kubernetes cluster to Seoul in the AP North region. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for this zone and the others within the region that your cluster is in.</td>
</tr>
<tr>
<td>08 February</td>
<td>[Update Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>Review the changes to make to your clusters before you update to Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Popular topics in January 2018
{: #jan18}

<table summary="The table shows popular topics. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Popular topics for containers and Kubernetes clusters in January 2018</caption>
<thead>
<th>Date</th>
<th>Title</th>
<th>Description</th>
</thead>
<tbody>
<td>25 January</td>
<td>[Global registry available](../services/Registry/registry_overview.html#registry_regions)</td>
<td>With the {{site.data.keyword.registryshort_notm}}, you can use the global `registry.bluemix.net` to pull IBM-provided public images.</td>
</tr>
<tr>
<td>23 January</td>
<td>[Zones available in Singapore and Montreal, CA](cs_regions.html)</td>
<td>Singapore and Montreal are zones available in the {{site.data.keyword.containerlong_notm}} AP North and US East regions. If you have a firewall, be sure to [open the firewall ports](cs_firewall.html#firewall) for these zones and the others within the region that your cluster is in.</td>
</tr>
<tr>
<td>08 January</td>
<td>[Enhanced flavors available](cs_cli_reference.html#cs_machine_types)</td>
<td>Series 2 virtual machine types include local SSD storage and disk encryption. [Move your workloads](cs_cluster_update.html#machine_type) to these flavors for improved performance and stability.</td>
</tr>
</tbody></table>

## Chat with like-minded developers on Slack
{: #slack}

You can see what others are talking about and ask your own questions in the [{{site.data.keyword.containerlong_notm}} Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com)
{:shortdesc}

If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
{: tip}
