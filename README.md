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
# Switch to rwe-tutorial if you haven't done so already
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

We did not specify the workspace name, so by default the above command created the workspace using the bundle name, `bundle-none-tutorial-padogrid`.

Let's switch into the new workspace and view its contents.

```bash
switch_workspace bundle-none-tutorial-padogrid
show_workspace
```

You should see an empty workspace something like the following.

```console
Workspace:
   bundle-none-tutorial-padogrid [padogrid_0.9.21]

RWE Directory:
   /home/vagrant/Padogrid/workspaces/rwe-tutorial

Workspace Directory:
   /home/vagrant/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid

Workspace Type:
   local

Switch Command:
   switch_rwe rwe-tutorial bundle-none-tutorial-padogrid
```

## 5. Create a Hazelcast cluster

We have installed Hazelcast OSS when we installed PadoGrid. Let's create a Hazelcast cluster. The following command creates the default Hazelcast cluster named, `myhz`.

```bash
make_cluster -product -hazelcast
```

Now, switch into the `myhz` cluster and view its contents.

```bash
switch_cluster myhz
show_cluster
```

Output:

```console
         CLUSTER: myhz
     CLUSTER_DIR: /home/vagrant/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid/clusters/myhz
         PRODUCT: hazelcast
    CLUSTER_TYPE: hazelcast
             POD: local
        POD_TYPE: local
 Members Running: 0/2
      MC Running: 0/1
         Version: 5.1.4
```

By default, PadoGrid creates a cluster with two (2) members and one (1) Management Center instance. The cluster you created is a local cluster that is configured to run on your machine only. It will start both members and a Management Center on your machine.

## 6. Enable VMs

If you wish to distribute your workspace so that the cluster can run on remote machines, then you would need to provide the remote machine addresses. This can be done per cluster or per the entire workspace as described below.

### Enable VM workspace

In order to distribute a workspace, we need to enable VMs. This is typically done when we create a workspace by running `create_workspace`, but for us, since we already have a workspace created, let's configure the workspace instead.

```bash
cd_workspace
vi vmenv.sh
```

In the `vmenv.sh` file, enable VMs and enter your remote machine addresses as shown in the example below.

```bash
#
# Remote VM environment variables
#
# Set true to enable remote commands, false to disable remote commands.
VM_ENABLED="true"
# Enter a comma-separated VM host names with NO spaces. If VM_ENABLED is true then
# VM_HOSTS must be set, otherwise, VM_HOSTS and other VM_ variables are ignored.
VM_HOSTS="192.168.56.22,192.168.56.23,192.168.56.24,192.168.56.25"
# VM user name.
VM_USER="ec2-user"
```

If you are running cloud VMs like AWS EC2 instances then place the private key in the workspace directory. For example, the following copies your EC2 private key in the current workspace.

```bash
cd_workspace
cp /path/to/my/privateKey.pem .
# Make sure to make it read only
chmod 400 privateKey.pem
```

### Enable VM cluster

Every cluster you create, either Hazelcast or other clusters, the PadoGrid cluster settings are configured in the cluster's `etc/cluster.properties` file. This is the only file that is PadoGrid specific in terms of configuring clusters. All others are product specific and you refer the product documentation.

Let's edit the `cluster.properties` file to enable VMs.

```bash
cd_cluster
vi etc/cluster.properties
```

At the bottom of the `cluster.properties` file, enable VMs and enter your remote machine addresses as shown in the example below.

```properties
# Enable/disable VM cluster
vm.enabled=true
# A comma separated list of host names or addresses. IMPORTANT: No spaces allowed.
vm.hosts=192.168.56.22,192.168.56.23,192.168.56.24,192.168.56.25
# SSH user name. If not specified then defaults to the shell login session user name.
vm.user=vagrant
# Optional private key file path. e.g., a private '.pem' key file. If not specified
# it defaults to VM_PRIVATE_KEY_FILE defined in the workspace 'vmenv.sh' file.
#vm.privateKeyFile=/your/private/keyfile.pem
```

After you made the changes in the `cluster.properties`, provided that you have `ssh` access to the machines listed for the `vm.hosts` property, your cluster is now VM-enabled and ready to be deployed.

```bash
show_cluster
```

Output:

```console
         CLUSTER: myhz
    CLUSTER_DIR: /home/vagrant/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid/clusters/myhz
         PRODUCT: hazelcast
    CLUSTER_TYPE: hazelcast
      Deployment: VM
    MEMBER_COUNT: 4
 Members Running: 0/4
      MC Running: 0/1
         Version: 5.1.4
  Switch Cluster: switch_rwe rwe-tutorial bundle-none-tutorial-padogrid; switch_cluster myhz
```

