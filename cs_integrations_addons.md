---

copyright:
  years: 2014, 2020
lastupdated: "2020-04-06"

keywords: kubernetes, iks, helm

subcollection: containers

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# Adding services by using managed add-ons
{: #managed-addons}

Quickly add extra capabilities and open-source technologies to your cluster with managed add-ons.
{: shortdesc}

**What are managed add-ons?** </br>
Managed {{site.data.keyword.containerlong_notm}} add-ons are an easy way to enhance your cluster with extra capabilities and open-source capabilities, such as Istio, Knative, Kubernetes Terminal, or {{site.data.keyword.block_storage_is_short}}. The version of the driver, plug-in, or open-source tool that you add to your cluster is tested by IBM and approved to be used in {{site.data.keyword.containerlong_notm}}.

The managed add-ons that you can install in your cluster depend on the type of cluster, the container platform, and the infrastructure provider that you choose.
{: note}

**How does the billing and support work for managed add-ons?** </br>
Managed add-ons are fully integrated into the {{site.data.keyword.cloud_notm}} support organization. If you have a question or an issue with using the managed add-ons, you can use one of the {{site.data.keyword.containerlong_notm}} support channels. For more information, see [Getting help and support](/docs/containers?topic=containers-cs_troubleshoot#getting_help).

If the tool that you add to your cluster incurs costs, these costs are automatically integrated and listed as part of your {{site.data.keyword.containerlong_notm}} billing. The billing cycle is determined by {{site.data.keyword.cloud_notm}} depending on when you enabled the add-on in your cluster.

**What limitations do I need to account for?** </br>
If you installed the [container image security enforcer admission controller](/docs/Registry?topic=registry-security_enforce#security_enforce) in your cluster, you cannot enable managed add-ons in your cluster.

## Adding managed add-ons
{: #adding-managed-add-ons}

To enable a managed add-on in your cluster, you use the [`ibmcloud ks cluster addon enable` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). When you enable the managed add-on, a supported version of the tool, including all Kubernetes resources are automatically installed in your cluster. Refer to the documentation of each managed add-on to find the prerequisites that your cluster must meet to install the managed add-on.

For more information about the prerequisites for each add-on, see:

- [Istio](/docs/containers?topic=containers-istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)
- [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web)
- [{{site.data.keyword.block_storage_is_short}}](/docs/containers?topic=containers-vpc-block)

## Updating managed add-ons
{: #updating-managed-add-ons}

The versions of each managed add-on are tested by {{site.data.keyword.cloud_notm}} and approved for use in {{site.data.keyword.containerlong_notm}}. To update the components of an add-on to the most recent version supported by {{site.data.keyword.containerlong_notm}}, use the following steps.
{: shortdesc}

1. Check for update instructions that are specific to your managed add-on, such as [Knative](/docs/containers?topic=containers-serverless-apps-knative#update-knative-addon) or [Istio](/docs/containers?topic=containers-istio#istio_update). If you do not find update instructions, continue with the next step.
2. If your add-on does not have specific update instructions, select the cluster where you installed managed add-ons from your [cluster dashboard](https://cloud.ibm.com/kubernetes/clusters).
3. Select the **Add-ons** tab.
4. From the actions menu, select **Update** to start updating the managed add-on. When the update is installed, the latest version of the managed add-on is listed on the cluster add-on page.

<br />


## Reviewing add-on states and statuses
{: #debug_addons_review}

You can check the health state and status of a cluster add-on by running the following command:
```
ibmcloud ks cluster addons -c <cluster_name_or_ID>
```
{: pre}

Example output:
```
Name            Version   Health State   Health Status
debug-tool      2.0.0     normal         Addon Ready
istio           1.4       -              Enabling
kube-terminal   1.0.0     normal         Addon Ready
```
{: screen}

The **Health State** reflects the lifecycle of the add-on components. The **Health Status** provides details of what add-on operation is in progress. Each state and status is described in the following tables.

|Add-on health state|Description|
|--- |--- |
|`critical`|The add-on is not ready to be used for one of the following reasons:<ul><li>Some or all of the add-on components are unhealthy.</li><li>The add-on failed to deploy.</li><li>The add-on may be at an unsupported version.</li></ul>Check the **Health Status** field for more information.|
|`normal`|The add-on is successfully deployed. Check the status to verify that the add-on is `Ready` or to see if an update is available.|
|`pending`|Some or all components of the add-on are currently deploying. Wait for the state to become `normal` before working with your add-on.|
|`warning`|The add-on might not function properly due to cluster limitations. Check the **Health Status** field for more information.|
{: caption="Add-on health states"}
{: summary="Table rows read from left to right, with the add-on state in column one and a description in column two."}

</br>

|Add-on health status|Description|
|--- |--- |
|`Addon Not Ready`|Some or all of the add-on components are unhealthy. Check whether all add-on component pods are running. For example, for the Istio add-on, check whether all pods in the `istio-system` namespace are `Running` by running `kubectl get pods -n istio-system`.|
|`Addon Ready`|The add-on is successfully deployed and is healthy.|
|`Addon Unsupported`|The add-on runs an unsupported version. [Update your add-on to the latest version](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons), or see specific update steps for [Istio](/docs/containers?topic=containers-istio#istio_update) or [Knative](/docs/containers?topic=containers-serverless-apps-knative#update-knative-addon).|
|`Cluster resources low, not enough workers in Ready state.`|The add-on is not ready to be used for one of the following reasons:<ul><li>The cluster does not meet the size criteria for the add-on. For example, check the size requirements for [Istio](/docs/containers?topic=containers-istio#istio_install) or [Knative](/docs/containers?topic=containers-serverless-apps-knative#knative-setup).</li><li>Worker nodes in your cluster are not in a `Normal` state. [Review the worker nodes' state and status](/docs/containers?topic=containers-cs_troubleshoot_clusters#debug_worker_nodes).</li></ul>|
|`Enabling`|The add-on is currently deploying to the cluster. Note that the add-on might take up to 15 minutes to install.|
|`Istio data plane components may need to be updated.`| When the Istio control plane is updated, the Istio data plane components are not updated automatically. Whenever the Istio add-on is updated to a new patch or minor version, you must [manually update your data plane components](/docs/containers?topic=containers-istio#update_client_sidecar), including the `istioctl` client and the Istio sidecars for your app.|
{: caption="Add-on health statuses"}
{: summary="Table rows read from left to right, with the add-on status in column one and a description in column two."}


Still having trouble with the Istio add-on? Check out these [troubleshooting topics](/docs/containers?topic=containers-istio#istio-ts) to debug your Istio deployment.
{: tip}
