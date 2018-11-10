---
title: 'Azure Monitor für VMs (Vorschauversion): bekannte Probleme | Microsoft-Dokumentation'
description: Azure Monitor for VMs ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. In diesem Artikel werden bekannte Probleme behandelt.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: eba9e26f12fd9e1862727adec4f8c8f594e8f659
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091673"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekannte Probleme bei Azure Monitor für VMs (Vorschauversion)

Die folgende Aufstellung enthält bekannte Probleme beim Integritätsfeature von Azure Monitor for VMs:

- Die Integritätsfunktion wird in alle VMs integriert, die mit dem Log Analytics-Arbeitsbereich verbunden sind, auch wenn das Onboarding von einer einzelnen VM aus gestartet und abgeschlossen wird.
- Wenn eine Azure-VM nicht mehr existiert, weil sie entfernt oder gelöscht wurde, erscheint sie drei bis sieben Tage lang in der VM-Listenansicht. Wenn Sie auf den Zustand einer entfernten oder gelöschten VM klicken, wird darüber hinaus die Ansicht **Integritätsdiagnosen** geöffnet, die dann in eine Ladeschleife übergeht. Die Auswahl des Namens einer gelöschten VM startet ein Blatt mit der Meldung, dass die VM gelöscht wurde.
- Zeitraum und Häufigkeit der Integritätskriterien können in dieser Version nicht geändert werden. 
- Integritätskriterien können nicht deaktiviert werden. 
- Nach dem Onboarding kann eine gewisse Zeit vergehen, ehe Daten in „Azure Monitor“ > „Virtual Machines“ > „Integrität“ oder auf dem VM-Ressourcenblatt > „Insights“ angezeigt werden.
- Die Benutzeroberfläche der Integritätsdiagnose wird schneller als alle anderen Ansichten aktualisiert, daher kann es beim Wechseln zwischen Ansichten zu Informationsverzögerungen kommen.  
- Die Warnungszusammenfassung in Azure Monitor for VM-Integrität bezieht sich nur auf Warnungen, die für erkannte Integritätsprobleme auf überwachten Azure-VMs ausgelöst wurden.
- Die Ansicht der Seite **Warnungsliste** in der einzelnen VM und in Azure Monitor zeigt Warnungen, deren Überwachungsbedingung auf „ausgelöst“ innerhalb der letzten 30 Tage festgelegt ist.  Sie sind nicht konfigurierbar. Nach dem Klicken auf die Zusammenfassung können Sie jedoch nach dem Starten der Seitenansicht **Warnungsliste** die Filterwerte sowohl der Überwachungsbedingung als auch des Zeitraums ändern.
- Wir empfehlen Ihnen, auf der Seitenansicht **Warnungsliste** die Filter **Ressourcentyp**, **Ressource** und **Überwachungsdienst** nicht zu ändern, da sie spezifisch für die Lösung konfiguriert wurden (diese Listenansicht zeigt gegenüber der Warnungslistenansicht in Azure Monitor einige zusätzliche Felder an).    
- In Linux-VMs wird in der Ansicht **Integritätsdiagnose** der vollständige Domänenname der VM anstelle des benutzerdefinierten VM-Namens angezeigt.
- Beim Herunterfahren von VMs werden einige ihrer Integritätskriterien zu einem kritischen Status aktualisiert, andere zu einem fehlerfreien Status, wobei der Netzwerkstatus der VMs kritisch gemeldet wird.
- Der Schweregrad von Integritätswarnungen kann nicht geändert werden, sie können lediglich aktiviert oder deaktiviert werden.  Außerdem werden einige Schweregrade auf der Grundlage des Status von Integritätskriterien aktualisiert.
- Das Ändern einer beliebigen Einstellung einer Instanz eines Integritätskriteriums führt zur Änderung der gleichen Einstellung in allen Instanzen von Integritätskriterien des gleichen Typs in der VM. Wenn beispielsweise der Schwellenwert der Instanz des Integritätskriteriums „verfügbarer Speicherplatz“, der dem logischen Laufwerk C: entspricht, geändert wird, gilt dieser Schwellenwert auch für alle anderen logischen Datenträger, die für die gleiche VM ermittelt wurden und überwacht werden.   
- Schwellenwerte für die folgenden Integritätskriterien, die auf eine Windows-VM abzielen, sind nicht veränderbar, da ihr Integritätszustand bereits auf **laufend** oder **verfügbar** gesetzt ist. Bei Abfrage der [Workload-Monitor-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) zeigt der Integritätszustand den *Vergleichsoperator*-Wert von **LessThan** oder **GreaterThan** mit einem *Schwellenwert*-Wert von **4** für den Dienst oder die Entität, wenn:
   - Integrität des DNS-Clientdiensts – Dienst wird nicht ausgeführt 
   - Integrität des DHCP-Clientdiensts – Dienst wird nicht ausgeführt 
   - Integrität des RPC-Diensts – Dienst wird nicht ausgeführt 
   - Integrität des Windows-Firewalldiensts– Dienst wird nicht ausgeführt
   - Integrität des Windows-Ereignisprotokolldiensts– Dienst wird nicht ausgeführt 
   - Integrität des Serverdiensts – Dienst wird nicht ausgeführt 
   - Integrität des Windows-Remoteverwaltungsdiensts– Dienst wird nicht ausgeführt 
   - Dateisystemfehler oder Beschädigungen – Logischer Datenträger nicht verfügbar

