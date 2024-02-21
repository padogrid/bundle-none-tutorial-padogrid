![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 1. PadoGrid Help

PadoGrid has over 100 CLI sub-commands. Each of these commands has `bash` auto-completion enabled so that you can view and select their options by entering the `tab` key. Try typing the following.

```bash
# Type padogrid and hit the tab key twice
padogrid <tab><tab>
```

Output:

```console
Display all 104 possibilities? (y or n)
-?                  help_padogrid       remove_docker       stop_cluster
-product            install_bundle      remove_group        stop_group
-rwe                install_padogrid    remove_k8s          stop_jupyter
-version            install_rwe         remove_locator      stop_member
add_cluster         kill_cluster        remove_member       stop_pod
add_member          kill_group          remove_node         stop_rwe
add_node            kill_member         remove_pod          stop_workspace
build_pod           kill_rwe            remove_workspace    switch_cluster
cd_app              kill_workspace      show_bundle         switch_group
cd_cluster          list_apps           show_cluster        switch_pod
cd_docker           list_clusters       show_group          switch_rwe
cd_k8s              list_docker         show_jupyter        switch_workspace
cd_pod              list_groups         show_log            uninstall_padogrid
cd_rwe              list_k8s            show_padogrid       update_padogrid
cd_workspace        list_pods           show_pod            vc_publish
clean_cluster       list_rwes           show_rwe            vc_start
create_app          list_workspaces     show_workspace      vc_subscribe
create_bundle       open_jupyter        shutdown_cluster    vm_copy
create_cluster      open_vscode         shutdown_rwe        vm_deploy_bundle
create_docker       pwd_cluster         shutdown_workspace  vm_deploy_padogrid
create_group        pwd_group           start_cluster       vm_download
create_k8s          pwd_pod             start_group         vm_exec
create_pod          pwd_rwe             start_jupyter       vm_install
create_rwe          pwd_workspace       start_member        vm_show_padogrid
create_workspace    remove_app          start_pod           vm_sync
find_padogrid       remove_cluster      start_workspace     vm_test
```

Each command has a man page providing usage details.

```bash
man padogrid
man show_rwe
man start_cluster
```

The same man pages that are tailored to the current PadoGrid workspace environment can be displayed by specifying the `-?` option.

```bash
padogrid -?
padogrid show_rwe -?
padogrid start_cluster -?
padogrid stop_cluster -?
```

Each sub-command runs on their own such that the `padogrid` command is not required.

```bash
show_rwe -?
start_cluster -?
stop_cluster -?
```

Try hitting the tab key on some of the commands.

```bash
show_cluster <tab><tab>
create_cluster <tab><tab>
vm_sync <tab><tab>
```

PadoGrid commands are bash scripts grouped and identifiable by prefixes and postfixes as shown below.

Prefixes
   - `cp_` (Hazelcast CP Subsystem), `t_` (tools), `vm_` (VMs).

Postfixes
   - `_app`, `_bundle`, `_cluster`, `_datanode`, `_docker`, `_group`, `_jupyter`, `_k8s`, `_leader`, `_locator`, `_master`, `_member`, `_namenode`, `_pod`, `_rwe`, `_vm`, `_vscode`, `_worker`, `_workspace`.

The sub-commands begin with the following verbs.

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
- `vc`(virtual cluster specific commands)
- `vm_`(vm specific commands)


## 1.1. View PadoGrid environment

When you installed PadoGrid, the default RWE, `myrwe`, and workspace, `myws` are automatically created. You can view the PadoGrid workspace environment by executing `padogrid` as follows.

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
Copyright 2020-2023 Netcrest Technologies, LLC. All rights reserved.
Version: v0.9.28
 Manual: https://github.com/padogrid/padogrid/wiki
Bundles: https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md

Root Workspaces Environments (RWEs)
-----------------------------------
/Users/dpark/Padogrid/workspaces
└── myrwe
    └── myws [padogrid_0.9.28]
```

An RWE (Root Workspaces Environment) is a workspace root directory that contains one or more workspaces. You can have more than one RWE, each containing their own set of workspaces. A workspace contains PadoGrid components such as apps, clusters, Docker, groups, Kubernetes, and PadoGrid pods (Vagrant VMs). We will explore all the components individually.

By default, PadoGrid initially installs the RWE named, `myrwe`, and creates the workspace named, `myws` in that RWE.

You can also view each PadoGrid component using the `show_*` commands.

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

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