## 7. Start cluster

Either you have a VM-enabled workspace or not, the PadoGrid commands work the same way. Let's start the cluster and Hazelcast Management Center.

```bash
# The '-all' option starts both members and management center
start_cluster -all
```

Once started, you can check the cluster status by running `show_cluster`

```bash
# Display short running status 
show_cluster

# Display detailed status
show_cluster -long
```

For Hazelcast, you can view the Management Center (MC) status separtely by running `show_mc` as follows.

```bash
show_mc
show_mc -long
```

You can click on the Management Center URL displayed to manage the Hazelcast cluster from the browser. PadoGrid closely follows the Hazelcast default settings. This means the default Hazelcast cluster name is **dev**, and not **myhz**. This, of course, can be changed easily in the Hazelcast configuration file, but for now, let's stick with **dev**.

- From the browser, select the "ENABLE" button for the "DEV MODE".
- Select the "Add" button under the "Cluster Connections" pane.
- Keep the default settings and select the "CONNECT" button in the "Connect Cluster/Connect Directly" page.
- From the "Cluster Connections" pane, select the "VIEW CLUSTER" button to open the management center.

Upon successful connection, you should see the Management Center view similar to the following image.

![Hazelcast Management Center](images/management-center.png)

## 8. Ingest data

PadoGrid includes several apps that are tailored to each product. The `perf_test` app is most commonly used for ingesting mock data into clusters. Let's install `perf_test` and ingest data.

```bash
# Create perf_test, without any option, it create 'perf_test'
create_app

# You can specify a different name using the '-name' option
create_app -name perf_test_hz

# For other apps, speciy the '-app' option. The following creates a grafana app instance
create_app -app grafana -name granfana
```

Assuming you have named `perf_test`, change directory to that app as follows. Note that apps do not have the `switch_` command at this time due to application specifics that need to be configured. This may change in the future as PadoGrid supports additional apps.

```bash
# Change directory to perf_test and view directory structure
cd_app perf_test
tree .
```

Output:

```console
.
├── README.md
├── bin_sh
│   ├── build_app
│   ├── clean_results
│   ├── create_csv
│   ├── listen_map
│   ├── read_cache
│   ├── setenv.sh
│   ├── subscribe_topic
│   ├── test_group
│   ├── test_ingestion
│   └── test_tx
├── etc
│   ├── group-cache.properties
│   ├── group-factory-er.properties
│   ├── group-factory.properties
│   ├── group-get.properties
│   ├── group-put-sleep.properties
│   ├── group-put.properties
│   ├── group-queue.properties
│   ├── group-rmap.properties
│   ├── group-rtopic.properties
│   ├── group-topic.properties
│   ├── group.properties
│   ├── hazelcast-client-blue.xml
│   ├── hazelcast-client-failover.xml
│   ├── hazelcast-client-green.xml
│   ├── hazelcast-client-k8s.xml
│   ├── hazelcast-client.xml
│   ├── hibernate.cfg-mysql.xml
│   ├── hibernate.cfg-postgresql.xml
│   ├── ingestion.properties
│   ├── log4j2.properties
│   ├── tx.properties
│   ├── v3
│   │   ├── hazelcast-client-blue.xml
│   │   ├── hazelcast-client-failover.xml
│   │   ├── hazelcast-client-green.xml
│   │   ├── hazelcast-client-k8s.xml
│   │   └── hazelcast-client.xml
│   ├── v4
│   │   ├── hazelcast-client-blue.xml
│   │   ├── hazelcast-client-failover.xml
│   │   ├── hazelcast-client-green.xml
│   │   ├── hazelcast-client-k8s.xml
│   │   └── hazelcast-client.xml
│   └── v5
│       ├── hazelcast-client-blue.xml
│       ├── hazelcast-client-failover.xml
│       ├── hazelcast-client-green.xml
│       ├── hazelcast-client-k8s.xml
│       └── hazelcast-client.xml
├── pom-hazelcast.xml
└── pom.xml
```

The `perf_test` app directory contains mostly configuration files along with scripts to run the application. By default, it is configured to connect to the **dev** Hazelcast cluster, so you can simply run any of the scripts to connect to the cluster you have started.

The `test_ingestion` script ingests PBM (Pharmacy Benefit Management) data into the cluster. If you run it without the `-run` option then it will only display ingestion information of the configured test cases, providing a preview.

```bash
cd bin_sh
./test_ingestion -run
```

