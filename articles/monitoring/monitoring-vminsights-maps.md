---
title: Anzeigen von Anwendungsabhängigkeiten mit Azure Monitor für VMs (Vorschau) | Microsoft-Dokumentation
description: Zuordnung ist ein Feature von Azure Monitor for VMs, das Anwendungskomponenten auf Windows- und Linux-Systemen automatisch ermittelt und die Kommunikation zwischen Diensten abbildet. Dieser Artikel enthält Details zu seiner Verwendung in einer Reihe von Szenarien.
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
ms.openlocfilehash: af3d2b997a4d58f2d32705c55151c4cf13bdcc7c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095477"
---
# <a name="using-azure-monitor-for-vms-preview-map-to-understand-application-components"></a>Verwenden der Zuordnung in Azure Monitor für VMs (Vorschau) zum Verstehen von Anwendungskomponenten
Das Anzeigen der ermittelten Anwendungskomponenten auf virtuellen Windows- und Linux-Computern, die in Ihrer Azure-Umgebung ausgeführt werden, kann mit Azure Monitor für VMs auf zwei Arten erfolgen: direkt in einem virtuellen Computer oder übergreifend für eine Gruppe von VMs in Azure Monitor. 

Dieser Artikel soll Ihnen die unterschiedliche Benutzererfahrung der beiden Perspektiven und die Verwendung des Zuordnungsfeatures nahebringen. Informationen zum Konfigurieren von Azure Monitor for VMs finden Sie unter [Enable Azure Monitor for VMs](monitoring-vminsights-onboard.md) (Aktivieren von Azure Monitor for VMs).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="introduction-to-map-experience"></a>Einführung in die Zuordnungsoberfläche
Bevor wir in das Anzeigen der Zuordnung für eine einzelne VM oder eine Gruppe von VMs einsteigen, müssen wir Ihnen eine kurze Einführung in das Feature geben, damit Sie verstehen, wie die Informationen dargestellt werden und wofür die Visualisierungen stehen.  

Gleich, ob Sie das Zuordnungsfeature direkt in einer VM oder in Azure Monitor auswählen, bietet es eine einheitliche Benutzererfahrung.  Der einzige Unterschied besteht darin, dass Sie in Azure Monitor alle Mitglieder einer mehrschichtigen Anwendung oder eines Clusters in einer Zuordnung sehen können.

Die Zuordnung visualisiert die Abhängigkeiten der VMs, indem sie laufende Prozesse mit aktiven Netzwerkverbindungen zwischen Servern, Latenz bei eingehenden und ausgehenden Verbindungen und Ports überall in einer per TCP verbundenen Architektur über einen angegebenen Zeitraum ermittelt.  Beim Erweitern einer VM werden Prozessdetails angezeigt, und nur die Prozesse, die mit der VM kommunizieren, werden dargestellt. Die Anzahl der Front-End-Clients, die Verbindungen mit der VM herstellen, wird in der Clientgruppe angezeigt. Die Anzahl der Back-End-Server, mit denen die VM Verbindungen herstellt, wird in den Serverportgruppen angezeigt. Erweitern Sie eine Serverportgruppe, um die Detailliste der über diesen Port verbundenen Server anzuzeigen.  

Wenn Sie auf den virtuellen Computer klicken, wird der Bereich **Eigenschaften** auf der rechten Seite erweitert, um die Eigenschaften des ausgewählten Elements anzuzeigen, wie die vom Betriebssystem gemeldeten Systeminformationen, Eigenschaften der Azure-VM und ein Ringdiagramm, das die ermittelten Verbindungen zusammenfasst. 

![Systemeigenschaften des Computers](./media/monitoring-vminsights-maps/properties-pane-01.png)

Klicken Sie auf der rechten Seite des Bereichs auf das Symbol **Protokollereignisse**, um den Fokus des Bereichs umzuschalten und eine Liste der Tabellen anzuzeigen, die aus der VM gesammelte Daten an Log Analytics gesendet haben, die zum Abfragen verfügbar sind.  Durch Klicken auf einen der aufgelisteten Datensatztypen wird die Seite **Protokolle** geöffnet und zeigt die Ergebnisse für den betreffenden Typ mithilfe einer vorkonfigurierten Abfrage an, die nach der bestimmten VM gefiltert ist.  

![Protokollsuchliste im Bereich „Eigenschaften“](./media/monitoring-vminsights-maps/properties-pane-logs-01.png)

