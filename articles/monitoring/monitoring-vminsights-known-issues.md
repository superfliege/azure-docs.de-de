---
title: Azure Monitor for VMs – bekannte Probleme | Microsoft-Dokumentation
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
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 819c3e74355cf80c7a998abb8b02b10c9e077059
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47062767"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Bekannte Probleme bei Azure Monitor for VMs

Die folgende Aufstellung enthält bekannte Probleme beim Integritätsfeature von Azure Monitor for VMs:

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
- Die Schwellenwerte für einige Windows-Integritätskriterien, wie die Integrität des DNS-Clientdiensts, können nicht geändert werden, da ihr fehlerfreier Status bereits im Status **wird ausgeführt**, **verfügbar** des Diensts oder der Entität – je nach Kontext – gesperrt ist.  Stattdessen wird der Wert durch die Zahl 4 dargestellt – diese wird in einer kommenden Version in die eigentliche Anzeigezeichenfolge konvertiert.  
- Schwellenwerte für einige Linux-Integritätskriterien lassen sich nicht ändern, wie etwa die Integrität von logischen Datenträgern, da für sie bereits die Auslösung bei fehlerhaftem Zustand festgelegt ist.  Diese Schwellenwerte zeigen an, ob etwas online/offline oder ein oder aus ist und werden durch die Werte 1 und 0 dargestellt und auch so angezeigt.
- Das Aktualisieren des Ressourcengruppenfilters in einer beliebigen Ressourcengruppe bei Verwendung der Massenansicht in der Listenansicht Azure Monitor > „Virtual Machines“ > „Integrität“ > „Alle“ mit vorausgewähltem Abonnement und vorausgewählter Ressourcengruppe bewirkt, dass die Listenansicht **Kein Ergebnis** anzeigt.  Wechseln Sie zurück zur Registerkarte Azure Monitor > „Virtual Machines“ > „Integrität“, wählen Sie das gewünschte Abonnement und die gewünschte Ressourcengruppe aus, und navigieren Sie dann zur Listenansicht.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung Ihrer virtuellen Computer finden Sie unter [Führen Sie das Onboarding von Azure Monitor for VMs durch](monitoring-vminsights-onboard.md).