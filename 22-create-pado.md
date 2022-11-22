![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

## 22. Create and start federated Pado clusters

Pado federates Geode/GemFire clusters providing a single logical entry point for globally managing data grids. It provides essential design patterns that may be applied in building global data hubs. It is a working prototype that addresses many of data hub related problems.

❗ *Please note that Pado is not for production use. It is made available as a prototype for building an actual production system.*

The following command creates five (5) Pado clusters with the cluster name prefix, `grid`, in the group named `mypado_group`. Using the specified prefix, it generates grids (clusters) named `grid0`, `grid1`, `grid2`, `grid3`, and `grid4`. By default, `grid0` is the parent grid of the remaining grids.

```bash
create_group -product geode  -group mypado_group -prefix grid -type pado -count 5
```

If you run `show_workspace`, you will see the additional Pado clusterss.

```bash
show_workspace
```

Output:

```console
...
apps
├── desktop
├── granfana_hz
├── padodesktop
├── perf_test_geode
└── perf_test_hz

clusters
├── grid0
├── grid1
├── grid2
├── grid3
├── grid4
├── mycluster
├── mygeode
└── mypadolite

groups
└── mypado_group
...
```

To view the group status, execute the `show_group` command. The following switches into the group you just created and displays its status.

```bash
switch_group mypado_group
show_group
```

Output:

```console
           GROUP: mypado_group
        Clusters: grid0, grid1, grid2, grid3, grid4

         CLUSTER: grid0
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/grid0
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: pado
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster grid0

         CLUSTER: grid1
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/grid1
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: pado
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster grid1

         CLUSTER: grid2
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/grid2
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: pado
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster grid2

         CLUSTER: grid3
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/grid3
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: pado
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster grid3

         CLUSTER: grid4
     CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-bundles/bundle-none-tutorial-padogrid/clusters/grid4
         PRODUCT: geode
    CLUSTER_TYPE: geode
        Run Type: pado
             POD: local
        POD_TYPE: local
Locators Running: 0/1
 Members Running: 0/2
         Version: 1.15.1
  Switch Cluster: switch_rwe rwe-bundles bundle-none-tutorial-padogrid; switch_cluster grid4
```

The `start_group` command will start the five (5) Pado clusters that you created. Each cluster starts two (2) members and one (1) locator. The members are configured with 1GB of maximum heap size and the locators with 512MB of maximum heap size. This means you should have about `2.5*5=12.5GB` of free memory in order to run all of the clusters. 

If you don't have enough free memory then you can remove some of the clusters using the `remove_cluster` command, which removes the specified cluster and updates the group accordingly. The following example remove the `grid4` cluster.

❗ *Do not remove `grid0` which is the parent grid that orchestrates other grids.*

```bash
# Remove cluster named 'grid4'
remove_cluster -cluster grid4 

# Check if 'grid4' is not in the group
show_group
```

Let's now start the group of clusters.

```bash
start_group
```

Once started, you can check the group status by executing `show_group`.

```bash
# Short status
show_group

# Detailed status
show_group -long
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
