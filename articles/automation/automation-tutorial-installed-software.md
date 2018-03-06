---
title: Ermitteln der auf Ihren Computern installierten Software mit Azure Automation | Microsoft-Dokumentation
description: Ermitteln Sie mit der Bestandsfunktion, welche Software auf den Computern in Ihrer Umgebung installiert ist.
services: automation
keywords: "Inventar, Inventur, Bestand, Automatisierung, Änderung, Nachverfolgung"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 02/28/2018
ms.topic: tutorial
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 97cd2c91ca2c70b044518c43d49356918202d5ff
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Ermitteln der Software, die auf Ihren Azure-Computern und anderen Computern installiert ist

In diesem Tutorial wird beschrieben, wie Sie ermitteln können, welche Software in Ihrer Umgebung installiert ist. Sie können die Bestandsinformationen für Software, Dateien, Linux-Daemons, Windows-Dienste und Windows-Registrierungsschlüssel auf Ihren Computern sammeln und anzeigen. Durch das Nachverfolgen der Konfigurationen Ihrer Computer können Sie betriebsbezogene Problem in Ihrer Umgebung erkennen und ein besseres Verständnis des Zustands Ihrer Computer entwickeln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren einer VM für die Änderungsnachverfolgung und den Bestand
> * Anzeigen von installierter Software
> * Durchsuchen von Bestandsprotokollen nach installierter Software

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Änderungsnachverfolgung und Bestandsfunktion

In diesem Tutorial müssen Sie zuerst die Änderungsnachverfolgung und die Bestandsfunktion für Ihre VM aktivieren. Wenn Sie die Lösung **Änderungsnachverfolgung** für Ihre VM zuvor nicht aktiviert haben, ist dieser Schritt nicht erforderlich.

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer**, und wählen Sie in der Liste eine VM aus.
2. Klicken Sie im Menü auf der linken Seite unter dem Abschnitt **Vorgänge** auf **Bestand**. Die Seite **Änderungsnachverfolgung und Bestand aktivieren** wird geöffnet.

![Banner für die Konfiguration zur Integration der Bestandsfunktion](./media/automation-tutorial-installed-software/enableinventory.png)

Konfigurieren Sie den gewünschten Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto, und klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Wenn die Felder ausgegraut sind, bedeutet dies, dass eine andere Automatisierungslösung für die VM aktiviert ist und derselbe Arbeitsbereich und dasselbe Automation-Konto verwendet werden müssen.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Bestandsfunktion generiert werden.
Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.

Das Aktivieren der Lösung kann bis zu 15 Minuten dauern. Während dieses Zeitraums sollten Sie das Browserfenster nicht schließen.
Nachdem die Lösung aktiviert wurde, werden Informationen zur installierten Software und Änderungen der VM-Datenflüsse zu Log Analytics angezeigt.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="view-installed-software"></a>Anzeigen von installierter Software

Nachdem die Lösung für die Änderungsnachverfolgung und den Bestand aktiviert wurde, können Sie die Ergebnisse auf der Seite **Bestand** anzeigen.

Wählen Sie auf Ihrer VM unter **VORGÄNGE** die Option **Bestand**.

Klicken Sie auf der Seite **Bestand** auf die Registerkarte **Software**.

Auf der Registerkarte **Software** wird eine Tabellenliste mit der gefundenen Software angezeigt. Die Software ist nach Name und Version der Software gruppiert.

Die allgemeinen Details zu den einzelnen Softwaredatensätzen können in der Tabelle angezeigt werden. Diese Details enthalten den Softwarenamen, die Version, den Herausgeber, den letzten Aktualisierungszeitpunkt (letzter Aktualisierungszeitpunkt, der von einem Computer der Gruppe gemeldet wurde) und die Computer (Anzahl von Computern mit der jeweiligen Software).

![Softwarebestand](./media/automation-tutorial-installed-software/inventory-software.png)

Klicken Sie auf eine Zeile, um die Eigenschaften des Softwaredatensatzes und die Namen der Computer mit dieser Software anzuzeigen.

Sie können im Textfeld direkt oberhalb der Softwareliste eine Suche durchführen, um nach einer bestimmten Software oder Softwaregruppe zu suchen.
Der Filter ermöglicht Ihnen die Suche anhand des Softwarenamens, der Version und des Herausgebers.

Wenn Sie beispielsweise nach „Contoso“ suchen, wird die gesamte Software zurückgegeben, deren Name, Herausgeber oder Version den Text „Contoso“ enthält.

## <a name="search-inventory-logs-for-installed-software"></a>Durchsuchen von Bestandsprotokollen nach installierter Software

Bei der Bestandsfunktion werden Protokolldaten generiert, die an Log Analytics gesendet werden. Um die Protokolle per Ausführung von Abfragen zu durchsuchen, klicken Sie oben im Fenster **Bestand** auf **Log Analytics**.

Die Bestandsdaten werden unter dem Typ **ConfigurationData** gespeichert.
Mit der folgenden Log Analytics-Beispielabfrage werden die Herausgeber zurückgegeben, die „Microsoft“ enthalten, sowie die Anzahl von Softwaredatensätzen (gruppiert nach „SoftwareName“ und „Computer“) für jeden Herausgeber.

```
ConfigurationData
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
| where ConfigDataType == "Software"
| search Publisher:"Microsoft"
| summarize count() by Publisher
```

Weitere Informationen zur Ausführung von Abfragen und zum Durchsuchen von Protokolldateien in Log Analytics finden Sie unter [Azure Log Analytics](https://docs.loganalytics.io/index).

### <a name="single-machine-inventory"></a>Bestandsermittlung für einen einzelnen Computer

Zum Anzeigen des Softwarebestands für einen einzelnen Computer können Sie über die Seite mit den Azure-VM-Ressourcen auf die Bestandsfunktion zugreifen oder Log Analytics zum Filtern nach dem entsprechenden Computer verwenden. Mit der folgenden Log Analytics-Beispielabfrage wird eine Liste mit Software für einen Computer mit dem Namen ContosoVM zurückgegeben.

```
ConfigurationData
| where ConfigDataType == "Software" 
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie den Softwarebestand beispielsweise für folgende Zwecke anzeigen:

> [!div class="checklist"]
> * Integrieren einer VM für die Änderungsnachverfolgung und den Bestand
> * Anzeigen von installierter Software
> * Durchsuchen von Bestandsprotokollen nach installierter Software

Fahren Sie mit der Übersicht über die Lösung für die Änderungsnachverfolgung und den Bestand fort, um mehr darüber zu erfahren.

> [!div class="nextstepaction"]
> [Lösung für die Änderungsnachverfolgung und den Bestand](automation-change-tracking.md)