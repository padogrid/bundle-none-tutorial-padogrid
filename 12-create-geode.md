![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

## 12. Create Geode cluster

In the previous section, we learned how to install Geode and GemFire. Let's continue our tutorial with Geode. If you want to try GemFire, then simply replace `geode` with `gemfire`. All the commands remain the same.

```bash
# If the '-cluster' option is not specified then it creates the default cluster, 'mygeode'
make_cluster -product geode

# Switch and start cluster
switch_cluster mygeode
start_cluster
```

Once the cluster is started, check the status:

```bash
show_cluster -long
```

Note that Geode has one additional member called **locator** running. The locator oversees the cluster members and provides registry and management services. It also by default provides a JMX management service with the frontend Pulse that is similar to Hazelcast Management Center.

## 12.1. Configure Ports

Follow the instructions in one of the subsections that applies to your environment.

### 12.1.1.  Local Machine

Nothing to do.

### 12.1.2. Docker

Nothing to do. Docker ports have already beeen exposed.

### 12.1.3. Kubernetes

Forward the Pulse port to your host OS.

```bash
# Geode/GemFire Pulse
kubectl port-forward svc/padogrid 7070
```

### 12.1.4. OpenShift

Expose the Pulse port by executing the following:

```bash
oc expose svc padogrid --name geode-pulse --port 7070
oc get route
```

Output:

```console
NAME          HOST/PORT                               PATH   SERVICES   PORT   TERMINATION   WILDCARD
geode-pulse   geode-pulse-padogrid.apps-crc.testing          padogrid   7070                 None
padogrid      padogrid-padogrid.apps-crc.testing             padogrid   8888                 None
```

## 12.2. Local Machine, Docker, Kubernetes

### 12.2.1. Geode/GemFire Pulse

Geode/GemFire Pulse URL: <http://localhost:7070/pulse>

- Username: admin
- Password: admin

## 12.3. OpenShift

### 12.3.1. Geode/GemFire Pulse

Geode/GemFire Pulse URL: <http://geode-pulse-padogrid.apps-crc.testing/pulse>

- Username: admin
- Password: admin

## 12.4. Browse Geode/GemFire Pulse

PadoGrid closely follows the Geode/GemFire default settings. By default, Geode/GemFire uses locators to host Pulse. If we had two (2) locators running, then we would have an option to use either one to login to Pulse.

![Pulse](images/geode-pulse.png)

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
