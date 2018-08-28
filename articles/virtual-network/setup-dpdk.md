---
title: DPDK auf einer Linux-VM in Azure | Microsoft-Dokumentation
description: Informationen zum Einrichten des DPDK auf einer Linux-VM
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: 205a1e399eadd268ffaa390a7ebb4397fda9feff
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444652"
---
# <a name="setup-dpdk-in-a-linux-virtual-machine"></a>Einrichten des DPDK auf einer Linux-VM

Data Plane Development Kit (DPDK) in Azure bietet ein schnelles Paketverarbeitungsframework auf Benutzerseite für leistungsintensive Anwendungen, die den VM-Kernelnetzwerkstapel umgehen.

Bei der herkömmlichen Paketverarbeitung mit dem Kernelnetzwerkstapel treten Interrupts auf. Immer wenn die Netzwerkschnittstelle eingehende Pakete empfängt, gibt es einen Kernelinterrupt, damit das Paket und der Kontextwechsel vom Kernel- zum Benutzerbereich verarbeitet werden. DPDK eliminiert Kontextwechsel und die interruptgesteuerte Methode zugunsten einer Implementierung im Benutzerbereich mit Poll Mode Driver (PMD), damit Pakete schnell verarbeitet werden können.

DPDK besteht aus einer Reihe von Benutzerbibliotheken, die Zugriff auf untergeordnete Ressourcen wie Hardware, logische Kerne, Speicherverwaltung und PMDs für Netzwerkadapter bieten.

DPDK kann auf Azure-VMs ausgeführt werden und unterstützt mehrere Betriebssystemverteilungen. Es bietet eine wichtige Leistungsdifferenzierung beim Implementieren von Netzwerkfunktionsvirtualisierungen in Form von virtuellen Netzwerkgeräten, z.B. virtuelle Router, Firewall, VPN, Lastenausgleich, Evolved Packet Core (EPC) und DoS-Anwendungen (Denial of Service).

## <a name="benefit"></a>Vorteil

**Mehr Pakete pro Sekunde**: Das Umgehen des Kernels und das Steuern der Pakete auf Benutzerseite reduziert die Zyklen durch Eliminierung des Kontextwechsels. Zudem wird die Rate der pro Sekunde verarbeiteten Pakete auf Linux-VMs in Azure verbessert.


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgenden Distributionen werden aus dem Azure-Katalog unterstützt:

| Linux-Betriebssystem     | Kernelversion        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-azure     |
| Ubuntu 18.04 | 4.15.0-1015-azure     |
| SLES 15      | 4.12.14-5.5-azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Benutzerdefinierte Kernelunterstützung**

