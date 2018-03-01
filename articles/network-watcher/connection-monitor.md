---
title: "Überwachen von Netzwerkverbindungen mit Azure Network Watcher – Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Netzwerkkonnektivität mit Azure Network Watcher mithilfe des Azure-Portals überprüfen."
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Überwachen von Netzwerkverbindungen mit Azure Network Watcher mithilfe des Azure-Portals

Erfahren Sie, wie Sie den Verbindungsmonitor verwenden, um die Netzwerkkonnektivität zwischen einem virtuellen Azure-Computer und einer IP-Adresse zu überwachen. Die IP-Adresse kann einer anderen Azure-Ressource oder einer Internet- oder lokalen Ressource zugewiesen sein.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Schritte in diesem Artikel ausführen können:

* Eine Instanz von Network Watcher in der Region, in der Sie eine Verbindung überwachen möchten. Falls Sie noch nicht über eine Instanz verfügen, können Sie eine erstellen, indem Sie die Schritte unter [Erstellen einer Azure Network Watcher-Instanz](network-watcher-create.md) ausführen.
* Ein virtueller Computer, von dem aus Sie die Überwachung durchführen.
* Sie müssen auf dem virtuellen Computer, von dem aus Sie eine Verbindung überwachen möchten, `AzureNetworkWatcherExtension` installieren. Informationen zum Installieren der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="create-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors

1. Wählen Sie im Portal auf der linken Seite **Weitere Dienste** aus.
2. Beginnen Sie mit der Eingabe von *Network Watcher*. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
3. Wählen Sie unter **Überwachung** die Option **Verbindungsmonitor (Vorschau)** aus. Features, die sich in der Vorschauversion befinden, weisen nicht die gleiche Zuverlässigkeit oder regionale Verfügbarkeit auf wie bereits allgemein verfügbare Features.
4. Wählen Sie **+ Hinzufügen**.
5. Geben Sie die entsprechenden Informationen zu der Verbindung, die Sie überwachen möchten, ein, oder wählen Sie sie aus, und wählen Sie dann **Hinzufügen** aus. In diesem Beispiel wird eine Verbindung zwischen den virtuellen Computern *myVmSource* und *myVmDestination* über Port 80 überwacht.
    
    |  Einstellung                                 |  Wert               |
    |  -------------------------------------   |  ------------------- |
    |  NAME                                    |  myConnectionMonitor |
    |  Virtueller Ausgangscomputer                  |  myVmSource          |
    |  Quellport                             |                      |
    |  Ziel – wählen Sie einen virtuellen Computer aus.   |  myVmDestination     |
    |  Zielport                        |  80                  |

6. Die Überwachung beginnt. Der Verbindungsmonitor führt alle 60 Sekunden einen Test aus.
7. Der Verbindungsmonitor zeigt die durchschnittliche Roundtripzeit und den Prozentsatz der nicht erfolgreichen Tests an. Sie können die Überwachungsdaten in einem Raster oder in einem Diagramm anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).

- Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um zu ermitteln, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.