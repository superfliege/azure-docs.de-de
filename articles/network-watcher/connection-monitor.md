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
ms.openlocfilehash: c7d98350dc8f66ebd4097f22b44dcbbe2653b25d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Überwachen von Netzwerkverbindungen mit Azure Network Watcher mithilfe des Azure-Portals

Erfahren Sie, wie Sie den Verbindungsmonitor verwenden, um die Netzwerkkonnektivität zwischen einem virtuellen Azure-Computer und einer IP-Adresse zu überwachen. Der Verbindungsmonitor bietet Überwachung auf Verbindungsebene. Eine Verbindung ist definiert als eine Kombination aus Quell- und Ziel-IP-Adresse und -Port. Ein Verbindungsmonitor ermöglicht Szenarien wie die Überwachung der Konnektivität von einem virtuellen Computer in einem virtuellen Netzwerk zu einem virtuellen Computer mit SQL Server im gleichen oder einem anderen virtuellen Netzwerk über Port 1433. Ein Verbindungsmonitor bietet Ihnen eine Verbindungslatenz als Azure Monitor-Metrik, die alle 60 Sekunden aufgezeichnet wird. Zudem bietet er Ihnen eine Hop-by-Hop-Topologie, und es werden Konfigurationsprobleme identifiziert, die sich auf Ihre Verbindung auswirken.


## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Schritte in diesem Artikel ausführen können:

* Eine Instanz von Network Watcher in der Region, in der Sie eine Verbindung überwachen möchten. Falls Sie noch nicht über eine Instanz verfügen, können Sie eine erstellen, indem Sie die Schritte unter [Erstellen einer Azure Network Watcher-Instanz](network-watcher-create.md) ausführen.
* Ein virtueller Computer, von dem aus Sie die Überwachung durchführen.
* Sie müssen auf dem virtuellen Computer, von dem aus Sie eine Verbindung überwachen möchten, `AzureNetworkWatcherExtension` installieren. Informationen zum Installieren der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Azure Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="create-a-connection-monitor"></a>Erstellen eines Verbindungsmonitors

Die folgenden Schritte ermöglichen die Verbindungsüberwachung zu einem virtuellen Zielcomputer über die Ports 80 und 1433:

1. Wählen Sie im Portal auf der linken Seite **Weitere Dienste** aus.
2. Beginnen Sie mit der Eingabe von *Network Watcher*. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
3. Wählen Sie unter **Überwachung** die Option **Verbindungsmonitor (Vorschau)** aus. Features, die sich in der Vorschauversion befinden, weisen nicht die gleiche Zuverlässigkeit oder regionale Verfügbarkeit auf wie bereits allgemein verfügbare Features.
4. Wählen Sie **+ Hinzufügen**.
5. Geben Sie die Informationen zu der Verbindung, die Sie überwachen möchten, ein, oder wählen Sie sie aus, und wählen Sie dann **Hinzufügen** aus. Im Bild zum folgenden Beispiel wird die Verbindung zwischen den virtuellen Computern *MultiTierApp0* und *Database0* überwacht:

    ![Hinzufügen eines Verbindungsmonitors](./media/connection-monitor/add-connection-monitor.png)

    Die Überwachung beginnt. Der Verbindungsmonitor führt alle 60 Sekunden einen Test aus.

## <a name="view-connection-monitoring"></a>Anzeigen der Verbindungsüberwachung

1. Führen Sie die in [Erstellen eines Verbindungsmonitors](#create-a-connection-monitor) beschriebenen Schritte 1 bis 3 aus, um die Verbindungsüberwachung anzuzeigen.
2. Das folgende Bild zeigt die Details für die AppToDB(80)-Verbindung. Der **Status** ist erreichbar. Die **Diagrammansicht** zeigt **Durchschnittliche Roundtrip-Zeit** und **Fehlerhafte Tests in Prozent**. Das Diagramm liefert Hop-by-Hop-Informationen und zeigt, dass es keine Probleme mit Auswirkungen auf die Erreichbarkeit des Ziels gibt.

    ![Anzeigen des Verbindungsmonitors](./media/connection-monitor/view-connection-monitor.png)

3. Beim Betrachten des *AppToDB(1433)*-Monitors, wie in der folgenden Abbildung gezeigt, sehen Sie, dass der Status für die gleichen virtuellen Quell- und Zielcomputer über Port 1433 nicht erreichbar ist. Die **Rasteransicht** in diesem Szenario liefert die Hop-by-Hop-Informationen und sich auf die Erreichbarkeit auswirkende Probleme. In diesem Fall blockiert eine NSG-Regel den gesamten Datenverkehr für Port 1433 beim zweiten Hop.

    ![Anzeigen des Verbindungsmonitors](./media/connection-monitor/view-connection-monitor-2.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).
- Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um zu ermitteln, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.