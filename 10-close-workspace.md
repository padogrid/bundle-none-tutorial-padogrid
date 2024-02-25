![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 10. Close workspace

We have just completed the basics of operating clusters in a PadoGrid workspace. Let's now close the workspace by shutting down the software components.

## 10.1. View workspaces

First, view the workspaces to see what we have created so far. 

```bash
show_workspace
```

Output:

```console
Workspace:
   bundle-none-tutorial-padogrid [padogrid_1.0.0]

RWE Directory:
   /Users/dpark/Padogrid/workspaces/rwe-tutorial

Workspace Directory:
   /Users/dpark/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid

Workspace Type:
   local

apps
├── desktop
├── grafana
└── perf_test_hz

clusters
└── myhz

Switch Command:
   switch_rwe rwe-tutorial bundle-none-tutorial-padogrid
```

## 10.2. Stop Hazelcast cluster

We can now stop the Hazelcast cluster and Management Center.

```bash
# The '-all' option stops both the cluster and Management Center
stop_cluster -all
```

## 10.3. Stop Prometheus and Grafana

```bash
cd_app grafana/bin_sh
./stop_grafana
./stop_prometheus
```

Don't forget to close the HazelcastDesktop.

## 10.4. Summary

We have so far accomplished the following.

- Installed PadoGrid
- Created PadoGrid RWEs and workspaces
- Installed this bundle in a new RWE  
- Created and started a Hazelcast cluster and Management Center
- Created the `perf_test_hz` app to ingest data
  - test_ingestion for PBM data
  - test_group for binary, customer and order mock data
  - read_cache to read data stored in the cluster
- Installed and executed HazelcastDesktop
- Installed and executed Prometheus and Grafana 
- Closed the workspace by shutting down the Hazelcast cluster, Prometheus, and Grafana

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