Unter [Patches zum Erstellen eines Linux-Kernels für Azure](https://github.com/microsoft/azure-linux-kernel) finden Sie alle nicht aufgeführten Linux-Kernelversionen. Wenn Sie mehr erfahren möchten, wenden Sie sich an [azuredpdk@microsoft.com](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Unterstützung für Regionen

Alle Azure-Regionen unterstützen DPDK.

## <a name="prerequisites"></a>Voraussetzungen

Auf der Linux-VM muss der beschleunigte Netzwerkbetrieb aktiviert sein. Die VM sollte mindestens zwei Netzwerkschnittstellen haben, davon eine für die Verwaltung. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>Installieren von DPDK-Abhängigkeiten

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15"></a>SLES 15

**Azure-Kernel**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Standardkernel**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="setup-virtual-machine-environment-once"></a>Einmaliges Einrichten einer VM-Umgebung

1. [Laden Sie das aktuellste DPDK herunter.](https://core.dpdk.org/download) Azure erfordert Version 18.02 oder höher.
2. Erstellen Sie zuerst die Standardkonfiguration mit `make config T=x86_64-native-linuxapp-gcc`.
3. Aktivieren Sie Mellanox PMDs in der generierten Konfiguration mit `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
4. Kompilieren Sie mit `make`.
5. Installieren Sie mit `make install DESTDIR=<output folder>`.

# <a name="configure-runtime-environment"></a>Konfigurieren der Laufzeitumgebung

Führen Sie die folgenden Befehle nach dem Neustart einmalig aus:

1. Umfangreiche Seiten

   * Konfigurieren Sie umfangreiche Seiten, indem Sie den folgenden Befehl einmal für alle NUMA-Knoten ausführen:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   *  Erstellen Sie ein Verzeichnis für die Bereitstellung mit `mkdir /mnt/huge`.
   *  Stellen Sie umfangreiche Seiten mit `mount -t hugetlbfs nodev /mnt/huge` bereit.
   *  Überprüfen Sie mit `grep Huge /proc/meminfo`, ob umfangreiche Seiten reserviert sind.

     > [!NOTE]
     > Sie können die GRUB-Datei so ändern, dass umfangreiche Seiten beim Starten reserviert werden. Befolgen Sie dazu einfach die [Anweisungen](http://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) für DPDK. Die Anweisungen befinden sich unten auf der Seite. Wenn Sie in eine Linux-VM in Azure ausführen, ändern Sie Dateien stattdessen unter /etc/config/grub.d, um umfangreiche Seiten für Neustarts zu reservieren.

2. MAC- und IP-Adressen: Verwenden Sie `ifconfig –a`, um die MAC- und IP-Adresse der Netzwerkschnittstellen anzuzeigen. Die *VF*-Netzwerkschnittstelle und die *NETVSC*-Netzwerkschnittstelle haben dieselbe MAC-Adresse, allerdings hat nur die *NETVSC*-Netzwerkschnittstelle eine IP-Adresse. VF-Schnittstellen werden als untergeordnete Schnittstellen von NETVSC-Schnittstellen ausgeführt.

3. PCI-Adressen

   * Finden Sie mit `ethtool -i <vf interface name>` heraus, welche PCI-Adresse Sie für *VF* verwenden können.
   * Überprüfen Sie, ob testpmd nicht versehentlich das VF-PCI-Gerät für *eth0* übernimmt, wenn für *eth0* der beschleunigte Netzwerkbetrieb aktiviert ist. Wenn die DPDK-Anwendung versehentlich die Verwaltungsnetzwerkschnittstelle übernimmt, und dadurch die SSH-Verbindung unterbrochen wird, verwenden Sie die serielle Konsole, um die DPDK-Anwendung zu beenden oder die VM zu stoppen/starten.

4. Laden Sie bei jedem Neustart *ibuverbs* mit `modprobe -a ib_uverbs`. Nur für SLES 15: Laden Sie auch *mlx4_ib* mit `modprobe -a mlx4_ib`.

## <a name="failsafe-pmd"></a>Ausfallsicherer PMD

DPDK-Anwendungen müssen über den ausfallsicheren PMD ausgeführt werden, der in Azure verfügbar gemacht wird. Wenn die Anwendung direkt über den VF-PMD ausgeführt wird, empfängt sie nicht **alle** Pakete, die an die VM gerichtet sind, da einige Pakete über die synthetische Schnittstelle angezeigt werden. Das Ausführen über den ausfallsicheren PMD stellt sicher, dass die Anwendung alle für sie bestimmten Pakete erhält, und dass die Anwendung auch dann im DPDK-Modus fortgesetzt wird, wenn VF beim Warten des Hosts widerrufen wird. Weitere Informationen finden Sie in der [Ausfallsicherheits-PMD-Bibliothek](http://doc.dpdk.org/guides/nics/fail_safe.html) (in englischer Sprache).

## <a name="run-testpmd"></a>Ausführen von testpmd

Verwenden Sie `sudo` vor dem *testpmd*-Befehl für die Ausführung im Stammmodus.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Standard: Integritätsprüfung, Initialisierung des Ausfallsicherheitsadapters

1. Führen Sie die folgenden Befehle aus, um eine testpmd-Anwendung mit einem einzigen Port zu starten:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Führen Sie die folgenden Befehle aus, um eine testpmd-Anwendung mit zwei Ports zu starten:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   Bei der Ausführung mit mehr als 2 Netzwerkadaptern hat das `--vdev`-Argument dieses Muster: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Führen Sie nach dem Start `show port info all` aus, um die Portinformationen zu überprüfen. Es sollten ein oder zwei DPDK-Ports vom Typ „net_failsafe“ angezeigt werden (nicht *net_mlx4*).
4.  Starten Sie mit `start <port> /stop <port>` den Datenverkehr.

Die vorherigen Befehle starten *testpmd* im interaktiven Modus (empfohlen), um einige testpmd-Befehle zu testen.

### <a name="basic-single-sendersingle-receiver"></a>Standard: ein Absender/ein Empfänger

Mit den folgenden Befehlen werden Pakete in einem regelmäßigen Sekundentakt gedruckt:

1. Führen Sie auf Absenderseite den folgenden Befehl aus:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. Führen Sie auf Empfängerseite den folgenden Befehl aus:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Wenn Sie die vorherigen Befehle auf einer VM ausführen, ändern Sie *IP_SRC_ADDR* und *IP_DST_ADDR* in `app/test-pmd/txonly.c`, um der tatsächlichen IP-Adresse der VM zu entsprechen, bevor Sie kompilieren. Andernfalls werden die Pakete gelöscht, bevor sie den Empfänger erreichen.

### <a name="advanced-single-sendersingle-forwarder"></a>Erweitert: ein Absender/eine Weiterleitung
Mit den folgenden Befehlen werden Pakete in einem regelmäßigen Sekundentakt gedruckt:

1. Führen Sie auf Absenderseite den folgenden Befehl aus:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. Führen Sie auf Weiterleitungsseite den folgenden Befehl aus:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Wenn Sie die vorherigen Befehle auf einer VM ausführen, ändern Sie *IP_SRC_ADDR* und *IP_DST_ADDR* in `app/test-pmd/txonly.c`, um der tatsächlichen IP-Adresse der VM zu entsprechen, bevor Sie kompilieren. Andernfalls werden die Pakete gelöscht, bevor sie die Weiterleitung erreichen. Es ist nicht möglich, dass ein dritter Computer den weitergeleiteten Datenverkehr empfängt, weil die *testpmd*-Weiterleitung die Vermittlungsschichtadressen nicht verändert, es sei denn, Sie nehmen Codeänderungen vor.

## <a name="references"></a>Referenzen

* [EAL-Optionen](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options) (in englischer Sprache)
* [Testpmd-Befehle](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options) (in englischer Sprache)