Schließen Sie *Protokolle*\*, kehren Sie zum Bereich **Eigenschaften** zurück, und wählen Sie **Warnungen** aus, um Warnungen anzuzeigen, die von den Integritätskriterien für die VM ausgelöst wurden. Die Zuordnung ist in Azure-Warnungen integriert, um Warnungen für den ausgewählten Server anzuzeigen, die im ausgewählten Zeitraum ausgelöst werden. Für den Server wird ein Symbol angezeigt, wenn aktuelle Warnungen vorliegen, und im Computerwarnungsbereich werden die Warnungen aufgelistet. 

![Computerwarnungen im Bereich „Eigenschaften“](./media/monitoring-vminsights-maps/properties-pane-alerts-01.png)

Damit das Zuordnungsfeature die relevanten Warnungen anzeigen kann, müssen Sie eine Warnungsregel aufstellen, die für einen bestimmten Computer ausgelöst wird. So erstellen Sie richtige Warnungen:
- Beziehen Sie eine Klausel zur Gruppierung nach Computer ein (z.B. **by Computer interval 1 minute**).
- Wählen Sie die Warnung auf Basis der Metrikmessung.

Weitere Informationen zu Azure-Warnungen und zum Erstellen von Warnungsregeln finden Sie unter [Einheitliche Oberfläche für Warnungen in Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)

Die Option **Legende** in der oberen rechten Ecke beschreibt die Symbole und Rollen auf einer Zuordnung.  Um die Ansicht zu vergrößern, damit Sie einen genaueren Blick auf Ihre Zuordnung werfen können, und um in ihr zu navigieren, kann die Vergrößerung mit den Zoomsteuerelementen unten rechts auf der Seite festgelegt und die Seite an die Größe der aktuellen Seite angepasst werden.  

## <a name="connection-metrics"></a>Verbindungsmetriken
Im Bereich **Verbindungen** werden Standardverbindungsmetriken für die ausgewählte Verbindung von der VM über den TCP-Port angezeigt. Die Metriken beinhalten die Antwortzeit, die Anforderungen pro Minute, den Datenverkehrsdurchsatz und Links.  

