![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 0. Install PadoGrid

PadoGrid requires `bash` and can be installed on Linux, macOS, Windows with Cygwin or WSL, Docker, or Kubernetes. It also requires Maven 3.x and Git for building apps and bundles. Java is also required by most of the cluster products. Furthermore, the optional `jq` executable is also recommended for handling JSON values. Make sure you have them installed before proceeding with this tutorial.

✏️  *If you are using a PadoGrid container, then all the required software components are already installed.*

## 0.1. Linux, macOS, Windows (Cygwin, WSL)

By default, PadoGrid installs in your home directory under the `~/Padogrid` directory. You can change this directory by running the `install_padogrid` script. For our tutorial, let's install it in the default directory. The following `curl` command silently installs PadoGrid and Hazelcast OSS.

```bash
curl -fsSL https://raw.githubusercontent.com/padogrid/padogrid/develop/padogrid-deployment/src/main/resources/common/bin_sh/install_padogrid | /bin/bash -s -- -no-stty -quiet -product hazelcast-oss
```

Upon installation, execute the following to initialize PadoGrid. Note that PadoGrid only runs in `bash`. If `bash` is not your default shell, then you must manually run `bash` to initialize PadoGrid.

```bash
. ~/Padogrid/workspaces/myrwe/initenv.sh -quiet
echo ". ~/Padogrid/workspaces/myrwe/initenv.sh -quiet" >> ~/.bashrc
```

## 0.2. Docker

To login directly to PadoGrid container:

```bash
docker run -it --rm padogrid/padogrid /bin/bash
```

To launch PadoGrid JupyterLab:

```bash
docker run --rm -d -p 8888:8888 padogrid/padogrid
```
JupyterLab URL: https://0.0.0.0:8888/lab/workspaces/myrwe

## 0.3. Podman

To login directly to PadoGrid container:

```bash
podman run --rm -it padogrid/padogrid /bin/bash
```

To launch PadoGrid JupyterLab:

```bash
podman run --rm -d -p 8888:8888 padogrid/padogrid
```
JupyterLab URL: https://0.0.0.0:8888/lab/workspaces/myrwe

## 0.4. Kubernetes

```bash
kubectl run padogrid --image=docker.io/padogrid/padogrid
```

## 0.5. OpenShift

```bash
oc run padogrid --image=docker.io/padogrid/padogrid
```

## 0.6. Directory Layout

Let's take a look at the default directory where PadoGrid is installed.

If you installed PadoGrid on OS, then its default directory is `$HOME/Padogrid` and has the following directory layout.

```bash
tree -L 2 ~/Padogrid
```

You should see the directory structure similar to the following.

```console
~/Padogrid/
├── downloads
│   ├── hazelcast-5.2.1-slim.tar.gz
│   └── padogrid_0.9.22.tar.gz
├── products
│   ├── hazelcast-5.2.1-slim
│   └── padogrid_0.9.22
├── snapshots
└── workspaces
    └── myrwe
```

If you are running PadoGrid in Docker or Kubernetes, then the PadoGrid directory is `/opt/padogrid` and has the directory structure as follows.

```bash
tree -L 2 /opt/padogrid
```

```bash
/opt/padogrid
├── downloads
├── padogrid_start
├── products
│   ├── hazelcast-5.2.1-slim
│   ├── hazelcast-management-center-5.2.0
│   └── padogrid_0.9.22
└── workspaces
    └── myrwe
```

- The `downloads` directory contains the downloads done by the `install_padogrid` command. PadoGrid distributions include this command so that you can use it to install additional products at any time.

- The `padogrid_start` script is a bootstrap script for Docker and Kubernetes. You can ignore that file.

- The `products` directory contains the installed products. Product installations are typically done by inflating the downloads. For example, the downloaded `hazelcast-5.2.1-slim.tar.gz` and `padogrid_0.9.22.tar.gz` distributions are inflated by `install_padogrid` in `products/hazelcast-5.2.1-slim` and `products/padogrid_0.9.22`, respectively.

- The `snapshots` directory is reserved for future use. PadoGrid snapshot builds are also available and installable using `install_padogrid`. Note that the snapshots are currently installed in the `products` directory, not in the `snapshots` directory. This may change in the future.

- The `workspaces` directory contains user workspaces. The `workspaces/myrwe` directory is the default RWE (Root Workspaces Environment) that contains the default `myws` workspace created by `install_padogrid`. We will go over the workspace directory contents shortly in this tutorial.

In addition to `~/Padogrid`, PadoGrid creates the `~/.padogrid` directory to store workspace metadata as shown below. We will touch up on the use of this directory later.

```bash
tree ~/.padogrid
```

Output:

```console
~/.padogrid
├── setenv.sh
└── workspaces
    └── myrwe
        └── myws
            ├── clusters
            │   └── myhz
            │       └── clusterenv.sh
            └── workspaceenv.sh
```

## 0.7. Uninstalling PadoGrid

PadoGrid is uninstalled by removing the `~/PadoGrid` and `~/.padogrid` directories, and removing the PadoGrid initialization line in the `.bashrc` file. Executing the following commands completely removes PadoGrid including workspaces.

```bash
# Remove all PadoGrid directories
rm -rf ~/Padogrid ~/.padogrid

# Update .bashrc with backup
#macOS - create backup .bashrc0
sed -i 0 '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' ~/.bashrc
#Linux - create backup .bashrc0
sed -i0 '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' ~/.bashrc
```

✏️  *The above removes PadoGrid and all PadoGrid installed products and workspaces. If you have PadoGrid clusters and applications running, then make sure you stop them first before removing PadoGrid. Please see [FAQ](https://github.com/padogrid/padogrid/wiki/faq-How-do-I-stop-all-cluster-processes-started-by-PadoGrid) for stopping all cluster processes started by PadoGrid.*

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
