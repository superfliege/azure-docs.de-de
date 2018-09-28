---
title: Darstellen der Leistung in Diagrammen mit Azure Monitor for VMs | Microsoft-Dokumentation
description: Leistung ist ein Feature von Azure Monitor for VMs, das Anwendungskomponenten auf Windows- und Linux-Systemen automatisch ermittelt und die Kommunikation zwischen Diensten abbildet. Dieser Artikel enthält Details zu seiner Verwendung in einer Reihe von Szenarien.
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
ms.date: 09/17/2018
ms.author: magoedte
ms.openlocfilehash: 06073197254245727cfa41020f060d904a4e50f9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957559"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Darstellen der Leistung in Diagrammen mit Azure Monitor for VMs
Azure Monitor for VMs beinhaltet einen Satz Leistungsdiagramme, die auf verschiedene Key Performance Indicators (KPIs) abzielen, um Sie beim Bestimmen der Leistung eines virtuellen Computers zu unterstützen. Die Diagramme zeigen die Ressourcennutzung über einen Zeitraum an, damit Sie Engpässe und Anomalien erkennen oder zu einer Perspektive wechseln können, in der jede VM aufgelistet ist, um die Ressourcennutzung nach der ausgewählten Metrik anzuzeigen. Zwar gibt es beim Thema Leistung eine Vielzahl von Elementen zu berücksichtigen, Azure Monitor for VMs legt den Schwerpunkt jedoch auf das System im Betrieb in Form von Prozessor, Arbeitsspeicher, Netzwerkadaptern und Datenträgern. Leistung ergänzt das Feature zur Integritätsüberwachung und hilft bei der Offenlegung von Problemen, die auf einen möglichen Ausfall von Systemkomponenten hinweisen, Feinabstimmung und Optimierung unterstützen, um Effizienz zu erreichen, oder bei der Kapazitätsplanung helfen.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Multi-VM-Perspektive in Azure Monitor
In Azure Monitor stellt das Leistungsfeature eine Multi-VM-Ansicht aller überwachten VMs zur Verfügung, die über alle Ressourcengruppen in Ihren Abonnements oder in Ihrer Umgebung bereitgestellt sind.  Führen Sie für den Zugriff aus Azure Monitor die folgenden Schritte aus. 

1. Wählen Sie im Azure-Portal **Überwachen** aus. 
2. Wählen Sie im Abschnitt **Lösungen** **Virtuelle Computer (Vorschau)** aus.
3. Wählen Sie die Registerkarte **Leistung** aus.

![Top-N-Listenansicht Leistung in VM Insights](./media/monitoring-vminsights-performance/vminsights-performance-aggview-01.png)

Wählen Sie auf der Registerkarte **Top-N-Diagramme** – falls Sie über mehrere Log Analytics-Arbeitsbereiche verfügen – mit dem **Arbeitsbereich**-Selektor oben auf der Seite den Arbeitsbereich aus, der in die Lösung integriert ist.  Wählen Sie anschließend mit dem Selektor **Gruppe** ein Abonnement, eine Ressourcengruppe oder einen bestimmten Computer über einen bestimmten Zeitraum aus.  Standardmäßig zeigen die Diagramme die letzten 24 Stunden an.  Mithilfe des **TimeRange**-Selektors können Sie nach historischen Zeiträumen von bis zu 30 Tagen abfragen, um darzustellen, wie die Leistung in der Vergangenheit war.   

Diese fünf Diagramme zur Kapazitätsauslastung werden auf der Seite angezeigt:

* CPU-Auslastung % – zeigt die 5 Computer mit der höchsten durchschnittlichen Prozessorauslastung an 
* Verfügbarer Arbeitsspeicher – zeigt die 5 Computer mit der kleinsten Menge an verfügbarem Arbeitsspeicher an 
* Genutzter Speicherplatz auf logischen Datenträgern % – zeigt die 5 Computer mit der höchsten durchschnittlichen Speicherplatznutzung über alle Datenträgervolumes in Prozent an 
* Rate der gesendeten Bytes – zeigt die 5 Computer mit der durchschnittlich größten Anzahl an gesendeten Bytes an 
* Rate der empfangenen Bytes – zeigt die 5 Computer mit der durchschnittlich größten Anzahl an empfangenen Bytes an 

Durch Klicken auf die obere rechte Ecke jedes der fünf Diagramme wird die **Top-N-Listenansicht** geöffnet.  Hier sehen Sie die Ressourcennutzung für die betreffende Leistungsmetrik nach einzelnen VMs in einer Listenansicht und den Computer mit der tendenziell höchsten Nutzung.  

