---

copyright: 
  years: 2014, 2023
lastupdated: "2023-03-22"

keywords: openshift

subcollection: openshift

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}





# Debugging clusters
{: #debug_clusters}
{: troubleshoot}
{: support}

[Virtual Private Cloud]{: tag-vpc} [Classic infrastructure]{: tag-classic-inf}

Review the options to debug your clusters and find the root causes for failures.
{: shortdesc}

1. List your cluster and find the `State` of the cluster.

    ```sh
    ibmcloud oc cluster ls
    ```
    {: pre}

1. Review the `State` of your cluster. If your cluster is in a **Critical**, **Delete failed**, or **Warning** state, or is stuck in the **Pending** state for a long time. For more information, see [cluster states](/docs/openshift?topic=openshift-cluster-states-reference). 

1. Review the state of each worker node. For more information, see [worker node states](/docs/openshift?topic=openshift-worker-node-state-reference).
    ```sh
    ibmcloud oc worker ls -c CLUSTER
    ```
    {: pre}

1. Review the [common worker node issues](/docs/openshift?topic=openshift-common_worker_nodes_issues). 

1. Start [debugging the worker nodes](/docs/openshift?topic=openshift-debug_worker_nodes).

1. Review the site map for additional [troubleshooting information](/docs/openshift?topic=openshift-sitemap#sitemap_troubleshooting).