- Die Schwellenwerte für die folgenden Linux-Zustandskriterien sind nicht veränderbar, da ihr Integritätszustand bereits auf **wahr** gesetzt ist.  Der Integritätszustand zeigt den *Vergleichsoperator* mit einem Wert **LessThan** und *Schwellenwert* von **1**, wenn er von der Workload-Monitoring-API für die Entität abhängig von ihrem Kontext abgefragt wird:
   - Status des logischen Datenträgers – Logischer Datenträger ist nicht online/verfügbar
   - Datenträgerstatus – Datenträger ist nicht online/verfügbar
   - Netzwerkadapterstatus – Netzwerkadapter ist deaktiviert  

- Das Integritätskriterium **Gesamte CPU-Auslastung** in Windows zeigt einen Schwellenwert von **not equal to 4** vom Portal und bei einer Abfrage über die Workload-Monitoring-API, wenn die CPU-Auslastung größer als 95 % und die Länge der Systemwarteschlange größer als 15 ist. Diese Integritätskriterien kann in dieser Version nicht geändert werden.  
- Konfigurationsänderungen, wie z.B. das Aktualisieren eines Schwellenwerts, dauern bis zu 30 Minuten, bis sie wirksam werden, obwohl das Portal oder die Workload-Monitor-API möglicherweise sofort aktualisiert werden.  
- Integritätskriterien für einzelne Prozessoren und logische Prozessorstufen sind in Windows nicht verfügbar, nur **Gesamte CPU-Auslastung** ist für Windows-VMs verfügbar.  
- Die für jedes Integritätskriterium definierten Warnregeln werden im Azure-Portal nicht angezeigt. Sie sind nur über die [Workload-Monitor-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) konfigurierbar, um eine Integritätswarnregel zu aktivieren oder zu deaktivieren.  
- Die Zuweisung einer [Azur-Monitor-Aktionsgruppe](../monitoring-and-diagnostics/monitoring-action-groups.md) für Zustandswarnungen ist vom Azur-Portal aus nicht möglich. Sie müssen die API für die Benachrichtigungseinstellung verwenden, um eine Aktionsgruppe zu konfigurieren, die bei jeder Auslösung einer Integritätswarnung ausgelöst wird. Derzeit können Aktionsgruppen einer VM zugeordnet werden, sodass alle *Integritätswarnungen*, die für die VM ausgelöst wurden, die gleiche(n) Aktionsgruppe(n) ausgelöst haben. Es gibt kein Konzept für eine separate Aktionsgruppe für jede Integritätswarnregel, wie bei herkömmlichen Azure-Warnungen. Darüber hinaus werden nur Aktionsgruppen unterstützt, die so konfiguriert sind, dass sie durch Senden einer E-Mail oder SMS benachrichtigt werden, wenn Integritätswarnungen ausgelöst werden. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung Ihrer virtuellen Computer finden Sie unter [Führen Sie das Onboarding von Azure Monitor for VMs durch](monitoring-vminsights-onboard.md).