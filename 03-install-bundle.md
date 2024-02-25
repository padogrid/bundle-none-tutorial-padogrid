![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)

---

# 3. Install bundle

Let's install this tutorial bundle in the `rwe-tutorial` RWE.

```bash
# Switch to rwe-tutorial if you haven't done so already
switch_rwe rwe-tutorial

# Install this bundle
install_bundle -checkout bundle-none-tutorial-padogrid
```

---

We have just installed `bundle-none-tutorial-padogrid` in `rwe-tutorial` as a workspace. A bundle can be installed several ways.

- *As a workspace* - PadoGrid creates a new workspace and downloads the bundle contents in the new workspace. 
```bash 
install_bundle -download -workspace bundle-none-tutorial-padogrid
```

- As workspace components - PadoGrid installs the bundle in the current workspace. This might overwrite the existing components so it will prompt with a warning message for you to confirm.

```bash
install_bundle bundle-none-tutorial-padogrid
```

- *As a checked out workspace* - A git cloned workspace for you to make changes. This allows you to create and maintain your own online bundles.

```bash
install_bundle -checkout bundle-none-tutorial-padogrid
```

- *As a downloaded repo source distribution* - If your machine is behind a firewall and do not have access to the Internet, then you can manually download the bundle repo distribution from your browser and install it with the `install_bundle` command.

```bash
install_bundle bundle-none-tutorial-padogrid-master.zip
```

- *As a preview bundle* - You can preview bundles without installing them.

```bash
install_bundle -download -preview bundle-none-tutorial-padogrid
```

---

For our tutorial, we specified the `-checkout` option to checkout the bundle as a workspace. Also, we did not specify the workspace name, so by default, we created the workspace using the bundle name, `bundle-none-tutorial-padogrid`.

Let's switch into the new workspace and view its contents.

```bash
switch_workspace bundle-none-tutorial-padogrid
show_workspace
```

Output: You should see an empty workspace something like the following.

```console
Workspace:
   bundle-none-tutorial-padogrid [padogrid_1.0.0]

RWE Directory:
   /Users/dpark/Padogrid/workspaces/rwe-tutorial

Workspace Directory:
   /Users/dpark/Padogrid/workspaces/rwe-tutorial/bundle-none-tutorial-padogrid

Workspace Type:
   local

Switch Command:
   switch_rwe rwe-tutorial bundle-none-tutorial-padogrid
```

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
