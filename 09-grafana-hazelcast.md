![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 9. Install and Run Prometheus and Grafana

PadoGrid supports Prometheus and Grafana for monitoring JMX metrics in real time. You can install them by running `install_padogrid` as follows.

❗The Grafana app scripts may not work with Grafana 10.x. Please install Grafana 9.x for this tutorial.

```bash
install_padogrid -product prometheus
install_padogrid -product grafana-enterprise
update_products -product prometheus
update_products -product grafana
```

Once installed, create and run the `grafana` app as follows.

```bash
create_app -product hazelcast -app grafana -name grafana_hz
cd_app grafana_hz/bin_sh
./start_prometheus
./start_grafana
```

## 9.1. Configure Ports

Follow the instructions in one of the subsections that applies to your environment.

### 9.1.1. Local Machine

Nothing to do.

### 9.1.2. Docker

Nothing to do. Docker ports have already beeen exposed.

### 9.1.3. Kubernetes

Forward Prometheus and Grafana ports to your host OS.

```bash
# Prometheus
kubectl port-forward svc/padogrid 9090
# Grafana
kubectl port-forward svc/padogrid 3000
```

### 9.1.4. OpenShift

Expose the Prometheus and Grafana ports by executing the following:

```bash
oc expose svc padogrid --name prometheus --port 9090
oc expose svc padogrid --name grafana --port 3000
oc get route
```

Output:

```console
NAME           HOST/PORT                                PATH   SERVICES   PORT   TERMINATION   WILDCARD
grafana        grafana-padogrid.apps-crc.testing               padogrid   3000                 None
hazelcast-mc   hazelcast-mc-padogrid.apps-crc.testing          padogrid   8080                 None
padogrid       padogrid-padogrid.apps-crc.testing              padogrid   8888                 None
prometheus     prometheus-padogrid.apps-crc.testing            padogrid   9090                 None
```

## 9.2. Local Machine, Docker, Kubernetes

### 9.2.1. Prometheus

Prometheus URL: <http://localhost:9090>

To view a complete list of metrics:

- All avalable metrics: http://localhost:9090/api/v1/label/name/values
- Metadata: http://localhost:9090/api/v1/metadata
- Prometheus specifics: http://localhost:9090/metrics
- Federated:
  ```bash
  curl -G http://localhost:9090/federate -d 'match[]={__name__!=""}'
  ```

### 9.2.2. Grafana

Grafana URL: <http://localhost:3000>

```console
User Name: admin
Password: admin
```

The grafana app has been preconfigured with the above user name and password. If you have a different account, then you can change them in setenv.sh. Note that the included commands require the user with administration privileges.


## 9.3. OpenShift

### 9.3.1 Prometheus

Prometheus URL: <http://prometheus-padogrid.apps-crc.testing>

To view a complete list of metrics:

- All avalable metrics: http://prometheus-padogrid.apps-crc.testing/api/v1/label/name/values
- Metadata: http://prometheus-padogrid.apps-crc.testing/api/v1/metadata
- Prometheus specifics: http://prometheus-padogrid.apps-crc.testing/metrics
- Federated:
  ```bash
  curl -G http://localhost:9090/federate -d 'match[]={__name__!=""}'
  ```

### 9.3.2. Grafana

Grafana URL: <http://grafana-padogrid.apps-crc.testing>

```console
User Name: admin
Password: admin
```

The grafana app has been preconfigured with the above user name and password. If you have a different account, then you can change them in setenv.sh. Note that the included commands require the user with administration privileges.

## 9.4. Import Dashboards to Grafana

The dashboards are organized by Grafana folders and they can be found in the following directory:

```bash
cd_app grafana_hz
ls etc/dashboards
```

To import all of the dashboards in the `etc/dashboards` directory, execute the following command.

```bash
cd_app grafana_hz/bin_sh
./import_folder -all
```

Now, go back to the Grafana console and select *Home/Dashboards*. Start with the *Hazelcast/00Main* dashboard, which resembles the Management Center main dashboard.

You can find further details from the following link.

  - https://github.com/padogrid/padogrid/wiki/Hazelcast-Grafana-App

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
