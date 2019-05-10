---
title: Includedatei
description: Includedatei
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 4/30/2019
ms.author: shants
ms.custom: include file
ms.openlocfilehash: 747fb9a38cc0c27d162192f4f3ed928e8a968f27
ms.sourcegitcommit: abeefca6cd5ca01c3e0b281832212aceff08bf3e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "64993107"
---
Azure aktualisiert die Plattform regelmäßig, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur für virtuelle Computer zu verbessern. Diese Updates reichen von Patches für Softwarekomponenten in der Hostumgebung über Upgrades für Netzwerkkomponenten bis hin zur Außerbetriebsetzung von Hardware. Die meisten dieser Updates haben keine Auswirkungen auf die gehosteten virtuellen Computer. Allerdings gibt es Fälle, in denen Updates Auswirkungen haben und in denen Azure das Verfahren mit den geringsten Auswirkungen für Updates wählt:

- Wenn ein Update ohne Neustart möglich ist, wird die VM angehalten, während der Host aktualisiert wird, oder sie wird in Echtzeit zu einem bereits aktualisierten Host migriert.

- Wenn die Wartung einen Neustart erfordert, werden Sie in einer Benachrichtigung über den geplanten Wartungstermin informiert. Azure räumt Ihnen außerdem ein Zeitfenster ein, in dem Sie die Wartung zu einem Zeitpunkt, der Ihnen passt, selbst starten können. Das Zeitfenster für die selbstständige Wartung umfasst in der Regel vier Wochen, sofern die Wartung nicht dringend ist. Azure setzt sich auch für Technologien ein, die die Zahl der Fälle verringern sollen, in denen VMs aus Gründen der planmäßigen Plattformwartung neu gestartet werden müssen. 

Auf dieser Seite erfahren Sie, wie Azure die beiden Wartungsarten durchführt. Weitere Informationen zu ungeplanten Ereignissen (Ausfällen) finden Sie unter „Verwalten der Verfügbarkeit virtueller Computer“ für [Windows](../articles/virtual-machines/windows/manage-availability.md) oder [Linux](../articles/virtual-machines/linux/manage-availability.md).

Mithilfe der geplanten Ereignisse für [Windows](../articles/virtual-machines/windows/scheduled-events.md) oder [Linux](../articles/virtual-machines/linux/scheduled-events.md) können Sie in der VM Benachrichtigungen zu bevorstehender Wartung erhalten.

