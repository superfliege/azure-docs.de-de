---
title: Häufig gestellte Fragen zu Azure Monitor for VMs | Microsoft-Dokumentation
description: Azure Monitor for VMs ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. Dieser Artikel beantwortet häufig gestellte Fragen.
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
ms.openlocfilehash: e8723bd8e26eb66d9333c2093b5ada8bc78ca14b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989600"
---
# <a name="azure-monitor-for-vms-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Monitor for VMs
Dieser Microsoft-Artikel enthält eine Liste häufig gestellter Fragen zu Azure Monitor für VMs in Microsoft Azure. Wenn Sie weitere Fragen zur Lösung haben, besuchen Sie das [Diskussionsforum](https://feedback.azure.com/forums/34192--general-feedback), und stellen Sie Ihre Fragen. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="can-i-onboard-to-an-existing-workspace"></a>Kann ich ein Onboarding zu einem vorhandenen Arbeitsbereich ausführen?
Wenn Ihre virtuellen Computer bereits mit einem Log Analytics-Arbeitsbereich verbunden sind, können Sie diesen Arbeitsbereich nach dem Onboarding von Azure Monitor for VMs weiterhin verwenden, vorausgesetzt, er befindet sich in einer der unterstützten Regionen, die [hier](monitoring-vminsights-onboard.md#prerequisites) aufgeführt sind.

Beim Onboarding konfigurieren wir Leistungsindikatoren für den Arbeitsbereich, was dazu führt, dass alle VMs, die Daten an den Arbeitsbereich melden, mit dem Sammeln dieser Daten für die Anzeige und Analyse in Azure Monitor for VMs beginnen.  Im Ergebnis sehen Sie Leistungsdaten von allen mit dem ausgewählten Arbeitsbereich verbundenen VMs.  Das Integritäts- und das Zuordnungsfeature werden nur für die VMs aktiviert, für die Sie Onboarding angegeben haben.

Weitere Informationen zu den aktivierten Leistungsindikatoren finden Sie in unserem Artikel zum [Onboarding](monitoring-vminsights-onboard.md).

## <a name="can-i-onboard-to-a-new-workspace"></a>Kann ich ein Onboarding zu einem neuen Arbeitsbereich ausführen? 
Wenn Ihre VMs derzeit nicht mit einem vorhandenen Log Analytics-Arbeitsbereich verbunden sind, müssen Sie einen neuen Arbeitsbereich zum Speichern Ihrer Daten erstellen.  Die Erstellung eines neuen Standardarbeitsbereichs erfolgt automatisch, wenn Sie eine einzelne Azure-VM im Azure-Portal für Azure Monitor for VMs konfigurieren.

Diese Schritte werden für die skriptbasierte Methode im [Onboarding](monitoring-vminsights-onboard.md)-Artikel beschrieben. 

## <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Wie gehe ich vor, wenn meine VM bereits an einen vorhandenen Arbeitsbereich berichtet?
Wenn Sie bereits Daten von Ihren VMs sammeln, haben Sie sie möglicherweise schon für das Melden von Daten an einen vorhandenen Log Analytics-Arbeitsbereich konfiguriert.  Sofern sich dieser Arbeitsbereich in einer der von uns unterstützten Regionen befindet, können Sie Azure Monitor for VMs für diesen bereits vorhandenen Arbeitsbereich aktivieren.  Wenn sich der von Ihnen bereits verwendete Arbeitsbereich nicht in einer der von uns unterstützten Regionen befindet, können Sie derzeit kein Onboarding von Azure Monitor for VMs ausführen.  Wir arbeiten aktiv daran, weitere Regionen zu unterstützen.

>[!NOTE]
>Wir konfigurieren Leistungsindikatoren für den Arbeitsbereich, die alle VMs betreffen, die an den Arbeitsbereich berichten, unabhängig davon, ob Sie sich entschieden haben, für diese ein Onboarding für Azure Monitor for VMs auszuführen. Weitere Informationen über die Konfiguration von Leistungsindikatoren für den Arbeitsbereich finden Sie in unserer [Dokumentation](../log-analytics/log-analytics-data-sources-performance-counters.md). Informationen zu den für Azure Monitor for VMs konfigurierten Leistungsindikatoren finden Sie in unserer [Onboardingdokumentation](monitoring-vminsights-onboard.md#performance-counters-enabled).  

## <a name="why-did-my-vm-fail-to-onboard"></a>Warum ist das Onboarding meiner VM nicht gelungen?
Beim Onboarding einer Azure-VM im Azure-Portal werden die folgenden Schritte ausgeführt:

* Es wird ein Log Analytics-Standardarbeitsbereich erstellt, wenn diese Option ausgewählt war.
* Die Leistungsindikatoren werden für den ausgewählten Arbeitsbereich konfiguriert. Wenn bei diesem Schritt Fehler auftreten, werden Sie bemerken, dass einige Leistungsdiagramme und Tabellen für die VM, für die Sie das Onboarding ausgeführt haben, keine Daten anzeigen. Sie können dies korrigieren, indem Sie das [hier](monitoring-vminsights-onboard.md#enable-with-powershell) dokumentierte PowerShell-Skript ausführen.
* Der Log Analytics-Agent wird auf Azure-VMs mithilfe einer VM-Erweiterung installiert, wenn festgestellt wird, dass er erforderlich ist.  
* Der Dependency-Agent der Azure Monitor for VM-Zuordnung wird auf Azure-VMs mithilfe einer Erweiterung installiert, wenn festgestellt wird, dass er erforderlich ist.  
* Ggf. werden Azure Monitor-Komponenten zur Unterstützung des Integritätsfeatures konfiguriert, und die VM wird für das Melden von Integritätsdaten konfiguriert.

Während des Onboardingprozesses überprüfen wir den Status jeder der oben genannten Komponenten, um Ihnen im Portal einen Benachrichtigungsstatus zurückzugeben.  Die Konfiguration des Arbeitsbereichs und die Agentinstallation nehmen normalerweise 5 bis 10 Minuten in Anspruch.  Bis zur Anzeige von Überwachungs- und Integritätsdaten im Portal vergehen weitere 5 bis 10 Minuten.  

Wenn Sie das Onboarding eingeleitet haben und Meldungen angezeigt werden, die besagen, dass für die VM Onboarding ausgeführt werden muss, sehen Sie bis zu 30 Minuten bis zum Abschluss dieses Vorgangs durch die VM vor. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>In den Leistungsdiagrammen für meine VM werden einige oder alle Daten nicht angezeigt
Wenn Sie in der Datenträgertabelle oder in einigen der Leistungsdiagramme keine Leistungsdaten sehen, wurden Ihre Leistungsindikatoren im Arbeitsbereich möglicherweise nicht konfiguriert. Führen Sie das folgende [PowerShell-Skript](monitoring-vminsights-onboard.md#enable-with-powershell) aus, um das Problem zu beheben.

## <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Wie unterscheidet sich das Zuordnungsfeature von Azure Monitor for VMs von der Dienstzuordnung?
Das Zuordnungsfeature von Azure Monitor for VMs basiert auf der Dienstzuordnung, weist aber die folgenden Unterschiede auf:

* Auf die Zuordnungsansicht kann über das VM-Blatt und über Azure Monitor for VMs unter Azure Monitor zugegriffen werden.
* Auf die Verbindungen in der Zuordnung kann jetzt geklickt werden. Sie zeigen dann eine Ansicht der Verbindungsmetrikdaten für die ausgewählte Verbindung in der Seitenleiste an.
* Es gibt eine neue API, die zum Erstellen der Zuordnungen verwendet wird und eine bessere Unterstützung für komplexere Zuordnungen bietet.
* Überwachte VMs sind jetzt im Clientgruppenknoten enthalten, und das Ringdiagramm zeigt den Anteil der überwachten und nicht überwachten VMs in der Gruppe an.  Sie können außerdem bei ausgeklappter Gruppe zum Filtern der Liste der Computer verwendet werden.
* Überwachte VMs sind jetzt in den Serverport-Gruppenknoten enthalten, und das Ringdiagramm zeigt den Anteil der überwachten und nicht überwachten Computer in der Gruppe an.  Sie können außerdem bei ausgeklappter Gruppe zum Filtern der Liste der Computer verwendet werden.
* Das Format der Zuordnung wurde aktualisiert, um eine bessere Konsistenz mit der App-Übersicht aus Application Insights zu erreichen.
* Die Seitenleisten wurden aktualisiert, weisen aber noch nicht den gesamten Integrationssatz auf, der in der Dienstzuordnung unterstützt wurde – Updateverwaltung, Änderungsnachverfolgung, Sicherheit und Service Desk. 
* Die Option zum Auswählen von Gruppen und Computern für die Zuordnung wurde aktualisiert und unterstützt jetzt Abonnements, Ressourcengruppen Skalierungsgruppen von Azure-VMs und Clouddienste.
* Sie können im Zuordnungsfeature von Azure Monitor for VMs keine neuen Computergruppen der Dienstzuordnung erstellen.  

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Warum werden in meinen Leistungsdiagrammen gepunktete Linien angezeigt?

Dies kann aus einer Reihe von Gründen passieren.  In Fällen, in denen eine Lücke in der Datensammlung besteht, stellen wir die Linien als gepunktet dar.  Wenn Sie die Erfassungshäufigkeit der Daten für die aktivierten Leistungsindikatoren geändert haben (in der Standardeinstellung werden Daten alle 60 Sekunden gesammelt), werden gepunktete Linien im Diagramm angezeigt, wenn Sie einen kleinen Zeitbereich für das Diagramm gewählt haben und Ihre Erfassungshäufigkeit geringer als die im Diagramm verwendete Bucketgröße ist (beispielsweise, wenn die Erfassungshäufigkeit 10 Minuten beträgt und die einzelnen Buckets im Diagramm 5 Minuten groß sind).  In diesem Fall sollte das Wählen eines größeren Zeitbereichs für die Darstellung bewirken, dass die Diagrammlinien als durchgezogene Linien statt als Punkte angezeigt werden.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Werden in Azure Monitor for VMs Gruppen unterstützt?
Das Leistungsfeature unterstützt Gruppen, die auf den hervorgehobenen Ressourcen in einem bestimmten Arbeitsbereich basieren, sowie Gruppierung auf der Grundlage einer bestimmten Azure-VM-Skalierungsgruppe und eines Clouddiensts.

## <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Wie kann ich anzeigen, welche Faktoren die Linie für das 95. Quantil in den Leistungsaggregatdiagrammen beeinflussen?
Standardmäßig ist die Liste so sortiert, dass die VMs mit dem höchsten Wert für das 95. Quantil für die ausgewählte Metrik angezeigt werden, mit Ausnahme des Diagramms für den verfügbaren Arbeitsspeicher, das die Computer mit dem niedrigsten Wert für das 5. Quantil anzeigt.  Durch Klicken auf das Diagramm wird die **Top-N-Listenansicht** angezeigt; die entsprechende Metrik ist ausgewählt.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>Wie behandelt das Zuordnungsfeature doppelt vorhandene IPs in verschiedenen Vnets und Subnetzen?
Wenn Sie in Subnetzen und Vnets doppelt vorhandene IP-Adressbereiche für VMs oder Azure-VM-Skalierungsgruppen verwenden, kann das dazu führen, dass die Azure Monitor for VMs-Zuordnung falsche Informationen anzeigt. Dies ist ein bekanntes Problem, und wir untersuchen Optionen, dieses Verhalten zu verbessern.

## <a name="does-map-feature-support-ipv6"></a>Unterstützt das Zuordnungsfeature IPv6?
Das Zuordnungsfeature unterstützt aktuell nur IPv4, und wir untersuchen derzeit die Unterstützung für IPv6. Wir unterstützen ferner IPv4 mit Tunnelung in IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Wenn ich eine Zuordnung für eine Ressourcengruppe oder eine andere große Gruppe lade, ist die Zuordnung schwierig zu betrachten.
Zwar haben wir Verbesserungen an der Zuordnung bei der Behandlung großer und komplexer Konfigurationen vorgenommen, es ist uns aber bewusst, dass eine Zuordnung viele Knoten und Verbindungen sowie Knoten aufweisen kann, die als Cluster fungieren.  Wir sind weiterhin bestrebt, die Unterstützung einer besseren Skalierbarkeit voranzutreiben.   

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>Warum sieht das Netzwerkdiagramm auf der Registerkarte „Leistung“ anders aus als das Netzwerkdiagramm auf der Azure-VM-Übersichtsseite?

Die Übersichtsseite für eine Azure-VM zeigt Diagramme auf der Grundlage der vom Host gemessenen Aktivität in der Gast-VM an.  Für das Netzwerkdiagramm in der Azure-VM-Übersicht wird nur Netzwerkdatenverkehr angezeigt, der in Rechnung gestellt wird.  Dies schließt Datenverkehr zwischen Vnets nicht ein.  Die für Azure Monitor for VMs angezeigten Daten und Diagramme basieren auf Daten aus der Gast-VM, und das Netzwerkdiagramm zeigt den gesamten eingehenden und ausgehenden TCP/IP-Verkehr für die VM an, einschließlich des Datenverkehrs zwischen Vnets.
