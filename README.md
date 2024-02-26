![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

<!-- Platforms -->
[![PadoGrid 1.x](https://github.com/padogrid/padogrid/wiki/images/padogrid-padogrid-1.x.drawio.svg)](https://github.com/padogrid/padogrid/wiki/Platform-PadoGrid-1.x) [![Host OS](https://github.com/padogrid/padogrid/wiki/images/padogrid-host-os.drawio.svg)](https://github.com/padogrid/padogrid/wiki/Platform-Host-OS) [![VM](https://github.com/padogrid/padogrid/wiki/images/padogrid-vm.drawio.svg)](https://github.com/padogrid/padogrid/wiki/Platform-VM) [![Docker](https://github.com/padogrid/padogrid/wiki/images/padogrid-docker.drawio.svg)](https://github.com/padogrid/padogrid/wiki/Platform-Docker) [![Kubernetes](https://github.com/padogrid/padogrid/wiki/images/padogrid-kubernetes.drawio.svg)](https://github.com/padogrid/padogrid/wiki/Platform-Kubernetes)

# PadoGrid Tutorial

This tutorial bundle covers PadoGrid essentials.

## Installing Bundle

This bundles should be installed as a workspace as follows.

```bash
install_bundle -download -workspace bundle-none-tutorial-padogrid
```

## Use Case

This bundle provides step-by-step hands-on instructions for understanding PadoGrid basics. You will learn how to install PadoGrid and accompanying products, manage clusters, ingest data, monitor clusters.

![PadoGrid Tutorial](images/padogrid-tutorial.drawio.png)

## Required OS/Platform

One of the following OS's or platforms:

- Linux
- macOS
- Windows (Cygwin, WSL)
- Docker
- Kubernetes

## Required Software

- [Bash](https://www.gnu.org/software/bash/)
- [Maven 3.x](https://maven.apache.org/download.cgi)
- [Git](https://maven.apache.org/download.cgi)
- [JDK 8+](https://www.oracle.com/java/technologies/downloads/)
- `curl`
- [jq](https://stedolan.github.io/jq/)

## Introduction

PadoGrid is a productivity toolkit for managing user workspaces in the server-side. It is commonly used for creating distributed workspaces on your laptop to manage various clustering products running locally and remotely. Managing clustering (or data grid) products such as GemFire, Hazelcast, Redis, Coherence, Spark, Kafka, Hadoop, etc. is a complex task that often requires development and maintenance of custom scripts. 

The scripts provided by each product are typically only good for managing a single cluster member. They are inadequate for performing cluster-level lifecycle management. This leaves you with the time-consuming task of manually deploying, configuring, running, and managing each member that belongs to a cluster. The complexity of this task multiplies as you add more members, clusters, and products to your architecture. A steep learning curve of each product ultimately leads to a significant amount of investment in time and efforts.

PadoGrid solves this problem by providing a single, unified set of commands for managing clustering products. The same set of commands applies to all the [supported clustering products](https://github.com/padogrid/padogrid/wiki/Supported-Data-Grid-Products-and-Downloads). For products that are not supported, they can be found in *bundles* as part of solutions. Each bundle is a shrink-wrapped, turnkey solution capturing an end-to-end use case that you can simply install and run. There are numerous [public online bundles](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) including this bundle that are readily available for you.

PadoGrid is built from the ground up to bring the concept of *distributed workspaces* to practice so that you can create sandbox environments on the fly. When you create a workspace, PadoGrid automatically isolates it from others so that you can instead focus on your application. The bundles are the direct results of this concept of workspace isolation enabling built-in support for [multitency](https://github.com/padogrid/padogrid/wiki/Multitenancy). By design, each bundle runs in an isolated environment to prevent workspace conflicts.

In this tutorial bundle, we will explore some of the commonly used PadoGrid commands to understand the benefits of having distributed workspaces. 

## Tutorial Sequence

This tutorial is broken down to sections in individually enumerated markdown files as follows.

### PadoGrid 

- [00-installation](00-installation.md)
- [01-help](01-help.md)
- [02-create-rwe](02-create-rwe.md)
- [03-install-bundle](03-install-bundle.md)

### Hazelcast

- [04-create-hazelcast](04-create-hazelcast.md)
- [05-vm-workspace](05-vm-workspace.md)
- [06-start-hazelcast](06-start-hazelcast.md)
- [07-ingest-data-hazelcast](07-ingest-data-hazelcast.md)
- [08-desktop-hazelcast](08-desktop-hazelcast.md)
- [09-grafana-hazelcast](09-grafana-hazelcast.md)
- [10-close-workspace](10-close-workspace.md)

### Geode/GemFire

- [11-install-geode](11-install-geode.md)
- [12-create-geode](12-create-geode.md)
- [13-ingest-data-geode](13-ingest-data-geode.md)
- [14-grafana-geode](14-grafana-geode.md)
- [15-stop-geode](15-stop-geode.md)

### Group/Workspace
- [16-group-clusters](16-group-clusters.md)
- [17-stop-workspaces](17-stop-workspaces.md)

### Pado

- [18-install-pado](18-install-pado.md)
- [19-create-padolite](19-create-padolite.md)
- [20-create-padodesktop](20-create-padodesktop.md)
- [21-stop-padolite](21-stop-padolite.md)
- [22-create-pado](22-create-pado.md)
- [23-ingest-data-pado](23-ingest-data-pado.md)
- [24-monitor-pado](24-monitor-pado.md)
- [25-stop-pado](25-stop-pado.md)

### Mosquitto

- [26-install-mosquitto](26-install-mosquitto.md)
- [27-create-mosquitto](27-create-mosquitto.md)
- [28-publish-subscribe-data-mosquitto](28-publish-subscribe-data-mosquitto.md)
- [29-stop-mosquitto](29-stop-mosquitto.md)

### Bundles
- [30-bundles](30-bundles.md)

### Multitenancy

- [31-multitenancy](31-multitenancy.md)

## Preface

This bundle is best run using JupyterLab. The following manual page describes how to install and run JupyterLab in PadoGrid.

<https://github.com/padogrid/padogrid/wiki/JupyterLab>


---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