![Beispiel für Diagramme im Bereich „Netzwerkverbindungen“](./media/monitoring-vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Verbindungsfehler
Verbindungsfehler werden in den Zuordnungen für Prozesse und Computer angezeigt. Eine gestrichelte rote Linie zeigt an, dass ein Clientsystem einen Prozess oder Port nicht erreichen kann. Verbindungsfehler werden von jedem System mit einem Dependency-Agent berichtet, wenn es sich dabei um das System handelt, das die fehlerhafte Verbindung herzustellen versucht. Das Zuordnungsfeature misst diesen Prozess durch Überwachen der TCP-Sockets, die keine Verbindung herstellen können. Der Fehler kann durch eine Firewall, eine fehlerhafte Konfiguration auf einem Client oder Server oder dadurch verursacht werden, dass ein Remotedienst nicht verfügbar ist.

![Beispiel für einen Verbindungsfehler in der Zuordnung](./media/monitoring-vminsights-maps/map-group-failed-connection-01.png)

Kenntnisse zu Verbindungsfehlern können Ihnen bei der Problembehandlung, der Überprüfung einer Migration, der Sicherheitsanalyse und dem Verständnis der allgemeinen Architektur des Diensts helfen. Manchmal sind Verbindungsfehler harmlos, häufig weisen sie aber auch direkt auf ein Problem hin, z.B. wenn eine Failoverumgebung plötzlich nicht mehr erreichbar ist oder zwei Anwendungsebenen nach einer Cloudmigration nicht mehr kommunizieren können.

### <a name="client-groups"></a>Clientgruppen
Clientgruppen in der Zuordnung stellen Clientcomputer dar, die über Verbindungen mit dem dargestellten Computer verfügen. Eine einzelne Clientgruppe stellt die Clients für einen einzelnen Prozess oder Computer dar.

![Beispiel für Clientgruppen in der Zuordnung](./media/monitoring-vminsights-maps/map-group-client-groups-01.png)

Um die überwachten Clients und IP-Adressen der Systeme in einer Clientgruppe anzuzeigen, wählen Sie die Gruppe aus. Die Inhalte der Gruppe werden neben der Gruppe aufgelistet.  

![Beispiel für eine IP-Adressliste einer Clientgruppe in der Zuordnung](./media/monitoring-vminsights-maps/map-group-client-group-iplist-01.png)

Wenn die Gruppen überwachte und nicht überwachte Clients beinhalten, können Sie den entsprechenden Abschnitt des Ringdiagramms auswählen, um nach den Clients zu filtern.

### <a name="server-port-groups"></a>Serverportgruppen
Serverportgruppen stellen Serverports auf Servern dar, für die eingehende Verbindungen vom dargestellten Computer bestehen. Die Gruppe enthält den Serverport zusammen mit der Anzahl von Servern mit Verbindungen zu diesem Port. Wählen Sie die Gruppe aus, um die einzelnen Server und Verbindungen aufgelistet anzuzeigen. 

![Beispiel für eine Serverportgruppe in der Zuordnung](./media/monitoring-vminsights-maps/map-group-server-port-groups-01.png)  

Wenn die Gruppen überwachte und nicht überwachte Server beinhalten, können Sie den entsprechenden Abschnitt des Ringdiagramms auswählen, um nach den Servern zu filtern.

## <a name="view-map-directly-from-a-virtual-machine"></a>Direktes Anzeigen der Zuordnung in einer VM 

Führen Sie die folgenden Schritte aus, um direkt aus einer VM auf Azure Monitor for VMs zuzugreifen.

1. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
2. Wählen Sie in der Liste eine VM und im Abschnitt **Überwachung** **Insights (Vorschau)** aus.  
3. Wählen Sie die Registerkarte **Zuordnung** aus.

Das Zuordnungsfeature visualisiert die Abhängigkeiten der VM, d.h. die ausgeführten Prozessgruppen und Prozesse mit aktiven Netzwerkverbindungen über einen angegebenen Zeitraum.  Standardmäßig zeigt die Zuordnung die letzten 30 Minuten.  Mithilfe des **TimeRange**-Selektors in der linken oberen Ecke können Sie nach historischen Zeiträumen (maximal eine Stunde) abfragen, um anzuzeigen, wie die Abhängigkeiten in der Vergangenheit aussahen, z.B. während eines Incidents oder vor einer Änderung.  

![Übersicht zur direkten VM-Zuordnung](./media/monitoring-vminsights-maps/map-direct-vm-01.png)

## <a name="view-map-from-azure-monitor"></a>Anzeigen der Zuordnung in Azure Monitor
In Azure Monitor stellt das Zuordnungsfeature eine globale Sicht Ihrer virtuellen Computer und ihrer Abhängigkeiten dar.  Führen Sie die folgenden Schritte aus, um in Azure Monitor auf das Zuordnungsfeature zuzugreifen. 

1. Wählen Sie im Azure-Portal die Option **Überwachen** aus. 
2. Wählen Sie im Bereich **Insights** **Virtuelle Computer (Vorschau)** aus.
3. Wählen Sie die Registerkarte **Zuordnung** aus.

![Übersicht einer Multi-VM-Zuordnung in Azure Monitor](./media/monitoring-vminsights-maps/map-multivm-azure-monitor-01.png)

Wählen Sie mit dem Selektor **Arbeitsbereich** oben auf der Seite – falls Sie über mehrere Log Analytics-Arbeitsbereiche verfügen – den Arbeitsbereich aus, der für die Lösung aktiviert ist und VMs aufweist, die an ihn berichten. Mit dem Selektor **Gruppe** werden Abonnements, Ressourcengruppen, [Computergruppen](../log-analytics/log-analytics-computer-groups.md) und VM-Skalierungsgruppen mit Computern zurückgegeben, die sich auf den ausgewählten Arbeitsbereich beziehen. Ihre Auswahl gilt nur für das Kartenfeature und nicht für die Integrität oder Leistung.

Standardmäßig zeigt die Zuordnung die letzten 30 Minuten. Mithilfe des **TimeRange**-Selektors können Sie nach historischen Zeiträumen (maximal eine Stunde) abfragen, um anzuzeigen, wie die Abhängigkeiten in der Vergangenheit aussahen, z.B. während eines Incidents oder vor einer Änderung.   

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwenden des Integritätsfeatures finden Sie unter [View Azure VM Health](monitoring-vminsights-health.md) (Anzeigen der Integrität von Azure-VMs); Informationen zum Erkennen von Engpässen und der Gesamtnutzung mit Ihrer VM-Leistung finden Sie unter [View Azure Monitor for VMs Performance](monitoring-vminsights-performance.md) (Anzeigen der Leistung von Azure Monitor for VMs). 
