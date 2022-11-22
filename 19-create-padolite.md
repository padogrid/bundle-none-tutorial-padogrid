![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

## 19. Create and start PadoLite cluster

PadoLite is a stripped down, non-federated version of Pado for including support for Pado features in Geode and GemFire. With PadoLite, you get all the Pado features that can be run only in a single cluster.

```bash
# Create 'mypadolite' cluster
make_cluster -product geode -type padolite

# switch and run mypadolite
switch_cluster mypadolite
start_cluster
```

Once started, you can ingest data into the `mypadolite` cluster using the `perf_test_geode` app we created earlier.

```bash
cd_app perf_test_geode/bin_sh

./test_ingestion -run
./test_tx -run
./test_group -run

# Ingest co-located data by specifying group-factory-er.properties 
./test_group -run -prop ../etc/group-factory-er.properties
```

You can run `gfsh` to monitor the cluster as we did before.

```bash
gfsh
```

From the gfsh console, execute the following:

```bash
connect
set variable --name=APP_RESULT_VIEWER --value=“external”
list regions
describe region --name=/nw/customers
describe region --name=/nw/orders
query --query="select * from /nw/customers"
query --query="select * from /nw/orders"
quit
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
