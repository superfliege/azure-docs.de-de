---
title: Bekannte Probleme bei Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel werden bekannte Probleme erläutert, die bei Azure Monitor für VMs auftreten. Azure Monitor ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Betriebssystems von Azure-VMs vereint. Azure Monitor für VMs ermittelt automatisch Anwendungskomponenten und Abhängigkeiten von anderen Ressourcen und steuert die Kommunikation zwischen diesen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/26/2018
ms.author: magoedte
ms.openlocfilehash: c329e1fa80c6647bb78b11917ecd012461e62ea4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790502"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekannte Probleme bei Azure Monitor für VMs (Vorschauversion)

In diesem Artikel werden bekannte Probleme erläutert, die bei Azure Monitor für VMs auftreten. Azure Monitor ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Betriebssystems von Azure-VMs vereint. 

Die folgende Aufstellung enthält bekannte Probleme bei der Integritätsfunktion:

- Die Integritätsfunktion ist für alle VMs aktiviert, die mit dem Log Analytics-Arbeitsbereich verbunden sind. Dies ist auch so, wenn die Aktion in einer einzelnen VM gestartet und beendet wird.
- Für virtuelle Linux-Computer enthält der Titel der Seite, auf der die Integritätskriterien für eine einzelne VM-Ansicht aufgelistet wird, den gesamten Domänenname des virtuellen Computers anstelle des benutzerdefinierten VM-Namens.  
- Wenn Sie die Überwachung für eine VM mithilfe der unterstützten Methoden deaktiviert haben, und Sie versuchen, diese erneut bereitzustellen, sollte dies im gleichen Arbeitsbereich erfolgen. Wenn Sie einen neuen Arbeitsbereich verwenden und versuchen, den Integritätszustand für diese VM anzuzeigen, werden Sie möglicherweise ein anormales Verhalten sehen.
- Wenn eine Azure-VM entfernt oder gelöscht wird, wird dies für einige Zeit in der VM-Listenansicht angezeigt. Wenn Sie außerdem auf den Zustand einer entfernten oder gelöschten VM klicken, wird die Ansicht **Integritätsdiagnosen** geöffnet und anschließend eine Ladeschleife initiiert. Durch Auswählen des Namens der gelöschten VM wird ein Bereich geöffnet mit der Meldung, dass die VM gelöscht wurde.
- Zeitraum und Häufigkeit der Integritätskriterien können in dieser Version nicht geändert werden. 
- Integritätskriterien können nicht deaktiviert werden. 
- Nach der Bereitstellung kann es etwas dauern, bis Daten im Bereich **Azure Monitor** > **Virtual Machines** > **Integrität** oder im Bereich **VM-Ressource**  > **Insights** angezeigt werden.
- Die Benutzeroberfläche der Integritätsdiagnose wird schneller aktualisiert als alle anderen Ansichten. Daher kann es beim Wechseln zwischen Ansichten zu Informationsverzögerungen kommen. 
- Die Warnungszusammenfassung in Azure Monitor für VMs bezieht sich nur auf Warnungen, die für erkannte Integritätsprobleme auf überwachten Azure-VMs ausgelöst wurden.
- Beim Herunterfahren von VMs werden einige Integritätskriterien auf *kritisch* und andere auf *fehlerfrei* aktualisiert. Der Netzwerkstatus der VM wird als *kritisch* angezeigt.
- Der Schweregrad von Integritätswarnungen kann nicht geändert werden, sie können lediglich aktiviert oder deaktiviert werden. Außerdem werden einige Schweregrade auf der Grundlage des Status von Integritätskriterien aktualisiert.   
- Das Ändern einer beliebigen Einstellung einer Instanz eines Integritätskriteriums führt zur Änderung der gleichen Einstellung in allen Instanzen von Integritätskriterien des gleichen Typs in der VM. Wenn beispielsweise der Schwellenwert der Instanz des Integritätskriteriums „verfügbarer Speicherplatz“, der dem logischen Laufwerk C: entspricht, geändert wird, gilt dieser Schwellenwert auch für alle anderen logischen Datenträger, die für die gleiche VM ermittelt wurden und überwacht werden.  
- Schwellenwerte für Integritätskriterien, die auf eine Windows-VM abzielen, sind nicht veränderbar, weil ihr Integritätszustand auf *laufend* oder *verfügbar* festgelegt ist. Wenn Sie den Integritätszustand über die [Workloadüberwachungs-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) abfragen, wird dieser den *Vergleichsoperator*-Wert von **LessThan** oder **GreaterThan** mit einem *Schwellenwert*-Wert von **4** für den Dienst oder die Entität anzeigen, wenn:
   - Integrität des DNS-Clientdiensts: Dienst wird nicht ausgeführt. 
   - Integrität des DHCP-Clientdiensts: Dienst wird nicht ausgeführt. 
   - Integrität des RPC-Diensts: Dienst wird nicht ausgeführt. 
   - Integrität des Windows-Firewalldiensts: Dienst wird nicht ausgeführt.
   - Integrität des Windows-Ereignisprotokolldiensts: Dienst wird nicht ausgeführt. 
   - Integrität des Serverdiensts: Dienst wird nicht ausgeführt. 
   - Integrität des Windows-Remoteverwaltungsdiensts: Dienst wird nicht ausgeführt. 
   - Fehler oder Beschädigung des Dateisystems: Logischer Datenträger ist nicht verfügbar.

