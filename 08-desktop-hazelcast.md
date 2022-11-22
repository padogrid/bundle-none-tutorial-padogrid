![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 8. Install and run desktop app

PadoGrid includes a **HazelcastDesktop** app that supports PadoGrid's own HQL (Hazelcast Query Language). HQL is similar to SQL introduced in Hazelcast 5.1. Its query engine is entirely written using the Hazelcast API and unlike Hazelcast's SQL which only runs on Hazelcast 5.1+, HQL runs on Hazelcast 3.x, 4.x, and 5.x. It has some limitations, however, such as no support for `limit` and select projections. If you can live with these limitations then it is a great tool for development, tests, and troubleshooting data problems.

Install and run the HazelcastDesktop.

```bash
install_padogrid -product hazelcast-desktop
update_products -product hazelcast-desktop
create_app -product hazelcast -app desktop
cd_app desktop/bin_sh
./desktop
```

You can login with any user name and password. 

![Hazelcast Desktop Login](images/hazelcast-desktop-login.png)

Once logged in, select any of the maps shown in the left pane or enter a select statement in the HQL pane and hit Ctrl-Enter to view data.

![Hazelcast Desktop](images/hazelcast-desktop.png)

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
