---
title: Überwachen von Netzwerkverbindungen mit Azure Network Watcher – Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Netzwerkkonnektivität mit Azure Network Watcher mithilfe des Azure-Portals überprüfen.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: 0d550d3bda119cfcb9ecc6f852006d5e325fdfa3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Überwachen von Netzwerkverbindungen mit Azure Network Watcher mithilfe des Azure-Portals

Erfahren Sie, wie Sie den Verbindungsmonitor verwenden, um die Netzwerkkonnektivität zwischen einem virtuellen Azure-Computer (VM) und einer IP-Adresse zu überwachen. Der Verbindungsmonitor leistet die Überwachung zwischen Quell- und Ziel-IP-Adresse und Port. Ein Verbindungsmonitor ermöglicht Szenarien wie die Überwachung der Konnektivität zwischen einem virtuellen Computer in einem virtuellen Netzwerk mit einem virtuellen Computer mit SQL Server im gleichen oder einem anderen virtuellen Netzwerk über Port 1433. Ein Verbindungsmonitor bietet Ihnen eine Verbindungslatenz als Azure Monitor-Metrik, die alle 60 Sekunden aufgezeichnet wird. Zudem bietet er Ihnen eine Hop-by-Hop-Topologie, und es werden Konfigurationsprobleme identifiziert, die sich auf Ihre Verbindung auswirken.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Schritte in diesem Artikel ausführen können:

* Eine Instanz von Network Watcher in der Region, in der Sie eine Verbindung überwachen möchten. Falls Sie noch nicht über eine Instanz verfügen, können Sie eine erstellen, indem Sie die Schritte unter [Erstellen einer Azure Network Watcher-Instanz](network-watcher-create.md) ausführen.
* Einen virtuellen Computer, von dem aus die Überwachung erfolgt. Um zu erfahren, wie Sie einen virtuellen Computer erstellen, lesen Sie die Artikel zum Erstellen von [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)-VMs.
* Sie müssen auf dem virtuellen Computer, von dem aus Sie eine Verbindung überwachen möchten, `AzureNetworkWatcherExtension` installieren. Informationen zum Installieren der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="create-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors

Die folgenden Schritte ermöglichen die Verbindungsüberwachung zu einem virtuellen Zielcomputer über die Ports 80 und 1433:

1. Wählen Sie im Portal auf der linken Seite **Alle Dienste** aus.
2. Beginnen Sie mit der Eingabe von *Network Watcher* im Feld **Filter**. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
3. Wählen Sie unter **ÜBERWACHUNG** die Option **Verbindungsmonitor** aus.
4. Wählen Sie **+ Hinzufügen**.
5. Geben Sie die Informationen zu der Verbindung, die Sie überwachen möchten, ein, oder wählen Sie sie aus, und wählen Sie dann **Hinzufügen** aus. Im Bild zum folgenden Beispiel wird die Verbindung zwischen den virtuellen Computern *MultiTierApp0* und *Database0* über Port 80 überwacht:

    ![Hinzufügen eines Verbindungsmonitors](./media/connection-monitor/add-connection-monitor.png)

    Die Überwachung beginnt. Der Verbindungsmonitor führt alle 60 Sekunden einen Test aus.
6. Führen Sie Schritt 5 erneut aus, und geben Sie die gleiche Quell- und Ziel-VM sowie die folgenden Werte an:
    
    |Einstellung  |Wert          |
    |---------|---------      |
    |NAME     | AppToDB(1433) |
    |Port     | 1433          |

## <a name="view-connection-monitoring"></a>Anzeigen der Verbindungsüberwachung

1. Führen Sie die in [Erstellen eines Verbindungsmonitors](#create-a-connection-monitor) beschriebenen Schritte 1 bis 3 aus, um die Verbindungsüberwachung anzuzeigen.
2. Das folgende Bild zeigt die Details für die *AppToDB(80)*-Verbindung. Der **Status** ist erreichbar. Die **Diagrammansicht** zeigt **Durchschnittliche Roundtrip-Zeit** und **Fehlerhafte Tests in Prozent**. Das Diagramm liefert Hop-by-Hop-Informationen und zeigt, dass es keine Probleme mit Auswirkungen auf die Erreichbarkeit des Ziels gibt.

    ![Diagrammansicht](./media/connection-monitor/view-graph.png)

3. Beim Betrachten der in der folgenden Abbildung gezeigten *AppToDB(1433)*-Verbindung sehen Sie, dass der Status für die gleichen virtuellen Quell- und Zielcomputer über Port 1433 nicht erreichbar ist. Die **Rasteransicht** in diesem Szenario liefert die Hop-by-Hop-Informationen und sich auf die Erreichbarkeit auswirkende Probleme. In diesem Fall blockiert eine NSG-Regel den gesamten Datenverkehr für Port 1433 beim zweiten Hop.

    ![Rasteransicht](./media/connection-monitor/view-grid.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).
- Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um zu ermitteln, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.