Weitere Informationen zum Verwalten der geplanten Wartung finden Sie unter „Behandlung von Benachrichtigungen der geplanten Wartung“ für [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) oder [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-not-requiring-a-reboot"></a>Wartungsmaßnahmen, die keinen Neustart erfordern

Das Ziel für die meisten Wartungsmaßnahmen mit Auswirkungen, die keinen Neustart erfordern, ist eine Pause von weniger als 10 Sekunden für den virtuellen Computer. Azure wählt den Updatemechanismus aus, der die geringsten Auswirkungen auf die VMs des Kunden hat. In bestimmten Fällen werden arbeitsspeicherschonende Wartungsmechanismen verwendet, bei denen die VM für bis zu 30 Sekunden angehalten und der Arbeitsspeicher im RAM beibehalten wird. Anschließend wird der Betrieb des virtuellen Computers fortgesetzt, und seine Uhr wird automatisch synchronisiert. Azure verwendet in zunehmendem Maß Technologien zur Livemigration und bessere Mechanismen zur Erhaltung des Arbeitsspeichers, um die Anhaltedauer zu verringern.  

Diese Wartungsvorgänge ohne Neustart werden einzeln für jede Fehlerdomäne angewendet, und das Fortschreiten wird beendet, wenn Warnsignale zur Integrität empfangen werden. 

Einige Anwendungen werden durch Updates dieser Art unter Umständen beeinträchtigt. Falls der virtuelle Computer live zu einem anderen Host migriert wird, ist einige Minuten vor dem Anhalten der VM bei einigen empfindlichen Workloads unter Umständen eine geringfügige Leistungsbeeinträchtigung feststellbar. Solche Anwendungen können von der Nutzung geplanter Ereignisse für [Windows](../articles/virtual-machines/windows/scheduled-events.md) oder [Linux](../articles/virtual-machines/linux/scheduled-events.md) in der Vorbereitung der VM-Wartung profitieren und können dann Auswirkungen während der Azure-Wartung vermeiden. Azure arbeitet außerdem an Funktionen zur Wartungssteuerung für solche besonders empfindlichen Anwendungen. 

## <a name="live-migration"></a>Livemigration

Die Livemigration ist ein Vorgang ohne Neustart, bei dem der Arbeitsspeicher für den virtuellen Computer beibehalten wird und der zu einer eingeschränkten Pause oder einer Unterbrechung der Reaktion führt – in der Regel nicht länger als 5 Sekunden. Derzeit sind alle IaaS-VMs (Infrastructure-as-a-Service) mit Ausnahme der Serien G, M, N und H für die Live-Migration geeignet. Dies entspricht mehr als 90 % der IaaS-VMs, die in Azure bereitgestellt werden. 

Die Livemigration wird in den folgenden Szenarien durch das Azure-Fabric initiiert:
- Geplante Wartung
- Hardwarefehler
- Zuordnungsoptimierungen

Die Livemigration wird in einigen Szenarien mit geplanter Wartung genutzt. Geplante Ereignisse können verwendet werden, damit im Voraus bekannt ist, wann Livemigrationsvorgänge gestartet werden.

Die Livemigration dient auch zum Entfernen von VMs von Hardware mit einem vorhergesagten bevorstehenden Ausfall, der von unseren Machine Learning-Algorithmen erkannt wurde, und zum Optimieren von VM-Zuordnungen. Weitere Informationen zu unserer Vorhersagemodellierung, die Instanzen von beeinträchtigter Hardware erkennt, finden Sie in unserem Blogbeitrag [Improving Azure Virtual Machine resiliency with predictive ML and live migration](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure) (Verbessern der Resilienz von virtuellen Azure-Computern mit maschinellem Lernen zur Vorhersage und Livemigration). Kunden erhalten immer eine Benachrichtigung über die Livemigration in ihrem Azure-Portal in den Überwachungs-/Dienstintegritätsprotokollen sowie über Scheduled Events, wenn dieser Dienst verwendet wird.

## <a name="maintenance-requiring-a-reboot"></a>Wartungsmaßnahmen, die einen Neustart erfordern

Im seltenen Fall, dass virtuelle Computer für eine geplante Wartung neu gestartet werden müssen, werden Sie darüber im Voraus informiert. Die geplante Wartung hat zwei Phasen: das Self-Service-Zeitfenster und ein Zeitfenster für die geplante Wartung.

Das **Self-Service-Zeitfenster** ermöglicht es Ihnen, die Wartung auf Ihren virtuellen Computern zu starten. Innerhalb dieses Zeitfensters (normalerweise vier Wochen) können Sie die einzelnen virtuellen Computer abfragen, um ihren Status zu ermitteln und die Ergebnisse Ihrer letzten Wartungsanforderung zu prüfen.

Wenn Sie die Self-Service-Wartung starten, wird Ihre VM auf einem bereits aktualisierten Knoten neu bereitgestellt. Aufgrund des Neustarts des virtuellen Computers geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert.

Sollte im Rahmen der Self-Service-Wartung ein Fehler auftreten, wird der Vorgang beendet, der virtuelle Computer wird nicht aktualisiert, und Sie erhalten die Möglichkeit, die Self-Service-Wartung erneut durchzuführen. 

Nach Ablauf des Self-Service-Zeitfensters beginnt das **Zeitfenster für die geplante Wartung**. Innerhalb dieses Zeitfensters können Sie zwar weiterhin das Wartungszeitfenster abfragen, die Wartung aber nicht mehr selbst starten.

Weitere Informationen zur Verwaltung von Wartungen mit erforderlichem Neustart finden Sie unter „Behandlung von Benachrichtigungen der geplanten Wartung“ für [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) oder [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Überlegungen zur Verfügbarkeit während einer geplanten Wartung 

Wenn Sie bis zum Zeitfenster für die geplante Wartung warten möchten, sollten Sie einige Dinge berücksichtigen, um eine möglichst hohe Verfügbarkeit Ihrer virtuellen Computer zu gewährleisten. 

#### <a name="paired-regions"></a>Regionspaare

Jeder Azure-Region ist innerhalb der gleichen geografischen Region eine andere Region als Regionspartner zugeordnet. In der geplanten Wartungsphase aktualisiert Azure nur die virtuellen Computer in einer einzelnen Region eines Regionspaars. Wenn z. B. die VMs in der Region „USA, Norden-Mitte“ aktualisiert werden, aktualisiert Azure nicht gleichzeitig die VMs in der Region „USA, Süden-Mitte“. Andere Regionen wie Nordeuropa können jedoch gleichzeitig mit USA (Ost) gewartet werden. Sie sollten mit der Funktionsweise von Regionspaaren vertraut sein, um Ihre virtuellen Computer besser auf Regionen verteilen zu können. Weitere Informationen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Verfügbarkeitsgruppen und Skalierungsgruppen

Wenn Sie eine Workload auf virtuellen Azure-Computern bereitstellen, können Sie die virtuellen Computer in einer Verfügbarkeitsgruppe erstellen, um die Hochverfügbarkeit der Anwendung zu erreichen. Dadurch wird sichergestellt, dass während eines Ausfalls oder Wartungsereignisses mit Neustartanforderung mindestens eine VM verfügbar ist.

Innerhalb einer Verfügbarkeitsgruppe werden einzelne virtuelle Computer auf bis zu 20 Updatedomänen (UDs) verteilt. Während einer geplanten Wartung wird zum jeweiligen Zeitpunkt immer nur eine Updatedomäne aktualisiert. Die Reihenfolge der aktualisierten Updatedomänen ist jedoch nicht unbedingt sequenziell. 

VM-Skalierungsgruppen sind eine Azure-Computerressource, mit der Sie eine Gruppe identischer virtueller Computer als Einzelressource bereitstellen und verwalten können. Die Skalierungsgruppe wird automatisch über Updatedomänen hinweg bereitgestellt – genau wie virtuelle Computer in einer Verfügbarkeitsgruppe. Und genau wie bei Verfügbarkeitsgruppen wird auch bei Skalierungsgruppen während einer geplanten Wartung immer nur jeweils eine Updatedomäne aktualisiert.

Weitere Informationen zum Konfigurieren Ihrer VMs für Hochverfügbarkeit finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](../articles/virtual-machines/windows/manage-availability.md) bzw. [Verwalten der Verfügbarkeit virtueller Linux-Computer](../articles/virtual-machines/linux/manage-availability.md).
