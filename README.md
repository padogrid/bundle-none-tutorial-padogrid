![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# PadoGrid Tutorial

This tutorial bundle covers PadoGrid essentials.

## Installing Bundle

```bash
install_bundle -checkout bundle-none-tutorial-padogrid
```

## Use Case

PadoGrid is a productivity toolkit for managing user workspaces in the server-side. It is commonly used for creating distributed workspaces on your laptop to manage various clustering products running locally and remotely. Managing clustering (or data grid) products such as GemFire, Hazelcast, Redis, Coherence, Spark,Kafka, Hadoop, etc. is a complex task that often requires development and maintenance of custom scripts. Each product comes with a simple set of scripts primarily for developers running clusters on a single node thwarting their pratical use. This task multiplies as you include more products in your system architecture. Learning each product's scripts is a tedious and time consuming task.

PadoGrid solves this problem by providing a single, unified set of commands for managing data grid products. The same set of commands applies to all the [supported data grid products](https://github.com/padogrid/padogrid/wiki/Supported-Data-Grid-Products-and-Downloads). For products that are not supported, their support is included in online bundles instead. Each bundle represents an end-to-end use case and a turnkey solution that you can simply install and run. Including this bundle, there are numerous [public online bundles](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) that are readily available to you.

PadoGrid was built from the beginning to bring the concept of *distributed workspaces* to practice by allowing you to create sandbox environments on the fly. PadoGrid isolates each workspace that you create from others so that you can focus on the applications that will be running only on your workspace. The bundles are the direct results of this concept as they must be run in isolated environments to prevent workspace conflicts. In this bundle, we will explore some of the commonly used PadoGrid commands to understand the benefits of having distributed workspaces. 

## Required Software

- Maven 3.x
- Git
- jq

## 0. Install PadoGrid

By default, PadoGrid installs in your home directory under the `~/Padogrid` directory. You can change this directory by running the `install_padogrid` script. For our tutorial, let's install it in the default directory. The following `curl` command silently installs PadoGrid and Hazelcast OSS.

```bash
curl -fsSL https://raw.githubusercontent.com/padogrid/padogrid/develop/padogrid-deployment/src/main/resources/common/bin_sh/install_padogrid | /bin/bash -s -- -no-stty -quiet -product hazelcast-oss
```

Upon installation, execute the following to intialize PadoGrid. Note that PadoGrid only runs in `bash`. If `bash` is not your default shell, then you must manually run `bash` to intialize PadoGrid.

```bash
. ~/Padogrid/workspaces/myrwe/initenv.sh -quiet
echo ". ~/Padogrid/workspaces/myrwe/initenv.sh -quiet" >> ~/.bashrc
```

Let's take a look at the default directory where PadoGrid is stalled.

```bash
tree -L 2 ~/Padogrid
```

You should see the directory structure similar to the following.

```console
~/Padogrid/
├── downloads
│   ├── hazelcast-5.1.4-slim.tar.gz
│   └── padogrid_0.9.21.tar.gz
├── products
│   ├── hazelcast-5.1.4-slim
│   └── padogrid_0.9.21
├── snapshots
└── workspaces
    └── myrwe
```

The `downloads` directory stores all downloads done by the `install_padogrid` command, which you have previously executed using `curl`. PadoGrid distributions include this command so that you can use it to install additional products at any time.

The `products` directory contains the inflated product home directories. For example, `hazelcast-5.1.4-slim.tar.gz` and `padogrid_0.9.21.tar.gz` downloaded by `install_padogrid` are inflated in `products/hazelcast-5.1.4-slim` and `products/padogrid_0.9.21`, respectively.

The `snapshots` directory is reserved for future use. PadoGrid snapshot builds are also available and installable using `install_padogrid`. Note that the snapshots are currently installed in the `products` directory, not this directory. This may change in the future.

The `workspaces` directory contains user workspaces. The `workspaces/myrwe` directory is the default RWE (Root Workspaces Environment) that contains the default `myws` workspace created by `install_padogrid`. We will go over the contents shortly in this tutorial.

In addition to `~/Padogrid`, PadoGrid creates the `~/.padogrid` directory store workspace metadata as shown below. We will touch up on the use of this directory later.

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
            ├── workspaceenv.sh
            └── clusters
                └── myhz
                    └── clusterenv.sh
```

### Unstalling PadoGrid

PadoGrid is unstalled by removing the `~/PadoGrid` and `~/.padogrid` directories, and removing the PadoGrid initialization line in the `.bashrc` file. The following completely removes PadoGrid.

```bash
# Remove all PadoGrid directories
rm -rf ~/Padogrid ~/.padogrid

# Update .bashrc with backup
#macOS - create backup .bashrc0
sed -i 0 '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' ~/.bashrc
#Linux - create backup .bashrc0
sed -i0 '/Padogrid\/workspaces\/myrwe\/initenv.sh/d' ~/.bashrc
```

:pencil2: The above removes PadoGrid and all PadoGrid installed products and workspaces. If you have PadoGrid clusters and applications running, then make sure you stop them first before removing PadoGrid.

## 1. PadoGrid Help

PadoGrid has over 100 CLI sub-commands. Each of these commands has `bash` auto-completion enabled so that you can view and select their options by entering the `tab` key. Try typing the following.

```bash
# Type padogrid and hit the tab key twice
padogrid <tab><tab>
```

Output:

```console
Display all 100 possibilities? (y or n)
-?                  create_workspace    pwd_workspace       start_jupyter
-product            find_padogrid       remove_app          start_member
-rwe                help_padogrid       remove_cluster      start_pod
-version            install_bundle      remove_docker       start_workspace
add_cluster         install_padogrid    remove_group        stop_cluster
add_member          kill_cluster        remove_k8s          stop_group
add_node            kill_group          remove_locator      stop_jupyter
build_pod           kill_member         remove_member       stop_member
cd_app              kill_rwe            remove_node         stop_pod
cd_cluster          kill_workspace      remove_pod          stop_rwe
cd_docker           list_apps           remove_workspace    stop_workspace
cd_k8s              list_clusters       show_bundle         switch_cluster
cd_pod              list_docker         show_cluster        switch_pod
cd_rwe              list_groups         show_group          switch_rwe
cd_workspace        list_k8s            show_jupyter        switch_workspace
change_version      list_pods           show_log            uninstall_product
clean_cluster       list_rwes           show_pod            update_products
create_app          list_workspaces     show_products       vm_copy
create_bundle       make_cluster        show_rwe            vm_deploy_bundle
create_cluster      open_jupyter        show_workspace      vm_deploy_padogrid
create_docker       open_vscode         shutdown_cluster    vm_download
create_group        pwd_cluster         shutdown_rwe        vm_exec
create_k8s          pwd_group           shutdown_workspace  vm_install
create_pod          pwd_pod             start_cluster       vm_sync
create_rwe          pwd_rwe             start_group         vm_test
```

Each command has a man page providing usage details.

```bash
man padogrid
man show_rwe
man start_cluster
```

The same man page details that are tailored to the current PadoGrid workspace environment can be displayed by specifying the `-?` option.

```bash
padogrid -?
padogrid show_rwe -?
padogrid start_cluster -?
padogrid stop_cluster -?
```

Each sub-command runs on their own such that you don't need to start with the `padogrid` command to run them.

```bash
show_rwe -?
start_cluster -?
stop_cluster -?
```

Try hitting the tab key on some of the commands.

```bash
show_cluster <tab><tab>
make_cluster <tab><tab>
vm_sync <tab><tab>
```

PadoGrid commands are grouped and identifiable by prefixes and postfixes. Most of the commands begin with the following verbs.

- `add_` (add component)
- `build_` (build components)
- `cd_` (change directory)
- `create_` (create components)
- `find_` (find artifacts)
- `install_` (install components)
- `kill_` (kill components)
- `list_` (list components)
- `make_` (make components)
- `pwd_` (present working component or current context. `switch_` sets the current context)
- `open_` (open components)
- `remove_` (remove components)
- `show_` (show components)
- `stop_` (stop components)
- `shutcown_` (gracefully shutdown components)
- `start_` (start components)
- `shutcown_` (gracefully shutdown components)
- `switch_` (switch context and change directory)
- `uninstall_` (uninstall components)
- `update_` (update components)
- `cp_` (Hazelcast CP commands)
- `t_`(tool commands)
- `vm_`(vm specific commands)


## 1. View PadoGrid envrionment

When you installed PadoGrid, it created the default RWE, `myrwe`, and workspace, `myws`. You can view the PadoGrid workspace environment by executing `padogrid` as follows.

```bash
padogrid
```

Output:

```console
.______      ___       _______   ______     _______ .______       __   _______ ™
|   _  \    /   \     |       \ /  __  \   /  _____||   _  \     |  | |       \
|  |_)  |  /  ^  \    |  .--.  |  |  |  | |  |  __  |  |_)  |    |  | |  .--.  |
|   ___/  /  /_\  \   |  |  |  |  |  |  | |  | |_ | |      /     |  | |  |  |  |
|  |     /  _____  \  |  '--'  |  '--'  | |  |__| | |  |\  \----.|  | |  '--'  |
| _|    /__/     \__\ |_______/ \______/   \______| | _| '._____||__| |_______/
Copyright 2020-2022 Netcrest Technologies, LLC. All rights reserved.
Version: v0.9.21
 Manual: https://github.com/padogrid/padogrid/wiki