- Die Schwellenwerte für die folgenden Linux-Integritätskriterien sind nicht veränderbar, weil ihr Integritätszustand bereits auf *wahr* festgelegt ist. Der Integritätszustand zeigt den *Vergleichsoperator* mit einem Wert **LessThan** und *Schwellenwert* von **1**, wenn er von der Workloadüberwachungs-API für die Entität abhängig von ihrem Kontext abgefragt wird:
   - Status des logischen Datenträgers – Logischer Datenträger ist nicht online/verfügbar.
   - Datenträgerstatus – Datenträger ist nicht online/verfügbar.
   - Netzwerkadapterstatus – Netzwerkadapter ist deaktiviert  

- Konfigurationsänderungen, wie z. B. das Aktualisieren eines Schwellenwerts, dauern bis zu 30 Minuten, selbst wenn sie möglicherweise vom Portal oder der Workloadüberwachungs-API sofort aktualisiert werden. 
- Integritätskriterien für einzelne Prozessoren und logische Prozessorstufen sind in Windows nicht verfügbar. Nur „Gesamte CPU-Auslastung“ ist für Windows-VMs verfügbar. 
- Die für jedes Integritätskriterium definierten Warnregeln werden im Azure-Portal nicht angezeigt. Sie können eine Integritätswarnregel nur über die [Workloadüberwachungs-API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components) aktivieren oder zu deaktivieren. 
- Die Zuweisung einer [Azure Monitor-Aktionsgruppe](../../azure-monitor/platform/action-groups.md) für Integritätswarnungen ist vom Azure-Portal aus nicht möglich. Sie können nur die API für die Benachrichtigungseinstellung verwenden, um eine Aktionsgruppe zu konfigurieren, die bei jeder Auslösung einer Integritätswarnung ebenfalls ausgelöst wird. Derzeit können Aktionsgruppen einer VM zugewiesen werden, sodass alle *Integritätswarnungen*, die für die VM ausgelöst wurden, die gleichen Aktionsgruppen auslösen. Anders als bei herkömmlichen Azure-Warnungen gibt es kein Konzept einer separaten Aktionsgruppe für jede Integritätswarnregel. Darüber hinaus werden nur Aktionsgruppen unterstützt, die so konfiguriert sind, dass Sie durch Senden einer E-Mail oder SMS benachrichtigt werden, wenn Integritätswarnungen ausgelöst werden. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung der Überwachung Ihrer virtuellen Computer finden Sie unter [Deploy Azure Monitor for VMs (Bereitstellen von Azure Monitor für VMs)](vminsights-onboard.md).
