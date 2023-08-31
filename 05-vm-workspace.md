![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 5. Enable VMs

✏️  *If you have access to one or more VMs, then we encourage you to try out the steps in this section. Otherwise, read through the steps to get familiar with how to distribute workspaces.*

If you wish to distribute your workspace so that the cluster can run on remote machines, then you would need to provide the remote machine addresses. This can be done per cluster or per the entire workspace as described below.

## 5.1. Enable VM workspace

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

## 5.2. Enable VM cluster

Every cluster you create, either Hazelcast or other products, the PadoGrid cluster settings are configured in the cluster's `etc/cluster.properties` file. This is the only file that is PadoGrid specific in terms of configuring clusters. All others are product specific and you follow the configuration instructions in their respective product documentation.

Let's edit the `cluster.properties` file to enable VMs.

```bash
cd_cluster
vi etc/cluster.properties
```

At the bottom of the `cluster.properties` file, enable VMs and enter your remote machine addresses as shown in the example below.

```properties
...
# Management Center host and port numbers
# Use the first IP for MC.
mc.host=192.168.56.22
...
# Enable/disable VM cluster
vm.enabled=true
# A comma separated list of host names or addresses. IMPORTANT: No spaces allowed.
# Here, we removed the first IP address from the list. It is used for MC.
vm.hosts=192.168.56.23,192.168.56.24,192.168.56.25
# SSH user name. If not specified then defaults to the shell login session user name.
vm.user=ec2-user
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
    CLUSTER_DIR: /Users/dpark/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid/clusters/myhz
         PRODUCT: hazelcast
    CLUSTER_TYPE: hazelcast
      Deployment: VM
    MEMBER_COUNT: 4
 Members Running: 0/4
      MC Running: 0/1
         Version: 5.3.2
  Switch Cluster: switch_rwe rwe-tutorial bundle-none-tutorial-padogrid; switch_cluster myhz
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
