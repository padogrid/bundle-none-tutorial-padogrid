![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 6. Start Hazelcast cluster

Either you have a VM-enabled workspace or not, the PadoGrid commands work the same way. Let's start the cluster and Hazelcast Management Center.

```bash
# The '-all' option starts both members and management center
start_cluster -all
```

Once started, you can check the cluster status by running `show_cluster`

```bash
# Display short running status 
show_cluster

# Display detailed status
show_cluster -long
```

For Hazelcast, you can view the Management Center (MC) status separtely by running `show_mc` as follows.

```bash
show_mc
show_mc -long
```

You can click on the Management Center URL displayed to manage the Hazelcast cluster from the browser. PadoGrid closely follows the Hazelcast default settings. This means the default Hazelcast cluster name is **dev**, and not **myhz**. This, of course, can be changed easily in the Hazelcast configuration file, but for now, let's stick with **dev**.

- From the browser, select the *ENABLE* button for *DEV MODE*.
- Select the *Add* button under the *Cluster Connections* pane.
- Keep the default settings and select the *CONNECT* button in the *Connect Cluster/Connect Directly* page.
- From the *Cluster Connection*" pane, select the *VIEW CLUSTER* button to open the management center.

Upon successful connection, you should see the Management Center view similar to the following image.

![Hazelcast Management Center](images/management-center.png)

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
