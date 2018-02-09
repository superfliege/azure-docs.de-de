---
title: "Hinzufügen erweiterter Metriken für virtuelle Azure-Computer | Microsoft-Dokumentation"
description: "Dieser Artikel unterstützt Sie dabei, erweiterte Diagnosemetriken für Ihre Azure-VMs zu aktivieren und zu konfigurieren."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 91797aaab1dca96e78643f57776eb16d336e894b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Hinzufügen erweiterter Metriken für virtuelle Azure-Computer

Der Cost Management-Dienst verwendet Azure-Metrikdaten aus Ihren Azure-VMs, um ausführliche Informationen zu den zugehörigen Ressourcen anzuzeigen. Metrikdaten – auch Leistungsindikatoren genannt – werden in Cost Management zum Generieren von Berichten verwendet. In Cost Management werden jedoch nicht automatisch alle Azure-Metrikdaten von Gast-VMs erfasst – Sie müssen die Metrikerfassung aktivieren. Dieser Artikel unterstützt Sie dabei, zusätzliche Diagnosemetriken für Ihre Azure-VMs zu aktivieren und zu konfigurieren.

Das Aktivieren der Metrikerfassung ermöglicht Folgendes:

- Benachrichtigung über das Erreichen von Arbeitsspeicher-, Datenträger- und CPU-Limits für Ihre VMs
- Erkennen von Nutzungstrends und Anomalien
- Steuern Ihrer Kosten durch eine Dimensionierung gemäß Nutzung
- Erhalt von Optimierungsempfehlungen für eine kosteneffiziente Größenanpassung von Cost Management

Angenommen, Sie möchten die CPU-Auslastung in % und die Arbeitsspeicherauslastung in % für Ihre Azure-VMs überwachen. Die entsprechenden Azure-VM-Metriken lauten _[Host] CPU in Prozent_ und _[Gast] Arbeitsspeicherprozentsatz_.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Überprüfen, ob Metriken auf VMs aktiviert sind

1. Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.
2. Wählen Sie unter **Virtuelle Computer** eine VM aus, und klicken Sie unter **Überwachung** auf **Metriken**. Eine Liste der verfügbaren Metriken wird angezeigt.
3. Wählen Sie einige Metriken aus, und die Daten für diese Metriken werden in einem Diagramm angezeigt.  
    ![Beispielmetrik: CPU-Auslastung in Prozent für den Host](./media/azure-vm-extended-metrics/metric01.png)

Im vorangehenden Beispiel steht ein eingeschränkter Satz mit Standardmetriken für Ihre Hosts bereit, es sind jedoch keine Metriken zum Arbeitsspeicher verfügbar. Arbeitsspeichermetriken gehören zu den erweiterten Metriken. Sie müssen einige zusätzliche Schritte ausführen, um die erweiterten Metriken zu aktivieren. Nachfolgend wird die Vorgehensweise zum Aktivieren dieser erweiterten Metriken beschrieben.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Aktivieren erweiterter Metriken im Azure-Portal

Die Standardmetriken sind Metriken zum Hostcomputer. Ein Beispiel ist die Metrik _[Host] CPU in Prozent_. Darüber hinaus stehen grundlegende Metriken für Gast-VMs zur Verfügung, diese werden auch als erweiterte Metriken bezeichnet. Beispiele für erweiterte Metriken sind _[Gast] Arbeitsspeicherprozentsatz_ und _[Gast] Verfügbarer Arbeitsspeicher_.

Das Aktivieren der erweiterten Metriken ist einfach. Aktivieren Sie für jede VM die Überwachung auf Gastebene. Wenn Sie die Überwachung auf Gastebene aktivieren, wird der Azure-Diagnose-Agent auf der VM installiert. Das folgende Verfahren ist für klassische und reguläre VMs sowie für Windows- und Linux-VMs identisch.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Aktivieren der Überwachung auf Gastebene für vorhandene VMs

1. Zeigen Sie in **Virtuelle Computer** die Liste Ihrer VMs an, und wählen Sie eine VM aus.
2. Wählen Sie unter **Überwachung** die Option **Metriken**.
3. Klicken Sie auf **Diagnoseeinstellungen**.
4. Klicken Sie auf der Seite „Diagnoseeinstellungen“ auf **Überwachung auf Gastebene aktivieren**. Für Linux-VMs ist ein vorhandenes Speicherkonto erforderlich. Wenn Sie kein Speicherkonto für eine Windows-VM auswählen, wird ein Speicherkonto für Sie erstellt.  
    ![Überwachung auf Gastebene aktivieren](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Nach einigen Minuten wird der Azure-Diagnose-Agent auf der VM installiert. Aktualisieren Sie die Seite. Die Liste der verfügbaren Metriken wird mit Gastmetriken aktualisiert.  
    ![Erweiterte Metriken](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Aktivieren der Überwachung auf Gastebene für neue VMs

Stellen Sie beim Erstellen neuer VMs sicher, dass Sie **Diagnose des Gastbetriebssystems** aktivieren. Für Linux-VMs ist ein vorhandenes Speicherkonto erforderlich. Wenn Sie kein Speicherkonto für eine Windows-VM auswählen, wird ein Speicherkonto für Sie erstellt.

![Aktivieren der Diagnose des Gastbetriebssystems](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Resource Manager-Anmeldeinformationen

Nach dem Aktivieren der erweiterten Metriken müssen Sie sicherstellen, dass der Cost Management-Dienst Zugriff auf Ihre [Resource Manager-Anmeldeinformationen](activate-subs-accounts.md) hat. Ihre Anmeldeinformationen werden für Cost Management benötigt, um Leistungsdaten für Ihre VMs zu erfassen und anzuzeigen. Außerdem werden die Daten verwendet, um Empfehlungen zur Kostenoptimierung zu erstellen. Cost Management muss mindestens drei Tage lang Leistungsdaten von einer Instanz erhalten, um entscheiden zu können, ob es sich um einen Kandidaten für eine Verkleinerungsempfehlung handelt.

## <a name="enable-vm-metrics-with-a-script"></a>Aktivieren von VM-Metriken mit einem Skript

Sie können VM-Metriken mit Azure PowerShell-Skripts aktivieren. Wenn Sie viele VMs besitzen, für die Sie Metriken aktivieren möchten, können Sie den Vorgang mit einem Skript automatisieren. Beispielskripts finden Sie in GitHub im Abschnitt zur [Aktivierung der Azure-Diagnose](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Anzeigen von Azure-Leistungsmetriken

Um Leistungsmetriken für Ihre Azure-Instanzen im Cloudyn-Portal anzuzeigen, navigieren Sie zu **Assets** > **Compute** > **Instanz-Explorer**. Erweitern Sie in der Liste der VM-Instanzen eine Instanz, und erweiterten Sie dann eine Ressource, um Details anzuzeigen.

![Instanz-Explorer](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie den Azure Resource Manager-API-Zugriff für Ihre Konten noch nicht aktiviert haben, fahren Sie mit [Aktivieren von Azure-Abonnements und -Konten](activate-subs-accounts.md) fort.