The `test_tx` script performs transactions on co-located data ingested by `test_ingestion` using Hazelcast's Map Reduce services. It demonstrates how quickly PBM summary reports can be generated by parallelizing the work load to the members in the cluster.

```bash
./test_tx -run
```

If you need to measure Hazelcast performance that is aligned with how your application uses Hazelcast then you can run `test_group` to simulate your application workflows. A workflow is comprised of a series of Hazelcast data structure operations and application logic executions in a multi-threaded environment. `test_group` supports all of the Hazelcast data structure operations and simulates application logic executions by pausing between Hazelcast operations using the special `sleep` operation. You can create very complex workflows that represent your application before you have even created the application. All this without coding whatsoever.

```bash
./test_group -run
```

:pencil2: The performance metrics are placed in the `../results` directory for each `test_` script run.

There are many other features that are included in `perf_test`. The following is a list of core features. See [`perf_test` README.md](https://github.com/padogrid/padogrid/blob/develop/padogrid-deployment/src/main/resources/hazelcast/apps/perf_test/README.md) for details.

- Ingest mock data of any size
- Ingest transactional data of any size
- Ingest mock data with entity relationships (ER)
- Ingest mock data directly to databases
- Simulate complex appliation workflows that invoke Hazelcast operations without coding
- Measure Hazelcast latencies and throughputs in a multi-threaded user session environment

Once you have ingested data, you can view the data from the Management Center or run the `read_cache` script.

```bash
# Read the eligibility map
./read_cache eligibility

# Read the profile map
./read_cache profile

# Read the map1 map
./read_cache map1
```

We have been ingesting binary data as payload into Hazelcast. Let's ingest some meaningful data. This requires the `javafaker` library which we can install but running the `build_app` script.

```bash
./build_app
`` 

Upon completion of `build_app`, run `test_group` to ingest customer and order mock data as follows.

```bash
./test_group -run -prop ../etc/group-factory.properties
```

You can now view the data by running `read_cache` as follows. The mock data is stored in the `mw/customers` and `nw/orders` maps.

```bash
./read_cache nw/customers
./read_cache nw/orders
```

## 9. Install and run desktop app

PadoGrid includes a HazelcastDesktop app that supports PadoGrid's own HQL (Hazelcast Query Language). HQL is similar to SQL introduced in Hazelcast 5.1. Its query engine is entirely written using the Hazelcast API and unlike Hazelcast's SQL which only runs on Hazelcast 5.1+, HQL runs on Hazelcast 3.x, 4.x, and 5.x. It has some limitations such as no support for `limit` and select projections, however. If you can live with these limitations then it is a great tool for development, tests, and troubleshooting data problems.

Install and run the HazelcastDesktop.

```bash
install_padogrid -product hazelcast-desktop
update_products -product hazelcast-desktop
create_app -app desktop
cd_app desktop/bin_sh
./desktop
```

You can login with any user name and password. 

![Hazelcast Desktop Login](images/hazelcast-desktop-login.png)

Once logged in, select any of the maps shown in the left pane or enter a select statement in the HQL pane and hit Ctrl-Enter view data.

![Hazelcast Desktop](images/hazelcast-desktop.png)

## 10. Install and run Prometheus and Grafana

PadoGrid also supports Prometheus and Grafana for monitoring JMX metrics in real time. The `install_padogrid` command does not support these products at this time, however. You need to manually install and run them as described in the following link.

https://github.com/padogrid/padogrid/wiki/Hazelcast-Grafana-App

## 11. View workspaces

Let's now view the workspaces to see what we have created so far. 

```bash
show_workspace
```

Output:

```console
Workspace:
   bundle-none-tutorial-padogrid [padogrid_0.9.21]

RWE Directory:
   /home/vagrant/Padogrid/workspaces/rwe-tutorial

Workspace Directory:
   /home/vagrant/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid

Workspace Type:
   local

apps
├── desktop
├── grafana
└── perf_test

clusters
└── myhz

Switch Command:
   switch_rwe rwe-tutorial bundle-none-tutorial-padogrid
```

## 12. Summary

We have so far accomplished the following.

- Installed PadoGrid
- Created PadoGrid RWEs and workspaces
- Installed this bundle in a new RWE  
- Created and started a Hazelcast cluster and Management Center
- Created the `perf_test` app to ingest data
  - test_ingestion for PBM data
  - test_group for binary, customer and order mock data
  - read_cache to read data stored in the cluster
- Installed and executed HazelcastDesktop
- Installed and executed Prometheus and Grafana 


## 13. Stop cluster

We can now stop the Hazelcast cluster and Management Center.

```bash
# The '-all' option stops both the cluster and Management Center
stop_cluster -all
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
