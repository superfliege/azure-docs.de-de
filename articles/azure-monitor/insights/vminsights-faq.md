---
title: Häufig gestellte Fragen zu Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: Azure Monitor für VMs (Vorschauversion) ist eine Lösung in Azure, die die Integritäts- und die Leistungsüberwachung des Betriebssystems von Azure-VMs vereint. Sie ermittelt automatisch Anwendungskomponenten und Abhängigkeiten von anderen Ressourcen und stellt die Kommunikation zwischen diesen dar. Dieser Artikel bietet Antworten auf häufig gestellte Fragen.
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
ms.date: 11/08/2018
ms.author: magoedte
ms.openlocfilehash: a97a7be0eaa8438a4df27b610106ec6ab9f60d30
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184389"
---
# <a name="azure-monitor-for-vms-preview-faq"></a>Azure Monitor für VMs (Vorschauversion) – häufig gestellte Fragen
Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Azure Monitor für VMs. Wenn Sie weitere Fragen zu der Lösung haben, besuchen Sie das [Azure Diskussionsforum](https://feedback.azure.com/forums/34192--general-feedback), und stellen Sie dort Ihre Fragen. Wenn eine Frage häufiger gestellt wird, fügen wir sie zu diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="can-i-deploy-vms-to-an-existing-workspace"></a>Kann ich VMs in einem vorhandenen Arbeitsbereich bereitstellen?
Wenn Ihre virtuellen Computer bereits mit einem Log Analytics-Arbeitsbereich verbunden sind, können Sie diesen Arbeitsbereich nach dem Bereitstellen der Computer für Azure Monitor für VMs weiterhin verwenden. Der Arbeitsbereich muss in einer der unterstützten Regionen vorhanden sein, die im Abschnitt „Voraussetzungen“ des Artikels [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md#prerequisites) aufgeführt werden.

Während der Bereitstellung konfigurieren wir die Leistungsindikatoren für den Arbeitsbereich. Diese Aktion bewirkt, dass die virtuellen Computer, die Daten an den Arbeitsbereich melden, damit beginnen, die Informationen für die Anzeige und Analyse in Azure Monitor für VMs zu sammeln. Als Ergebnis sehen Sie Leistungsdaten aller mit dem ausgewählten Arbeitsbereich verbundenen VMs. Die Integritäts- und Zuordnungsfunktionen werden nur für die VMs aktiviert, für die Sie die Bereitstellung angegeben haben.

Weitere Informationen darüber, welche Leistungsindikatoren aktiviert sind, finden Sie unter [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md).

## <a name="can-i-deploy-vms-to-a-new-workspace"></a>Kann ich VMs in einem neuen Arbeitsbereich bereitstellen? 
Wenn Ihre VMs derzeit nicht mit einem vorhandenen Log Analytics-Arbeitsbereich verbunden sind, müssen Sie einen neuen Arbeitsbereich zum Speichern Ihrer Daten erstellen. Sie können automatisch einen Arbeitsbereich erstellen, indem Sie einen einzelnen virtuellen Computer für Azure Monitor für VMs im Azure-Portal konfigurieren.

Wenn Sie die skriptbasierte Methode verwenden möchten, beachten Sie den Artikel [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md). 

## <a name="what-can-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Wie gehe ich vor, wenn mein VM bereits an einen vorhandenen Arbeitsbereich berichtet?
Wenn Sie bereits Daten von Ihren virtuellen Computern sammeln, haben Sie diese Computer möglicherweise schon für das Melden von Daten an einen vorhandenen Log Analytics-Arbeitsbereich konfiguriert. Sofern sich dieser Arbeitsbereich in einer der von uns unterstützten Regionen befindet, können Sie Azure Monitor für VMs für diesen bereits vorhandenen Arbeitsbereich aktivieren. Wir arbeiten aktiv daran, weitere Regionen zu unterstützen.

>[!NOTE]
>Wir konfigurieren Leistungsindikatoren für den Arbeitsbereich, die alle VMs betreffen, die an den Arbeitsbereich berichten – unabhängig davon, ob Sie für diese Computer eine Bereitstellung für Azure Monitor für VMs vornehmen oder nicht. Weitere Informationen zur Konfiguration von Leistungsindikatoren für den Arbeitsbereich finden Sie im Abschnitt „Konfigurieren von Leistungsindikatoren“ des Artikels [Windows- und Linux-Leistungsindikatoren in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md). Informationen zu den für Azure Monitor für VMs konfigurierten Leistungsindikatoren finden Sie im Artikel [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md). 

## <a name="why-did-my-vm-deployment-fail"></a>Warum ist meine VM-Bereitstellung fehlgeschlagen?
Die Bereitstellung einer Azure-VM im Azure-Portal umfasst die folgenden Ereignisse:

* Es wird ein Log Analytics-Standardarbeitsbereich erstellt, wenn diese Option ausgewählt war.
* Die Leistungsindikatoren werden für den ausgewählten Arbeitsbereich konfiguriert. Wenn dieser Schritt fehlschlägt, zeigen einige Leistungsdiagramme und Tabellen für die bereitgestellte VM keine Daten an. Sie können dieses Problem durch Ausführen des PowerShell-Skripts beheben, das im Abschnitt „Aktivieren mithilfe von PowerShell“ des Artikels [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md#enable-with-powershell) aufgeführt ist.
* Der Log Analytics-Agent wird bei Bedarf mit einer VM-Erweiterung auf Azure-VMs installiert. 
* Der Map Dependency-Agent von Azure Monitor für VMs wird bei Bedarf mit einer Erweiterung auf Azure-VMs installiert. 
* Ggf. werden Azure Monitor-Komponenten zur Unterstützung des Integritätsfeatures konfiguriert, und die VM wird für das Melden von Integritätsdaten konfiguriert.

Während der Bereitstellung wird der Status für jeden der vorherigen Schritte überprüft und im Portal ein Benachrichtigungsstatus für Sie zurückgegeben. Die Konfiguration des Arbeitsbereichs und die Agentinstallation nehmen normalerweise 5 bis 10 Minuten in Anspruch. Das Anzeigen von Überwachungs- und Integritätsdaten im Azure-Portal kann weitere 5 bis 10 Minuten in Anspruch nehmen. 

Wenn Sie die Bereitstellung eingeleitet haben und Meldungen angezeigt werden, die besagen, dass die VM bereitgestellt werden muss, warten Sie bitte. Es kann bis zu 30 Minuten dauern, bis dieser Vorgang für die VM abgeschlossen ist. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>In den Leistungsdiagrammen für meine VM werden einige oder alle Daten nicht angezeigt
Wenn Sie in der Datenträgertabelle oder in einigen der Leistungsdiagramme keine Leistungsdaten sehen, wurden Ihre Leistungsindikatoren möglicherweise nicht im Arbeitsbereich konfiguriert. Führen Sie zum Beheben dieses Problems das PowerShell-Skript aus, das im Abschnitt „Aktivieren mithilfe von PowerShell“ des Artikels [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md#enable-with-powershell) aufgeführt ist.

## <a name="how-is-the-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Wie unterscheidet sich die Zuordnungsfunktion von Azure Monitor für VMs von der Dienstzuordnung?
Die Zuordnungsfunktion von Azure Monitor für VMs basiert auf der Dienstzuordnung, weist aber folgende Unterschiede auf:

* Auf die Zuordnungsansicht kann über das VM-Blatt und über Azure Monitor für VMs unter Azure Monitor zugegriffen werden.
* Auf die Verbindungen in der Zuordnung kann jetzt geklickt werden. Sie zeigen dann im Seitenbereich Verbindungsmetrikdaten an.
* Es gibt eine neue API, die zum Erstellen der Zuordnungen verwendet wird und eine bessere Unterstützung für komplexere Zuordnungen bietet.
* Überwachte VMs sind jetzt im Clientgruppenknoten enthalten, und das Ringdiagramm zeigt den Anteil der überwachten und nicht überwachten virtuellen Computer an. Sie können außerdem bei ausgeklappter Gruppe die Liste der Computer filtern.
* Überwachte virtuelle Computer sind jetzt in den Serverport-Gruppenknoten enthalten, und das Ringdiagramm zeigt den Anteil der überwachten und nicht überwachten Computer an. Sie können außerdem bei ausgeklappter Gruppe die Liste der Computer filtern.
* Das Format der Zuordnung wurde aktualisiert, um eine bessere Konsistenz mit der Anwendungsübersicht aus Azure Application Insights zu erreichen.
* Die Seitenbereiche wurden aktualisiert, sie verfügen jedoch noch nicht über alle Integrationen, die in der Dienstzuordnung unterstützt wurden: Updateverwaltung, Änderungsnachverfolgung, Sicherheit und Service Desk. 
* Die Option zum Auswählen von Gruppen und Computern für die Zuordnung wurde aktualisiert. Sie unterstützt jetzt Abonnements, Ressourcengruppen, Azure-Skalierungsgruppen für virtuelle Computer und Clouddienste.
* Sie können in der Zuordnungsfunktion von Azure Monitor für VMs keine neuen Computergruppen der Dienstzuordnung erstellen. 

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Warum werden in meinen Leistungsdiagrammen gepunktete Linien angezeigt?

Leistungsdiagramme zeigen aus verschiedenen Gründen gepunktete Linien anstelle von durchgezogenen Linien an:
* Möglicherweise ist die Datensammlung unvollständig. 

* Die Standardeinstellung für das Datensampling ist alle 60 Sekunden. Sie sehen möglicherweise gepunktete Linien, wenn Sie einen engen Zeitbereich für das Diagramm auswählen und Ihre Samplingfrequenz kleiner ist als die im Diagramm verwendete Bucketgröße. Lassen Sie uns annehmen, dass Sie eine Samplingfrequenz von 10 Minuten ausgewählt haben und jeder Bucket im Diagramm 5 Minuten entspricht. In diesem Fall sollte das Auswählen eines größeren Zeitbereichs für die Darstellung bewirken, dass die Diagrammlinien als durchgezogene Linien und nicht als Punkte angezeigt werden.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Werden in Azure Monitor for VMs Gruppen unterstützt?
Ja. Nachdem Sie den Dependency-Agent installiert haben, sammeln wir Informationen von den VMs, um Gruppen auf der Grundlage von Abonnement, Ressourcengruppe, VM-Skalierungsgruppe und Clouddiensten anzuzeigen. Wenn Sie die Dienstzuordnung verwendet und Computergruppen erstellt haben, werden diese Gruppen ebenfalls angezeigt. Computergruppen werden außerdem im Gruppenfilter angezeigt, wenn Sie die Gruppen für den angezeigten Arbeitsbereich erstellt haben. 

## <a name="how-can-i-display-the-details-about-whats-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Wie kann ich anzeigen, welche Faktoren die Linie für das 95. Perzentil in den Leistungsaggregatdiagrammen beeinflussen?
Standardmäßig wird die Liste so sortiert, dass Sie die virtuellen Computer sehen, die für die ausgewählte Metrik den höchsten Wert für das 95. Perzentil aufweisen. Eine Ausnahme hiervon ist das Diagramm **Verfügbarer Arbeitsspeicher**, das die Computer mit dem niedrigsten Wert für das fünfte Perzentil anzeigt. Klicken Sie auf das Diagramm, um die Ansicht **Top-N-Liste** mit der jeweils ausgewählten Metrik anzuzeigen.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-various-virtual-networks-and-subnets"></a>Wie behandelt die Zuordnungsfunktion doppelt vorhandene IPs in verschiedenen virtuellen Netzwerken und Subnetzen?
Wenn Sie in Subnetzen und virtuellen Netzwerken doppelt vorhandene IP-Adressbereiche für virtuelle Computer oder Azure-VM-Skalierungsgruppen verwenden, kann dies dazu führen, dass die Zuordnungsfunktion von Azure Monitor für VMs falsche Informationen anzeigt. Dies ist ein bekanntes Problem, und wir untersuchen Optionen, um dieses Verhalten zu verbessern.

## <a name="does-the-map-feature-support-ipv6"></a>Unterstützt die Zuordnungsfunktion IPv6?
Die Zuordnungsfunktion unterstützt aktuell nur IPv4, und wir untersuchen derzeit die Unterstützung für IPv6. Wir unterstützen ferner IPv4 mit Tunnelung in IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-why-is-the-map-difficult-to-view"></a>Warum ist die Zuordnung schwierig zu betrachten, wenn ich eine Zuordnung für eine Ressourcengruppe oder eine andere große Gruppe lade?
Zwar haben wir Verbesserungen an der Zuordnungsfunktion für die Handhabung großer und komplexer Konfigurationen vorgenommen, es ist uns aber bewusst, dass eine Zuordnung viele Knoten und Verbindungen sowie als Cluster fungierende Knoten aufweisen kann. Wir sind weiterhin bestrebt, die Unterstützung einer besseren Skalierbarkeit voranzutreiben.  

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-from-the-network-chart-on-the-azure-vm-overview-page"></a>Warum sieht das Netzwerkdiagramm auf der Registerkarte „Leistung“ anders aus als das Netzwerkdiagramm auf der Azure-VM-Übersichtsseite?

Die Übersichtsseite für eine Azure-VM zeigt Diagramme auf der Grundlage der vom Host gemessenen Aktivität in der Gast-VM an. Das Netzwerkdiagramm auf der Azure-VM-Übersichtsseite zeigt nur Netzwerkdatenverkehr an, der in Rechnung gestellt wird. Diese Anzeige enthält keinen Datenverkehr zwischen virtuellen Netzwerken. Die für Azure Monitor für VMs angezeigten Daten und Diagramme basieren auf Daten aus der Gast-VM, und das Netzwerkdiagramm zeigt den gesamten eingehenden und ausgehenden TCP/IP-Verkehr für diese VM an, einschließlich des Datenverkehrs zwischen virtuellen Netzwerken.

## <a name="what-are-the-limitations-of-the-log-analytics-free-pricing-plan"></a>Welche Einschränkungen gibt es im Free-Tarif von Log Analytics?
Wenn Sie Azure Monitor über den *Free*-Tarif mit einem Log Analytics-Arbeitsbereich konfiguriert haben, unterstützt die Zuordnungsfunktion von Azure Monitor für VMs maximal fünf Computer, die mit dem Arbeitsbereich verbunden sind. 

Nehmen wir beispielsweise an, dass Sie fünf virtuelle Computer mit einem kostenlosen Arbeitsbereich verbunden haben. Wenn Sie einen virtuellen Computer trennen und später einen anderen virtuellen Computer verbinden, wird der neue virtuelle Computer weder überwacht noch auf der Zuordnungsseite dargestellt. In diesem Szenario werden Sie beim Öffnen des neuen virtuellen Computers aufgefordert, die Option **Try Now** (Jetzt testen) zu verwenden und im linken Bereich **Insights (preview)** (Insights (Vorschau)) auszuwählen, selbst wenn Insights bereits auf dem virtuellen Computer installiert wurde. Allerdings werden Ihnen nicht die Optionen angeboten, die Sie normalerweise sehen würden, wenn der virtuelle Computer nicht für Azure Monitor für VMs bereitgestellt worden wäre. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung der Überwachung Ihrer virtuellen Computer finden Sie unter [Onboardingmethoden für den Azure Monitor für VMs (Vorschau)](vminsights-onboard.md).
