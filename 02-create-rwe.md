![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 2. Create RWE

Let's create a new RWE from which we will conduct our tutorial. We do this by running `create_rwe`. The `create_rwe` command by default runs interactively prompting for inputs. To run it non-interactively, you need to specify the `-quiet` option.

✏️ Keep in mind that an RWE is a top directory in which workspaces are stored. Similarly, a workspace is a directory in which PadoGrid components are stored.

## 2.1. Interactive RWE

For our tutorial, let's run it non-interactively as shown below.

```bash
create_rwe -quiet -rwe rwe-tutorial
```

## 2.2. Non-interactive RWE

If you run `create_rwe` without the `-quiet` option then it will prompt for the following information.

❗ All paths must be absolute paths.

- Product installation path: `~/Padogrid/products/hazelcast-5.1.4-slim`
- RWE base path: `~/Padogrid/workspaces`
- RWE name: `rwe-tutorial`
- JAVA_HOME path: `<Java installation path>`
- Default workspace name: `myws`
- Default cluster name: `myhz`
- Enable VM: `false`

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

## 2.3. Switch RWE

To use the RWE you just created, we need to switch into that RWE. By switching, we are changing the current context. We can change the context of RWE, workspace, cluster, group, and pod. Once in the switched context, then you are in that context until you switch into another context. You can check yor current context by executing `pwd_*` commands.

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

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
