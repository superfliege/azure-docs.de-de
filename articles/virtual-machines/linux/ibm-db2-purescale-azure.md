---
title: IBM Db2 pureScale in Azure
description: In diesem Artikel wird eine Architektur zum Ausführen einer IBM Db2 pureScale-Umgebung in Azure gezeigt.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 5a7ae96a3730df2d0dae04dde6d7609ce69cdee5
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51978117"
---
# <a name="ibm-db2-purescale-on-azure"></a>IBM Db2 pureScale in Azure

Die IBM Db2 pureScale-Umgebung bietet eine Datenbank-Clusterlösung für Azure mit hoher Verfügbarkeit und Skalierbarkeit unter Linux-Betriebssystemen. In diesem Artikel wird eine Architektur zum Ausführen von Db2 pureScale in Azure gezeigt.

## <a name="overview"></a>Übersicht

Unternehmen haben lange Zeit herkömmliche RDBMS-Plattformen (relationales Datenbank-Managementsystem) verwendet, um Anforderungen der Onlinetransaktionsverarbeitung (OLTP) zu erfüllen. Heutzutage führen viele eine Migration der Mainframe-basierten Datenbankumgebungen zu Azure durch, um so die Kapazität erweitern, Kosten senken und eine stabile Betriebskostenstruktur bewahren zu können.

Migration ist häufig der erste Schritt zur Modernisierung einer veralteten Plattform. Beispielsweise hat ein Unternehmen vor Kurzem seine IBM Db2-Umgebung unter z/OS auf IBM Db2 pureScale in Azure rehostet. Zwar ist dies nicht mit der ursprünglichen Umgebung identisch, doch bietet IBM Db2 pureScale unter Linux ähnliche Hochverfügbarkeit und Skalierbarkeitsfeatures wie IBM Db2 für z/OS, das in einer Parallel Sysplex-Konfiguration auf dem Mainframe ausgeführt wird.

