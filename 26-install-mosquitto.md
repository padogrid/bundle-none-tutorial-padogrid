![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
---

# 26. Install Mosquitto

Mosquitto is a popular light-weight MQTT broker for acquiring real-time data edge devices. It runs as a stand-alone server and does not support high availability. It does support a single active backup server, but the direction of data redunancy can only be done from the primary server to the backup server. Making it bidirectional will end up in an inifinite loop.

PadoGrid takes a novel approach to bring high availability to Mosquitto and other broker products. It introduces the concept of *virtual clusters*, which can be formed by the client applications on the fly. A virtual cluster behaves like any other physical clusters. To understand how virtual clusters works, please see the [Mosquitto Overview](https://github.com/padogrid/padogrid/wiki/Mosquitto-Overview) section of the PadoGrid Manual.

In this tutorial, you will get familiar with MQTT virtual clusters.

First, let's install Mosquitto.

✏️  *If you are using a PadoGrid container, then Mosquitto is already installed so that you can skip this section.*

You can install a Mosquitto binary using your OS package manager. However, the binary versions are typically a few minor versions behind. For this tutorial, let's build the latest binary version instead.

```bash
# Install Mosquitto source code
install_padogrid -product mosquitto
```

Output:

Upon completion, you should see Mosquitto build instructions as follows.

```console
...
Mosquitto downloads do not include binaries. You can build binaries by
following the steps below.

macOS:
   cd /Users/myhome/Padogrid/products/mosquitto-2.0.17
   cd mosquitto-2.0.17
   export OPENSSL_ROOT_DIR=/usr/local/opt/openssl
   cmake install -DDOCUMENTATION=NO -DWITH_PLUGINS=NO -DWITH_WEBSOCKETS=YES .
   make
   sudo make install

Linux (Debian, Ubuntu):
   cd /home/myhome/Padogrid/products/mosquitto-2.0.17
   sudo apt install -y g++ libc-ares-dev libssl-dev libcjson-dev libwebsockets-dev
   make
   sudo make install

Linux (Alpine):
   cd /home/myhome/Padogrid/products/mosquitto-2.0.17
   sudo apk add g++ c-ares openssl-dev libwebsockets-dev
   sudo apk add make cmake git
   git clone https://github.com/DaveGamble/cJSON.git
   cd cJSON
   mkdir build
   cd build
   cmake ..
   make
   sudo make install
   sudo vi /etc/ld.so.conf
   # Add the following line in /etc/ld.so.conf:
      /usr/local/lib64
   cd ..
   vi config.mk
   # Enable websockets in config.mk
      WITH_WEBSOCKETS:=yes
   make
   sudo make install

Linux (RedHat,Oracle Linux, Fedora, CentOS):
   cd /home/myhome/Padogrid/products/mosquitto-2.0.17
   sudo dnf install -y cmake gcc-c++ openssl-devel c-ares-devel
   git clone https://github.com/DaveGamble/cJSON.git
   cd cJSON
   mkdir build
   cd build
   cmake ..
   make
   sudo make install
   sudo vi /etc/ld.so.conf
   # Add the following line in /etc/ld.so.conf:
      /usr/local/lib64
   sudo /sbin/ldconfig
   cd ../..
   git clone https://github.com/warmcat/libwebsockets.git
   cd libwebsockets
   cmake .
   make
   sudo make install
   cd ..
   vi config.mk
   # Enable websockets in config.mk
      WITH_WEBSOCKETS:=yes
   make
   sudo make install

If you are unable to build, then you can install a binary package by following
the instructions in the following link:

   https://mosquitto.org/download/

PadoGrid Installation complete.
```

The installer's output shows how to compile Mosquitto in OS variants. Choose the one that matches your host OS and build the Mosquitto binary.

Upon completion of installing Mosquitto, update the product in PadoGrid.

```bash
update_padogrid -product mosquitto
```

Output:

```console
Products Directoy:
   /root/Padogrid/products

The following product versions are found in the products directory.
The current workspace versions are highlighted.

Mosquitto
   [0] (none)
   [1] 2.0.17
Enter a version to add [0]: 1
```

You should see the Mosquitto version you just compiled. Select that version as shown above.

---

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*PadoGrid*](https://github.com/padogrid) | [*Catalogs*](https://github.com/padogrid/catalog-bundles/blob/master/all-catalog.md) | [*Manual*](https://github.com/padogrid/padogrid/wiki) | [*FAQ*](https://github.com/padogrid/padogrid/wiki/faq) | [*Releases*](https://github.com/padogrid/padogrid/releases) | [*Templates*](https://github.com/padogrid/padogrid/wiki/Using-Bundle-Templates) | [*Pods*](https://github.com/padogrid/padogrid/wiki/Understanding-Padogrid-Pods) | [*Kubernetes*](https://github.com/padogrid/padogrid/wiki/Kubernetes) | [*Docker*](https://github.com/padogrid/padogrid/wiki/Docker) | [*Apps*](https://github.com/padogrid/padogrid/wiki/Apps) | [*Quick Start*](https://github.com/padogrid/padogrid/wiki/Quick-Start)
