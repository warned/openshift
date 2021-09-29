---

copyright: 
  years: 2014, 2021
lastupdated: "2021-09-24"

keywords: openshift, roks, rhoks, rhos

subcollection: openshift
content-type: troubleshoot

---



{{site.data.keyword.attribute-definition-list}}

# Why does installing the Object storage plug-in fail?
{: #cos_plugin_fails}

**Infrastructure provider**:
* <img src="../images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="../images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC


When you install the `ibm-object-storage-plugin`, the installation fails with an error similar to the following:
{: tsSymptoms}

```
Error: rendered manifest contains a resource that already exists. Unable to continue with install. Existing resource conflict: namespace: , name: ibmc-s3fs-flex-cross-region, existing_kind: storageClass, new_kind: storage.k8s.io/v1, Kind=StorageClass
Error: plugin "ibmc" exited with error
```
{: screen}


During the installation, many different tasks are executed by the {{site.data.keyword.cos_full_notm}} plug-in such as creating storage classes and cluster role bindings. 
{: tsCauses}

Some of these resources might already exist in your cluster from previous {{site.data.keyword.cos_full_notm}} plug-in installations and were not properly removed when you removed or upgraded the plug-in.

Delete the resource that is display in the error message and retry the installation.
{: tsResolve}

1. Delete the resource that is displayed in the error message.
    ```
    oc delete <resource_kind> <resource_name>
    ```
    {: pre}

    Example for deleting a storage class resource:
    ```
    oc delete storageclass <storage_class_name>
    ```
    {: pre}

2. [Retry the installation](/docs/openshift?topic=openshift-object_storage#install_cos).

3. If you continue to see the same error, get a list of the resources that are installed when the plug-in is installed. Get a list of storage classes that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get StorageClass --all-namespaces \
            -l app=ibmcloud-object-storage-plugin \
            -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

2. Get a list of cluster role bindings that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get ClusterRoleBinding --all-namespaces \
            -l app=ibmcloud-object-storage-plugin \
            -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

3. Get a list of role bindings that are created by the `ibmcloud-object-storage-driver`.
    ```sh
    oc get RoleBinding --all-namespaces \
        -l app=ibmcloud-object-storage-driver \
        -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

4. Get a list of role bindings that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get RoleBinding --all-namespaces \
            -l app=ibmcloud-object-storage-plugin \
            -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

5. Get a list of cluster roles that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get ClusterRole --all-namespaces \
        -l app=ibmcloud-object-storage-plugin \
        -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

6. Get a list of deployments that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get Deployments --all-namespaces \
        -l app=ibmcloud-object-storage-plugin \
        -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

7. Get a list of the daemon sets that are created by the `ibmcloud-object-storage-driver`.
    ```sh
    oc get DaemonSets --all-namespaces \
        -l app=ibmcloud-object-storage-driver \
        -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

8. Get a list of the service accounts that are created by the `ibmcloud-object-storage-driver`.
    ```sh
    oc get ServiceAccount --all-namespaces \
        -l app=ibmcloud-object-storage-driver \
        -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre }

9. Get a list of the service accounts that are created by the `ibmcloud-object-storage-plugin`.
    ```sh
    oc get ServiceAccount --all-namespaces \
        -l app=ibmcloud-object-storage-plugin \
        -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
    ```
    {: pre}

4. Delete the conflicting resources.

5. After you delete the conflicting resources, [retry the installation](/docs/openshift?topic=openshift-object_storage#install_cos).







