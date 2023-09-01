![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
---

# 28. Publish/subscribe data

We need two (2) terminals for the tutorial in this section. Run the commands in the designated terminal.


## 28.1. `perf_test`

![Terminal](images/terminal.png) Terminal 2

Create `perf_test_mosquitto` and subscribe to the topic filter, `test/#`. 

```bash
switch_cluster mymosquitto
create_app -product mosquitto -name perf_test_mosquitto
cd_app perf_test_mosquitto/bin_sh
./subscribe_topic test/#
```

![Terminal](images/terminal.png) Terminal 1

Run `test_group` to publish data.

```bash
switch_cluster mymosquitto
cd_app perf_test_mosquitto/bin_sh
./test_group -run
```

You should Terminal 2 receiving the published data.

The `perf_test` app has been preconfigured to connect to the default endpoints, `tcp://localhost:1883-1885`, defined in the `etc/mqtt5-client.yaml` file.

![Terminal](images/terminal.png) Terminal 1

```bash
cd_app perf_test_mosquitto
cat etc/mqtt5-client.yaml
```

Output:

```yaml
...
clusters:
...
  - name: ${cluster.name}
    connections:
    ...
        connection:
        ...
          serverURIs: [tcp://localhost:1883-1885]
...
```

The `serverURIs` attribute defines the endpoints that form the virtual cluster, named `${cluster.name}`, which is the system property passed in by the `perf_test` scripts. In our case, the cluster name is `mymosquitto`.

## 28.2. `vc_subscribe`

![Terminal](images/terminal.png) Terminal 2

Let's now use the PadoGrid's `vc_subscribe` command subscribe to the topic filter, `test/#`, to receive messages published by `test_group`.

```bash
switch_cluster mymosquitto
vc_subscribe -t test/#
```

![Terminal](images/terminal.png) Terminal 1

Run `test_group` again.

```bash
cd_app perf_test_mosquitto/bin_sh
./test_group -run
```

Terminal 2 should receive messages.

The `vc_subscribe` is a general purpose tool that connects to one or more virtual clusters. By default, it connects to the current context cluster. It provides options, `-cluster`, `-endpoints`, and `-config` to connect to a specific cluster.

## 28.3. `vc_publish`

![Terminal](images/terminal.png) Terminal 1

Let's now use `vc_publish` to publish your own messages.

```bash
vc_publish -t test/tweet -m "hello world"
```

You should see the message, `hello world`, received by Terminal 2.

## 28.4. Public Brokers

![Terminal](images/terminal.png) Terminal 2

Stop the `vc_subscribe` command by typing *Ctrl-C*.

There are several MQTT brokers available for public use. We can create a virtual cluster comprising of those brokers by specifying the endpoints as follows.

```bash
vc_subscribe -t "test/#" \
             -endpoints "tcp://test.mosquitto.org:1883, ws://test.mosquitto.org:8080, \
                         tcp://broker.emqx.io:1883, ws://broker.emqx.io:8083, \
                         tcp://broker.hivemq.com:1883, ws://broker.hivemq.com:8000"
```

You should receive messages published by the users around the world. Note that the endpoints contain both `tcp` and `ws` (websockets) protocols. In this example, we have clustered six (6) brokers just by listing their endpoints. 

## 28.5. `vc_start`

The `vc_start` command allows you to add plugins for virtual clusters. The plugins can handle a wide variety of tasks such as bridging clusters, cleansing/transforming/curating data, providing data services, embedding business logic, and etc. Note that `vc_start` requires a configuration file in which one or more plugins are configured.

To the usage, specify the `-?` option.

![Terminal](images/terminal.png) Terminal 1

```bash
vc_start -?
```

Take a look at the [Archetype 4](https://github.com/padogrid/bundle-mosquitto-tutorial-virtual-clusters#archetype-4) example in the [Mosquitto/MQTT Virtual Cluster Tutorial](https://github.com/padogrid/bundle-mosquitto-tutorial-virtual-clusters) bundle. It runs `vc_start` to bridge two (2) virtual clusters.

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
