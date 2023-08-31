![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

## 13. Ingest data into Geode

✏️ *Note that even though you are running Geode/GemFire, all the PadoGrid commands shown here are exactly the same commands as the ones you executed in the Hazelcast tutorial sections. PadoGrid unifies the data grid products under the same set of lifecycle management commands.*

Create and run `perf_test_geode` app:

```bash
create_app -product geode -name perf_test_geode
cd_app perf_test_geode/bin_sh
./test_ingestion -run
./test_tx -run
./test_group -run
```

Let's also ingest customer and order mock data.

```bash
# Build perf_test - downloads javafaker
./build_app

# Ingest data
./test_group -run -prop ../etc/group-factory.properties
```

The `read_cache` command included in the Hazelcast version of `perf_test` is not available for Geode/GemFire because we can use `gfsh` to view data and much more.

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

Output:

```console
...
1666048092339 | 1666048092339 | "k0000000844" | "000000-0051" | "811873-4214" | 1665881687184 | 1666578641451 | 1665940901612 | "2"     | 7.52    | "Greenfelder-Jerde"                 | "Suite 921 379 Rudolph Glen, Port Roselee, WV 64032-8306"            | "Dallastown"            | "CO"       | "99561-9110"   | "Israel"
1666048092306 | 1666048092306 | "k0000000553" | "000000-0072" | "682891-1797" | 1665525188481 | 1666168089977 | 1665941230224 | "2"     | 82.57   | "Farrell, Bashirian and Shanahan"   | "599 Abshire Estates, West Phylicia, KY 07231-2565"                  | "West Rodgerberg"       | "NM"       | "36270-2564"   | "Latvia"
1666048092122 | 1666048092122 | "k0000000011" | "000000-0078" | "185700-6185" | 1665628374485 | 1665978789214 | 1665632741720 | "1"     | 12.8    | "Rutherford Group"                  | "Apt. 901 18337 Stroman Flats, Shandashire, MO 33911"                | "Markston"              | "IN"       | "07324-4345"   | "Cuba"
1666048092324 | 1666048092324 | "k0000000694" | "000000-0030" | "140540+4051" | 1665475910756 | 1665551276247 | 1665728357433 | "3"     | 38.97   | "Legros LLC"                        | "28888 Jaskolski Groves, Denesikville, MD 32574-0768"                | "West Nakia"            | "IA"       | "50067"        | "Albania"
1666048092261 | 1666048092261 | "k0000000029" | "000000-0014" | "393271-0943" | 1665842117094 | 1666201621197 | 1665892182845 | "2"     | 99.45   | "Nicolas-Conn"                      | "Suite 740 92702 Senger Underpass, Willyton, CO 89798"               | "Vernontown"            | "FL"       | "70622-5405"   | "Northern Mariana Islands"
...
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