Dieser Artikel beschreibt die Architektur, die für diese Azure-Migration verwendet wurde. Zum Testen der Konfiguration wurde Red Hat Linux 7.4 verwendet. Diese Version ist im Azure Marketplace verfügbar. Prüfen Sie vor dem Auswählen einer Linux-Distribution die derzeit unterstützten Versionen. Ausführliche Informationen finden Sie in der Dokumentation zu [IBM Db2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) und [GlusterFS](https://docs.gluster.org/en/latest/).

Dieser Artikel dient lediglich als Ausgangspunkt für Ihren Db2-Implementierungsplan. Zwar sind Ihre Geschäftsanforderungen unterschiedlich, doch gilt das gleiche grundlegende Muster. Dieses Architekturmuster kann auch für Anwendungen zur analytischen Onlineverarbeitung (OLAP) in Azure verwendet werden.

In diesem Artikel werden weder die Unterschiede noch mögliche Migrationsaufgaben zum Verschieben einer IBM Db2 für z/OS-Datenbank zu IBM Db2 pureScale unter Linux behandelt. Es werden auch keine Schätzungen für entsprechende Größenordnungen oder Workloadanalysen für den Übergang von Db2 z/OS zu Db2 PureScale gemacht. Damit Sie sich für die optimale Db2 pureScale-Architektur für Ihre Umgebung entscheiden können, wird dringend empfohlen, eine umfassende Schätzung der Größenordnung auszuführen und eine Hypothese aufzustellen. Stellen Sie neben anderen Faktoren auf dem Quellsystem sicher, dass Db2 z/OS Parallel Sysplex mit Datenfreigabearchitektur, Coupling Facility-Konfiguration und DDF-Nutzungsstatistiken beachtet werden.

> [!NOTE]
> In diesem Artikel wird eine Methode der Db2-Migration beschrieben, doch gibt es auch noch andere. Beispielsweise kann Db2 pureScale auch in virtualisierten lokalen Umgebungen ausgeführt werden. IBM unterstützt Db2 auf Microsoft Hyper-V in verschiedenen Konfigurationen. Weitere Informationen finden Sie unter [Virtualisierungsarchitektur von Db2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) im IBM Knowledge Center.

## <a name="architecture"></a>Architektur

Zur Unterstützung von Hochverfügbarkeit und Skalierbarkeit in Azure kann eine Architektur mit gemeinsam genutzten Daten und horizontaler Skalierung für Db2 pureScale verwendet werden. Die folgende Beispielarchitektur wurde für unsere Kundenmigration verwendet.

![](media/db2-purescale-on-azure/pureScaleArchitecture.png "Db2 pureScale auf virtuellen Azure-Computern – Speicher und Netzwerk")


Das Architekturdiagramm zeigt die logischen Ebenen, die für einen Db2 pureScale-Cluster erforderlich sind. Dazu gehören virtuelle Computer für Client, Verwaltung, Caching, Datenbank-Engine und freigegebenen Speicher. Zusätzlich zu den Datenbank-Engine-Knoten enthält das Diagramm auch zwei Knoten, die für die sogenannten Cluster-Caching-Funktionen (CFs) verwendet werden. Mindestens zwei Knoten werden für die Datenbank-Engine selbst verwendet. Ein Db2-Server, der einem pureScale-Cluster angehört, wird als Member bezeichnet. Der Cluster ist über iSCSI mit einem GlusterFS-Cluster mit freigegebenem Speicher und drei Knoten verbunden, um Speicher für horizontale Skalierung und Hochverfügbarkeit zu bieten. Db2 pureScale wird auf virtuellen Azure-Computern installiert, auf denen Linux ausgeführt wird.

Dieser Ansatz dient einfach als Vorlage, die Sie entsprechend der für Ihre Organisation erforderlichen Größe und Skalierung anpassen können und die auf Folgendem basiert:

-   Mindestens zwei Datenbankmember werden mit mindestens zwei CF-Knoten kombiniert, die einen globalen Pufferpool (GBP) für freigegebenen Speicher und globale Sperren-Manager-Dienste (GLM) verwalten, um den freigegeben Zugriff und Sperrkonflikte zwischen mehreren aktiven Membern zu steuern. Ein CF-Knoten fungiert als primärer Knoten und der andere als sekundärer oder CF-Failoverknoten. Um das Erstellen einer Umgebung zu vermeiden, die eine einzelne Fehlerquelle aufweist, sind mindestens vier Knoten für einen Db2 pureScale-Cluster erforderlich.

-   Freigegebener Hochleistungsspeicher (in P30-Größe in Abbildung 1 dargestellt), der von jedem der GlusterFS-Knoten verwendet wird.

-   Hochleistungsnetzwerk für die Datenmember und freigegebenen Speicher.

### <a name="compute-considerations"></a>Computeaspekte

Bei dieser Architektur werden die Anwendungs-, Speicher- und Datenebenen auf virtuellen Azure-Computern ausgeführt. Die [Setupskripts für die Bereitstellung](http://aka.ms/db2onazure) erstellen Folgendes:

-   Einen Db2 pureScale-Cluster. Der Typ der in Azure benötigten Computeressourcen hängt von der jeweiligen Einrichtung ab. Im Allgemeinen können zwei Ansätze verwendet werden:

    -   Verwendung eines Netzwerks in HPC-Ausführung (High-Performance Computing) mit mehreren Knoten, bei dem mehrere kleine bis mittlere Instanzen auf freigegebenen Speicher zugreifen. Für diesen HPC-Konfigurationstyp bieten arbeitsspeicheroptimierte [virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) der E-Serie oder L-Serie in Azure die erforderliche Computeleistung.

    -   Verwendung weniger großer VM-Instanzen für die Daten-Engines. Bei großen Instanzen sind die größten arbeitsspeicheroptimierten virtuellen Computer der [M-Serie](https://azure.microsoft.com/pricing/details/virtual-machines/series/) ideal für hohe In-Memory-Workloads geeignet, doch kann je nach Größe der logischen Partition (LPAR), die zum Ausführen von Db2 verwendet wird, eine dedizierte Instanz erforderlich sein.

-   Die Db2-CF verwendet arbeitsspeicheroptimierte virtuelle Computer, z.B. der E-Serie oder L-Serie.

-   Der GlusterFS-Speicher verwendet virtuelle Computer des Typs Standard\_DS4\_v2, auf denen Linux ausgeführt wird.

-   Eine GlusterFS-Jumpbox ist ein virtueller Computer des Typs Standard\_DS2\_v2, auf dem Linux ausgeführt wird.

-   Der Client ist ein virtueller Computer des Typs Standard\_DS3\_v2, auf dem Windows ausgeführt wird (für Tests).

-   Ein Zeugenserver ist ein virtueller Computer des Typs Standard\_DS3\_v2, auf dem Linux ausgeführt wird (für Db2 pureScale).

> [!NOTE]
> Es sind mindestens zwei Db2-Instanzen in einem Db2 pureScale-Cluster erforderlich. Eine Cache-Instanz und Sperren-Manager-Instanz sind ebenfalls erforderlich.

### <a name="storage-considerations"></a>Speicheraspekte

Wie Oracle RAC ist auch Db2 pureScale eine Hochleistungsdatenbank mit Block-E/A und horizontaler Skalierung. Es wird empfohlen, den größten verfügbaren [Azure Storage Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)-Speicher zu verwenden, der Ihren Anforderungen entspricht. Kleinere Speicheroptionen können z.B. für Entwicklungs- und Testumgebungen geeignet sein, während bei Produktionsumgebungen häufig größere Speicherkapazität erforderlich ist. Die Beispielarchitektur verwendet [P30](https://azure.microsoft.com/pricing/details/managed-disks/) aufgrund des Verhältnisses von IOPS zu Größe und Preis. Verwenden Sie unabhängig von der Größe Storage Premium für eine optimale Leistung.

Db2 pureScale verwendet eine Architektur mit umfassender Freigabe, bei der von allen Clusterknoten auf alle Daten zugegriffen werden kann. Premium-Speicher muss für mehrere Instanzen freigegeben werden – ganz gleich, ob bedarfsgesteuerte oder dedizierte Instanzen.

Ein großer Db2 pureScale-Cluster kann 200 Terabyte (TB) oder mehr freigegebenen Premium-Speicher mit einem IOPS-Wert von 100.000 erfordern. Db2 pureScale unterstützt eine iSCSI-Blockschnittstelle, die in Azure verwendet werden kann. Die iSCSI-Schnittstelle erfordert einen Cluster mit freigegebenem Speicher, der mit GlusterFS, S2D oder einem anderen Tool implementiert werden kann. Bei diesem Lösungstyp wird ein Gerät für das virtuelle Storage Area Network (vSAN) in Azure erstellt. Db2 pureScale verwendet das vSAN zum Installieren des gruppierten Dateisystems, mit dem Daten zwischen mehreren virtuellen Computern freigegeben werden.

Bei dieser Architektur wurde das GlusterFS-Dateisystem verwendet, ein kostenloses, skalierbares, verteiltes Open-Source-Dateisystem, das speziell für Cloudspeicher optimiert ist.

### <a name="networking-considerations"></a>Netzwerkaspekte

IBM empfiehlt InfiniBand-Netzwerkfunktionen für alle Member in einem Db2 pureScale-Cluster. Db2 pureScale verwendet außerdem Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA), sofern verfügbar, für die verwendeten CFs.

Während des Setups wird eine Azure-[Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) erstellt, in der alle virtuellen Computer enthalten sind. Ressourcen werden allgemein auf der Grundlage ihrer Lebensdauer sowie auf der Grundlage der Benutzer gruppiert, die sie verwalten. Die virtuellen Computer in dieser Architektur erfordern [beschleunigten Netzwerkbetrieb](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/), ein Azure-Feature, das konsistente, extrem geringe Netzwerklatenz über E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) für einen virtuellen Computer bietet.

Jeder virtuelle Azure-Computer wird in einem virtuellen Netzwerk bereitgestellt, das in mehrere Subnetze segmentiert ist: Hauptsubnetz, GlusterFS-Front-End (gfsfe), GlusterFS-Back-End (bfsbe), Db2 pureScale (db2be) und Db2 pureScale-Front-End (db2fe). Das Installationsskript erstellt auch die primären [NICs](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) auf den virtuellen Computern im Hauptsubnetz.

Mithilfe von [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSGs) wird der Netzwerkdatenverkehr innerhalb des virtuellen Netzwerks eingeschränkt und Subnetze isoliert.

In Azure muss Db2 pureScale TCP/IP als Netzwerkverbindung zum Speicher verwenden.

## <a name="next-steps"></a>Nächste Schritte

-   [Bereitstellen dieser Architektur in Azure](deploy-ibm-db2-purescale-azure.md)