Bundles: https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md

Root Workspaces Environments (RWEs)
-----------------------------------
/opt/padogrid/workspaces
└── myrwe
    └── myws [padogrid_0.9.21]
```

An RWE (Root Workspaces Environment) is a workspace root directory that contains one or more workspaces. You can have more than one RWEs, each containing their own set of workspaces. A workspace contains PadoGrid components such as apps, clusters, Docker, groups, Kubernetes, and PadoGrid pods (Vagrant VMs). We will explore all the components individually.

By default, PadoGrid initailly installs the RWE named, `myrwe`, and created the workspace named, `myws` in the RWE, `myrwe`.

You can also view each workspace component using the `show_*` command.

```bash
padogrid show_rwe
padogrid show_workspace
padogrid show_cluster
```

Or without the `padogrid` command. For the remaining of the tutorial, we will use sub-commands without the `padogrid` command.

```bash
show_rwe
show_workspace
show_cluster
```

## 2. Create a new RWE

Let's create a new RWE from which we will conduct our tutorial. We do this by running `create_rwe`. The `create_rwe` command by default runs interactively prompting for inputs. To run it non-interactively, you need to specify the `-quiet` option.

### 2.1. Interactive RWE

For our tutorial, let's run it non-interactively as shown below.

```bash
create_rwe -quiet -rwe rwe-tutorial
```

### 2.2. Non-interactive RWE

:exclamation: **Do not run this section. This section is for your information only.**

Optionally, if you run `create_rwe` without the `-quiet` option then it will prompt for the following information.

:exclamation: All paths must be absolute paths.

- Product installation path: ~/Padogrid/products/hazelcast-5.1.4-slim
- RWE base path: ~/Padogrid/workspaces
- RWE name: rwe-tutorial
- JAVA_HOME path: <Java installation path>
- Default workspace name: myws
- Default cluster name: myhz
- Enable VM: false

The following shows an example.

```bash
create_rwe
```

Output:

```console
Enter the default product home path. Leave blank to skip. The supported products are
[geode gemfire hazelcast snappydata coherence redis spark kafka hadoop]
[]:
/Users/dpark/Padogrid/products/hazelcast-5.1.4-slim
Product selected: hazelcast
Enter the RWE home path where your RWEs will be stored.
[/Users/dpark/Padogrid/workspaces]:

