---
title: Azure Advisor-Empfehlungen für Hochverfügbarkeit | Microsoft-Dokumentation
description: Mithilfe von Azure Advisor können Sie die Hochverfügbarkeit Ihrer Azure-Bereitstellungen sicherstellen.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 61e85861ab5829620699d07fe24b1ebfdfc7cbdc
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839507"
---
# <a name="advisor-high-availability-recommendations"></a>Advisor-Empfehlungen für Hochverfügbarkeit

Der Azure Advisor hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Empfehlungen für Hochverfügbarkeit von Advisor erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Hochverfügbarkeit**.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für virtuelle Computer

Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Advisor identifiziert virtuelle Computer, die nicht zu einer Verfügbarkeitsgruppe gehören und empfiehlt, diese in eine Verfügbarkeitsgruppe zu verschieben. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können eine Verfügbarkeitsgruppe für den virtuellen Computer erstellen oder diesen einer vorhandenen Verfügbarkeitsgruppe hinzufügen.

> [!NOTE]
> Wenn Sie eine Verfügbarkeitsgruppe erstellen möchten, müssen Sie ihr mindestens einen weiteren virtuellen Computer hinzufügen. Wir empfehlen das Gruppieren von mindestens zwei virtuellen Computern in einer Verfügbarkeitsgruppe, um sicherzustellen, dass mindestens ein Computer bei einem Ausfall verfügbar ist.

## <a name="ensure-availability-set-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Verfügbarkeitsgruppen 

Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Der Advisor ermittelt Verfügbarkeitsgruppen mit nur einem virtuellen Computer und empfiehlt, dieser weitere hinzuzufügen. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können einen virtuellen Computer erstellen oder der Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzufügen.  

## <a name="ensure-application-gateway-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Anwendungsgateways
Um die Geschäftskontinuität unternehmenswichtiger, durch Anwendungsgateways unterstützter Anwendungen zu gewährleisten, identifiziert der Advisor Anwendungsgatewayinstanzen, die nicht für die Fehlertoleranz konfiguriert sind, und schlägt Ihnen Wartungsaktionen vor. Der Advisor identifiziert mittelgroße oder große Einzelinstanz-Anwendungsgateways und empfiehlt, mindestens eine weitere Instanz hinzufügen. Identifiziert außerdem kleine Einzel- und Mehrinstanz-Anwendungsgateways und empfiehlt die Migration zu mittelgroßen oder großen SKUs. Der Advisor empfiehlt diese Aktionen, um sicherzustellen, dass Ihre Anwendungsgatewayinstanzen so konfiguriert sind, dass sie die aktuellen SLA-Anforderungen für diese Ressourcen erfüllen.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Verbessern der Leistung und Zuverlässigkeit von Datenträgern virtueller Computer

Der Advisor ermittelt virtuelle Computer mit Standard-Datenträgern und empfiehlt ein Upgrade auf Premium-Datenträger.
 
Azure Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Datenträger von virtuellen Computern, die Storage Premium-Konten nutzen, speichern Daten auf Solid State Drives (SSD). Für eine optimale Leistung Ihrer Anwendung empfehlen wir, alle Datenträger von virtuellen Computern, die eine hohe IOPS-Leistung erfordern, zu Storage Premium zu migrieren. 

Wenn Ihre Datenträger keinen hohen IOPS-Wert erfordern, können Sie die Kosten begrenzen, indem Sie sie mit Storage Standard betreiben. Bei Storage Standard werden VM-Datenträgerdaten auf Festplattenlaufwerken (HDD) statt auf Solid State Drives (SSD) gespeichert. Sie können Ihre VM-Datenträger zu Premium-Datenträgern migrieren. Premium-Datenträger werden für die meisten VM-SKUs unterstützt. In einigen Fällen müssen Sie, wenn Sie Premium-Datenträger nutzen möchten, auch Ihre VM-SKUs aktualisieren.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Schützen Ihrer VM-Daten vor zufälligem Löschen

