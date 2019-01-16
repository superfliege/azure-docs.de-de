---
title: Bekannte Probleme bei Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel werden bekannte Probleme erläutert, die bei Azure Monitor für VMs auftreten. Azure Monitor ist eine Lösung in Azure, die Integritätsüberwachung, Erkennung von Anwendungsabhängigkeiten und Leistungsüberwachung für das Betriebssystem von Azure-VMs vereint.
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
ms.date: 01/09/2018
ms.author: magoedte
ms.openlocfilehash: 038c6afe94ccfea707eea3b4032a2e45f69e5102
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54187073"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekannte Probleme bei Azure Monitor für VMs (Vorschauversion)

In diesem Artikel werden bekannte Probleme erläutert, die bei Azure Monitor für VMs auftreten. Azure Monitor ist eine Lösung in Azure, die Integritätsüberwachung, Erkennung von Anwendungskomponenten und Leistungsüberwachung für das Betriebssystem von Azure-VMs vereint. 

## <a name="health"></a>Integrität 
Die folgende Aufstellung enthält bekannte Probleme bei der aktuellen Version der Integritätsfunktion:

- Wenn eine Azure-VM entfernt oder gelöscht wird, wird dies für einige Zeit in der VM-Listenansicht angezeigt. Wenn Sie außerdem auf den Zustand einer entfernten oder gelöschten VM klicken, wird die Ansicht **Integritätsdiagnosen** geöffnet und anschließend eine Ladeschleife initiiert. Durch Auswählen des Namens der gelöschten VM wird ein Bereich geöffnet mit der Meldung, dass die VM gelöscht wurde.
- Konfigurationsänderungen, wie z. B. das Aktualisieren eines Schwellenwerts, dauern bis zu 30 Minuten, selbst wenn sie möglicherweise vom Portal oder der Workloadüberwachungs-API sofort aktualisiert werden. 
- Die Benutzeroberfläche der Integritätsdiagnose wird schneller aktualisiert als die anderen Ansichten. Daher kann es beim Wechseln zwischen Ansichten zu Informationsverzögerungen kommen. 
- Beim Herunterfahren von VMs werden einige Integritätskriterien auf *kritisch* und andere auf *fehlerfrei* aktualisiert. Der Netzwerkstatus der VM wird als *kritisch* angezeigt.
- Für virtuelle Linux-Computer enthält der Titel der Seite, auf der die Integritätskriterien für eine einzelne VM-Ansicht aufgelistet wird, den gesamten Domänenname des virtuellen Computers anstelle des benutzerdefinierten VM-Namens. 
- Wenn Sie die Überwachung für eine VM mithilfe einer der unterstützten Methoden deaktiviert haben und versuchen, diese erneut bereitzustellen, sollte dies im gleichen Arbeitsbereich erfolgen. Wenn Sie einen anderen Arbeitsbereich auswählen und versuchen, den Integritätszustand für diese VM anzuzeigen, tritt möglicherweise ein inkonsistentes Verhalten auf.
- Das Integritätskriterium „Gesamte CPU-Auslastung“ für Windows zeigt einen Schwellenwert von *ungleich* **4**. Das bedeutet, dass die CPU-Auslastung höher als 95 % ist und die Länge der Systemwarteschlange 15 übersteigt. Dieses Integritätskriterium ist in dieser Vorschauversion nicht konfigurierbar.  
- Nach dem Entfernen der Lösungskomponenten aus dem Arbeitsbereich werden in den jeweiligen Ansichten des Portals weiterhin Daten zum Integritätszustand Ihrer Azure-VMs sowie Leistungs- und Zuordnungsdaten angezeigt. In den Ansichten für Leistung und Zuordnung werden nach einiger Zeit keine Daten mehr angezeigt, in der Integritätsansicht dagegen wird weiterhin der Integritätsstatus Ihrer VMs angezeigt. Die Option **Jetzt testen** steht nur zum erneuten Integrieren aus den Ansichten für Leistung und Zuordnung zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung der Überwachung Ihrer virtuellen Computer finden Sie unter [Deploy Azure Monitor for VMs (Bereitstellen von Azure Monitor für VMs)](vminsights-onboard.md).