Enter a new RWE name []: rwe-tutorial

Enter Java home path. Leave blank to skip.
[/Library/Java/JavaVirtualMachines/jdk1.8.0_311.jdk/Contents/Home]:

Enter product (hazelcast) home directory path. Leave blank to skip.
[/Users/dpark/Padogrid/products/hazelcast-slim-5.1.4]:

Enter default workspace name [myws]:
Enter default cluster name [myhz]:
Enable VM? Enter 'true' or 'false' [false]:

Creating an RWE as follows...
            RWE Home: /Users/dpark/Padogrid/workspaces
            RWE Name: rwe-tutorial
           JAVA_HOME: /Library/Java/JavaVirtualMachines/jdk1.8.0_311.jdk/Contents/Home
        Product Home: /Users/dpark/Padogrid/products/hazelcast-5.1.4-slim
   Default Workspace: myws
     Default Cluster: myhz
          VM Enabled: false
Enter 'c' to continue, 'r' to re-enter, 'q' to quit: c
```

## 3. Switch RWE

To use the RWE you just created, we need to switch into that RWE. By switching, we are changing the current context. We can change context of RWE, workspace, cluster, group, and pod. Once in the switched context, then you are in that context until you switch into another context. You can check yor current context by executing `pwd_*` commands.

```bash
# Check current context (this outputs myrwe)
pwd_rwe

# Switch RWE
switch_rwe rwe-tutorial

# Check current context (this outputs rwe-tutorial)
pwd_rwe
```

If you need to execute commands outside of your current context then you can specify the component options. For example, the following example displays the `myrwe` components.

```bash
# View myrwe components
show_rwe -rwe myrwe

# View current context RWE (rwe-tutorial)
show_rwe
```

All other components follow the same command arugment convention. For example, the following commands display `myws`.

```bash
# View current workspace
show_workspace

# View myws workspace
show_workspace -workspace myws
```

## 4. Install this bundle

Let's install this bundle in the `rwe-tutorial` RWE.

```bash
# Switch to rwe-tutorial if you haven't done so
switch_rwe rwe-tutorial

# Install this bundle
install_bundle -checkout bundle-none-tutorial-padogrid
```

We have just installed `bundle-none-tutorial-padogrid` in `rwe-tutorial` as a workspace. A bundle can be installed several ways.

- As a workspace - PadoGrid creates a new workspace and downloads the bundle contents in the new workspace. 
- As workspace components - PadoGrid installs the bundle in the current workspace. This might overwrite existing components so it will prompt with a warning message for you to confirm.
- As a checked out workspace - A git cloned workspace for you to make changes. This allows you to create and maintain your own online bundles.
- Downloaded repo source distribution - If your mahcine is behind a firewall and do not have access to the Internet, then you can manually download the bundle repo distribution from your browser and install it with the `install_bundle` command.
- Preview bundle - Before you install the bundle, you can preview what it contains.

We did not specify the workspace name so by default the above command created the workspace using the bundle name, `bundle-none-tutorial-padogrid`.

Let's switch into the new workspace and view its contents.

```bash
switch_workspace bundle-none-tutorial-padogrid
```


## References

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