Durch das Einrichten der Sicherung eines virtuellen Computers wird die Verfügbarkeit Ihrer geschäftskritischen Daten sichergestellt, und Ihre Daten werden vor versehentlichem Löschen oder vor Beschädigung geschützt.  Der Advisor identifiziert virtuelle Computer, auf denen die Sicherung nicht aktiviert ist, und empfiehlt das Aktivieren der Sicherung. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Sicherstellen, dass Sie bei Bedarf Zugang zu Azure-Cloudexperten haben

Bei unternehmenskritischen Workloads ist es wichtig, im Bedarfsfall Zugang zu technischem Support zu haben. Der Advisor ermittelt potenziell unternehmenskritische Abonnements, deren Supportplan keinen technischen Support enthält, und empfiehlt ein Upgrade auf eine Option mit technischem Support.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Erstellen von Azure Service Health-Warnungen für Benachrichtigungen bei Azure-Dienstproblemen

Es wird empfohlen, Azure Service Health-Warnungen einzurichten, um über Azure-Dienstprobleme benachrichtigt zu werden. [Azure Service Health](https://azure.microsoft.com/features/service-health/) ist ein kostenloser Dienst, der personalisierte Anleitungen und Unterstützung bietet, wenn Sie von einem Azure-Dienstproblem betroffen sind. Advisor ermittelt Abonnements, für die keine Warnungen konfiguriert sind, und empfiehlt, solche zu erstellen.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Konfigurieren von Traffic Manager-Endpunkten für Resilienz

Traffic Manager-Profile mit mehr als einem Endpunkt haben eine höhere Verfügbarkeit, wenn ein bestimmter Endpunkt ausfällt. Das Platzieren von Endpunkten in verschiedenen Regionen verbessert die Dienstzuverlässigkeit noch mehr. Advisor ermittelt Traffic Manager-Profile mit nur einem Endpunkt und empfiehlt, mindestens einen weiteren Endpunkt in einer anderen Region hinzuzufügen.

Wenn sich alle Endpunkte in einem Traffic Manager-Profil, das für das Näherungsrouting konfiguriert ist, in derselben Region befinden, kann es bei Benutzern in anderen Regionen zu Verbindungsverzögerungen kommen. Das Hinzufügen oder Verschieben eines Endpunkts in eine andere Region verbessert die Gesamtleistung und bietet eine höhere Verfügbarkeit, wenn alle Endpunkte in einer Region ausfallen. Advisor ermittelt Traffic Manager-Profile, die für das Näherungsrouting konfiguriert sind, bei denen sich alle Endpunkte in derselben Region befinden, und empfiehlt das Hinzufügen oder Verschieben eines Endpunkts in eine andere Azure-Region.

Wenn ein Traffic Manager-Profil für das geografische Routing konfiguriert ist, wird der Datenverkehr basierend auf definierten Regionen an die Endpunkte weitergeleitet. Wenn eine Region ausfällt, ist kein vordefiniertes Failover verfügbar. Durch das Konfigurieren eines Endpunkts mit der regionalen Gruppierung „Alle (Welt)“ wird das Verwerfen von Datenverkehr vermieden und die Dienstverfügbarkeit verbessert. Advisor ermittelt Traffic Manager-Profile, die für das geografische Routing konfiguriert sind und keinen Endpunkt haben, der mit der regionalen Gruppierung „Alle (Welt)“ konfiguriert ist, und empfiehlt, diese Konfiguration zu ändern.

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-in-the-event-of-accidental-overwrite-or-deletion"></a>Speichern und Wiederherstellen von Daten bei versehentlichem Überschreiben oder Löschen durch vorläufiges Löschen in Ihrem Azure Storage-Konto

Aktivieren Sie [vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) in Ihrem Speicherkonto, sodass gelöschte Blobs in einen vorläufig gelöschten Zustand übergehen, anstatt endgültig gelöscht zu werden. Wenn Daten überschrieben werden, wird eine vorläufig gelöschte Momentaufnahme generiert, um den Zustand der überschriebenen Daten zu speichern. So können Sie Daten nach einer versehentlichen Löschung oder Überschreibung wiederherstellen. Der Advisor identifiziert Azure Storage-Konten, bei denen das vorläufige Löschen deaktiviert ist und schlägt vor, es zu aktivieren.

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Zugreifen auf Advisor-Empfehlungen zu Hochverfügbarkeit

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Hochverfügbarkeit**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie unter:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)

