![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

## 16. Group clusters

A typical system architecture is comprised of more than one cluster. Managing multiple clusters individually is an error-prone and a time consuming task. To ease this task, PadoGrid provides a mechanism to logically group multiple clusters allowing you to manage them as a single unit. This is achieved by creating a group and adding clusters to the group.

Let's create a group called, `datagrids` that contains the `myhz` and `mygeode` clusters.

```bash
# By default, create_group creates and adds two (2) clusters. 
# Since we already have clusters that we want to group, let's
# create an empty group by specifying '-count 0'.
create_group -group datagrids -count 0

# Add the existing clusters to the group
add_cluster -group datagrids -cluster myhz
add_cluster -group datagrids -cluster mygeode
```

Switch into the `datagrids` group and run `show_group` to view the group details.

```bash
switch_group datagrids
show_group
```

Output:

```console
           GROUP: datagrids
        Clusters: myhz, mygeode

         CLUSTER: myhz
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/myhz
         PRODUCT: hazelcast
    CLUSTER_TYPE: hazelcast
             POD: local
        POD_TYPE: local
 Members Running: 0/2
      MC Running: 0/1
         Version: 5.3.2
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster myhz

         CLUSTER: mygeode
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/mygeode
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: default
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster mygeode
```

✏️ *You can always manage clusters individually even if they are in a group.*

Start the group. 

```bash
# Starts both myhz and mygeode. '-all' for starting Management Center
start_group -all
```

Stop the group.

```bash
# Stop both myhz ad mygeode. '-all' for stopping Management Center
stop_group -all

# Kill the group instead
kill_group -all
```

If you need to remove clusters from a group, execute the `remove_cluster` command. The following example removes the `myhz` cluster from the `datagrids` group.

```bash
# Remove myhz from the datagrids group
remove_cluster -group datagrids -cluster myhz
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
