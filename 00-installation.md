![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 0. Install PadoGrid

PadoGrid can be installed on Linux, macOS, Windows with Cygwin or WSL, Docker, or Kubernetes.

- Linux
- macOS
- Windows with Cygwin or WSL
- Docker
- Kubernetes

PadoGrid requires `bash`, Maven 3.x and Git for building apps and bundles. Java is optional but it is required by most of the cluster products. The optional `jq` executable is also recommended for handling JSON values.

- Maven 3.x
- JDK 8+ (JDK 11+ required to run Hazelcast Management Center)
- `git`
- `bash`
- `curl`
- `jq`

✏️  *If you are using a PadoGrid container, then all the required software components are already installed.*

## 0.1. Linux, macOS, Windows (Cygwin, WSL)

❗*Make sure you have the software listed above installed before installing PadoGrid on your local machine.*

By default, PadoGrid installs in your home directory under the `~/Padogrid` directory. You can change this directory by running the `install_padogrid` script. For our tutorial, let's install it in the default directory. The following `curl` command silently installs PadoGrid and Hazelcast OSS.

```bash
curl -fsSL https://raw.githubusercontent.com/padogrid/padogrid/develop/padogrid-deployment/src/main/resources/common/bin_sh/install_padogrid | /bin/bash -s -- -no-stty -quiet -product hazelcast-oss
```

PadoGrid requires `bash` as you can see from the above installation command, which upon successful installation, adds a PadoGrid initialization command to `.bashrc`. Take a look at the `.bashrc` file.

```bash
cat ~/.bashrc
```

Output:

```console
...
# >>> padogrid initialization >>>
. /home/myhome/Padogrid/workspaces/myrwe/initenv.sh -quiet
# <<< padogrid initialization <<<
...
```

The `.bashrc` file sources in the default RWE, `myrwe`, which is automatically created by the installer. An RWE (Root Workspaces Environment) is a top directory in which your workspaces are stored. As you get familar with PadoGrid, you will likely be creating additional RWEs to organize your workspaces. We will walk through RWE in detail later.

To initialze PadoGrid, you can source in the `.bashrc` file or run `bash`. Remember that PadoGrid supports only `bash`.

```bash
# Initialize PadoGrid - if your shell is bash:
. ~/.bashrc

# Initialize PadoGrid - run bash:
bash
```

## 0.2. Docker

Launch the PadoGrid container with the required ports exposed as follows.

```bash
docker run --name padogrid --hostname padogrid -d \
           -p 8888:8888 -p 8080:8080 -p 7070:7070 -p 9090:9090 -p 3000:3000 \
           padogrid/padogrid
```

JupyterLab URL: <http://localhost:8888/lab/workspaces/myrwe>

You can also login to the container as follows.

```bash
docker exec -it padogrid /bin/bash
```

## 0.3. Podman

Launch the PadoGrid container with the required ports exposed as follows.

```bash
podman run --name padogrid --hostname padogrid -d \
           -p 8888:8888 -p 8080:8080 -p 7070:7070 -p 9090:9090 -p 3000:3000 \
           padogrid/padogrid
```

JupyterLab URL: <http://localhost:8888/lab/workspaces/myrwe>

You can also login to the container as follows.

```bash
podman exec -it padogrid /bin/bash
```

## 0.4. Kubernetes

Apply the `padogrid.yaml` maintained in the PadoGrid repo.

```bash
kubectl apply -f https://raw.githubusercontent.com/padogrid/padogrid/develop/padogrid-deployment/src/main/resources/common/k8s/padogrid.yaml
```

To login to PadoGrid via JupyerLab:

```bash
kubectl port-forward padogrid 8888:8888
```

JupyterLab URL: <http://localhost:8888/lab/workspaces/myrwe>

To login directly to the PadoGrid pod:

```
# Login to padogrid pod
kubectl exec -it padogrid -- bash
```

## 0.5. OpenShift

PadoGrid runs as a non-root user (padogrid/1001). To run a non-root user container in OpenShift, we need to add the project's `default` user to the `nonroot` SSC (Security Context Constraints).

```bash
# Create project
oc new-project padogrid

# Add the nonroot SCC to the 'default' user in the 'padogrid' project
oc adm policy add-scc-to-user nonroot system:serviceaccount:padogrid:default
```

Apply the `padogrid.yaml` maintained in the PadoGrid repo.

```bash
oc apply -f https://raw.githubusercontent.com/padogrid/padogrid/develop/padogrid-deployment/src/main/resources/common/k8s/padogrid.yaml
```

To login to PadoGrid via JupyerLab:

```bash
oc expose svc padogrid --port 8888
```

View the exposed route:

```bash
oc get route
```

Output:

```console
NAME       HOST/PORT                            PATH   SERVICES   PORT   TERMINATION   WILDCARD
padogrid   padogrid-padogrid.apps-crc.testing          padogrid   8888                 None
```

The `HOST/PORT` column shows the JupyterLab address.

JupyterLab URL: <http://padogrid-padogrid.apps-crc.testing>

To login directly to the PadoGrid pod, use the PadoGrid pod name:

```bash
oc get pods
```

Output:

```console
NAME                        READY   STATUS    RESTARTS   AGE
padogrid-556797b799-57cg4   1/1     Running   0          4m4s
```

Login:

```bash
# Login to padogrid pod
oc exec -it padogrid-556797b799-57cg4 -- bash
```

## 0.6 X Window System

✏️  *This section has been extracted from the [Running Bundles in Container](https://github.com/padogrid/padogrid/wiki/Running-Bundles-in-Container) section of the PadoGrid manual.*

If you are using Docker or Kubernetes, then you should also install and run an X11 server on your host OS so that you can display GUI apps from the PadoGrid container.

### From your host OS:

Download and install X11 by following one of the links below.

- macOS: [XQuartz](https://www.xquartz.org/)
- Windows: [Xming](http://www.straightrunning.com/XmingNotes/)
- Linux: Use the package manager to install X11 packages. For example, on Ubuntu, `sudo apt install xorg openbox`.

Upon completion of installing and running X11, list the display authorization entries. 

```bash
xauth list
```

Output:

```console
myhost.local:0  MIT-MAGIC-COOKIE-1  2a7842352bdc123b2cf92c01bf0e822b
```

Your output may contain more than one (1) entry. We need the one with the host name that the containers can access. For Docker Kubernetes and Minikube, you can use the entry as is. However, for CRC, you need to use the host OS IP address. For our example, we can replace the host name with its IP address (192.168.1.10) as follows.

```console
192.168.1.10:0  MIT-MAGIC-COOKIE-1  2a7842352bdc123b2cf92c01bf0e822b
```

### From inside the container

Copy/paste the display authorization entry to the `xauth add` command and set the `DISPLAY`  environment variable as follows.

```bash
touch ~/.Xauthority
xauth add 192.168.1.10:0  MIT-MAGIC-COOKIE-1  2a7842352bdc123b2cf92c01bf0e822b
export DISPLAY=192.168.1.10:0
```

## 0.7. Directory Layout

Let's take a look at the default directory where PadoGrid is installed.

If you installed PadoGrid on your host OS, then its default directory is `$HOME/Padogrid` and has the following directory layout.

```bash
tree -L 2 ~/Padogrid
```

You should see the directory structure similar to the following.

```console
~/Padogrid/
├── downloads
│   ├── hazelcast-5.3.2-slim.tar.gz
│   └── padogrid_0.9.28.tar.gz
├── products
│   ├── hazelcast-5.3.2-slim
│   └── padogrid_0.9.28
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
│   ├── hazelcast-5.3.2-slim
│   ├── hazelcast-management-center-5.3.2
│   └── padogrid_0.9.28
└── workspaces
    └── myrwe
```

- The `downloads` directory contains the downloads done by the `install_padogrid` command. PadoGrid distributions include this command so that you can use it to install additional products at any time.

- The `padogrid_start` script is a bootstrap script for Docker and Kubernetes. You can ignore that file.

- The `products` directory contains the installed products. Product installations are typically done by inflating the downloads. For example, the downloaded `hazelcast-5.3.2-slim.tar.gz` and `padogrid_0.9.28.tar.gz` distributions are inflated by `install_padogrid` in `products/hazelcast-5.3.2-slim` and `products/padogrid_0.9.28`, respectively.

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

## 0.8. Uninstalling PadoGrid

If you have installed PadoGrid on your local machine then you can uninstall it by removing the `~/PadoGrid` and `~/.padogrid` directories, and removing the PadoGrid initialization line in the `.bashrc` file.

Before you uninstall PadoGrid, kill all the cluster processes launched by PadoGrid.

```bash
# Kill all RWEs (-quiet suppresses prompts)
for rwe in $(list_rwes); do kill_rwe -quiet -all -rwe $rwe; done
```

Execute the following commands to completely remove PadoGrid including workspaces.

```bash
# Remove all PadoGrid directories
rm -rf ~/Padogrid ~/.padogrid

# Update .bashrc with backup
#macOS - create backup .bashrc0
sed -i 0 -e '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' \
         -e '/padogrid initialization/d' ~/.bashrc
#Linux - create backup .bashrc0
sed -i0 -e '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' \
        -e '/padogrid initialization/d' ~/.bashrc
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