![Top-N-Listenansicht für eine ausgewählte Leistungsmetrik](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Wenn Sie auf den virtuellen Computer klicken, wird der Bereich **Eigenschaften** auf der rechten Seite erweitert, um die Eigenschaften des ausgewählten Elements anzuzeigen, wie die vom Betriebssystem gemeldeten Systeminformationen, Eigenschaften der Azure-VM usw. Durch Klicken auf eine der Optionen im Abschnitt **Quicklinks** werden Sie direkt von der ausgewählten VM zu dem betreffenden Feature weitergeleitet.  

![Bereich „Eigenschaften von virtuellen Computern“](./media/monitoring-vminsights-performance/vminsights-properties-pane-01.png)

Wechseln Sie zur Registerkarte **Aggregatdiagramme**, um die Leistungsmetriken nach dem Durchschnitt oder nach Quantilskennzahlen gefiltert anzuzeigen.  

![Aggregatansicht Leistung in VM Insights](./media/monitoring-vminsights-performance/vminsights-performance-aggview-02.png)

Die folgenden Diagramme zur Kapazitätsauslastung stehen zur Verfügung:

* CPU-Auslastung % – zeigt standardmäßig den Mittelwert und das obere 95. Quantil an 
* Verfügbarer Arbeitsspeicher – zeigt standardmäßig den Mittelwert und das obere 5. und 10. Quantil an 
* Genutzter Speicherplatz auf logischen Datenträgern % – zeigt standardmäßig den Mittelwert und das 95. Quantil an 
* Rate der gesendeten Bytes – zeigt standardmäßig den Mittelwert für gesendete Bytes an 
* Rate der empfangenen Bytes – zeigt standardmäßig den Mittelwert für empfangene Bytes an

Außerdem haben Sie die Möglichkeit, die Granularität der Diagramme innerhalb des Zeitraums durch Auswahl von **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Quantilselektor zu ändern.   

Um die Ressourcennutzung der einzelnen VMs in einer Listenansicht darzustellen und zu sehen, welcher Computer tendenziell die höchste Nutzung aufweist, wählen Sie die Registerkarte **Top-N-Liste** aus.  Auf der Seite **Top-N-Liste** werden die 20 Computer mit der höchsten Nutzung nach dem 95. Quantil für die Metrik *CPU-Auslastung %* angezeigt.  Sie können weitere Computer anzeigen, indem Sie **Weitere laden** auswählen, und die Ergebnisse werden erweitert, um die oberen 500 Computer anzuzeigen. 

>[!NOTE]
>Die Liste kann nicht mehr als jeweils 500 Computer darstellen.  
>

![Beispiel für die Top-N-Liste-Seite](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Um die Ergebnisse für eine bestimmte VM in der Liste zu filtern, geben Sie ihren Computernamen in das Textfeld **Nach Namen suchen** ein.  

Wenn Sie lieber die Nutzung für eine andere Leistungsmetrik anzeigen möchten, wählen Sie in der Dropdownliste **Metrik** **Verfügbarer Arbeitsspeicher**, **Genutzter Speicherplatz auf logischen Datenträgern %**, **Netzwerk – empfangene Byte/s** oder **Netzwerk – gesendete Byte/s** aus, dann wird die Liste aktualisiert, um die Auslastung im Bereich der betreffenden Metrik darzustellen.  

Durch Auswählen eines virtuellen Computers in der Liste wird der Bereich **Eigenschaften** rechts auf der Seite geöffnet, und hier können Sie **Leistungsdetails** auswählen.  Die Seite **VM-Details** wird mit dem Bereich der betreffenden VM geöffnet; die Benutzeroberfläche beim direkten Zugriff auf VM Insights – Leistung aus der Azure VM ist ähnlich.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Direktes Anzeigen der Leistung in einer Azure-VM
Führen Sie für den direkten Zugriff aus einer VM die folgenden Schritte aus.

1. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
2. Wählen Sie in der Liste eine VM und im Abschnitt **Überwachung** **Insights (Vorschau)** aus.  
3. Wählen Sie die Registerkarte **Leistung** aus. 

Diese Seite enthält nicht nur Diagramme zur Leistungsauslastung, sondern auch eine Tabelle, die für jeden ermittelten logischen Datenträger dessen Kapazität, die Auslastung und den Gesamtmittelwert für jede Kennzahl anzeigt.  

Die folgenden Diagramme zur Kapazitätsauslastung stehen zur Verfügung:

* CPU-Auslastung % – zeigt standardmäßig den Mittelwert und das obere 95. Quantil an 
* Verfügbarer Arbeitsspeicher – zeigt standardmäßig den Mittelwert und das obere 5. und 10. Quantil an 
* Genutzter Speicherplatz auf logischen Datenträgern % – zeigt standardmäßig den Mittelwert und das 95. Quantil an 
* Logischer Datenträger – IOPs – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Logischer Datenträger – MB/s – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Logischer Datenträger – max. Nutzung – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Rate der gesendeten Bytes – zeigt standardmäßig den Mittelwert für gesendete Bytes an 
* Rate der empfangenen Bytes – zeigt standardmäßig den Mittelwert für empfangene Bytes an

![Ansicht von VM Insights – Leistung direkt in der VM](./media/monitoring-vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung des Integritätsfeatures finden Sie unter [View Azure Monitor for VMs Health](monitoring-vminsights-health.md) (Azure Monitor for VMs – Integrität anzeigen); Informationen zum Anzeigen entdeckter Anwendungsabhängigkeiten finden Sie unter [View Azure Monitor for VMs Map](monitoring-vminsights-maps.md) (Azure Monitor for VMs – Zuordnung anzeigen). 