![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 11. Install Geode/GemFire

Let's now try another product, Geode/GemFire. Keep in mind that all of the cluster commands that we used previously for Hazelcast also apply to other products. 

Geode is open source software maintained by VMware. It originates from GemFire which has since been rebranded as the enterprise version of Geode providing support and services under the VMware Tanzu product portfolio. 

Let's install Geode using `install_padogrid`.


```bash
install_padogrid -product geode
update_products -product geode
```

The `install_padogrid` command does not support installation of GemFire due to its sign-in requirement. You will need to download it manually by signing into the VMware download site. We will use Geode instead.

You can view all the download links including GemFire by specifying the usage option, `install_padogrid -?` as shown below.


```bash
install_padogrid -?
```

Output:

```console
...
   The following products are not downlodable due to their sign-in requirements. You must manually
   download and install them. See the PadoGrid manual for instructions.

      - Oracle Coherence     https://www.oracle.com/middleware/technologies/coherence-downloads.html
      - TIBCO ComputeDB      https://edelivery.tibco.com/storefront/index.ep
      - Redis Enterprise     https://redis.com/redis-enterprise-software/download-center/software/
      - Redis Stack          https://redis.io/download/
      - VMware Tanzu GemFire https://network.pivotal.io/products/pivotal-gemfire
...
```

As you can see from the output, in addtion to GemFire, there are other products that you would need to manually download and install.

To install the downloaded prouct distributions, inflate them in the PadoGrid `products` directory. For example, the following inflates a GemFire distribution.

```bash
tar -C ~/Padogrid/products/ -xzf $PADOGRID_ENV_BASE_PATH/downloads/vmware-gemfire-9.15.3.tgz
```

✏️ `PADOGRID_ENV_BASE_PATH` is by default set to `~/Padogrid/products` or `/opt/padogrid/products`.


Once installed, update the workspace with the product version that you want to use. The `update_products` command automatically picks up all of the products in the `products` directory.

```bash
# Update GemFire product
update_products -product gemfire
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
