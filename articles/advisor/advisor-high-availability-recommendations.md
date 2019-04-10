---
title: Verbessern der Verfügbarkeit Ihrer Anwendung mit Azure Advisor | Microsoft-Dokumentation
description: Mithilfe von Azure Advisor können Sie die Hochverfügbarkeit Ihrer Azure-Bereitstellungen sicherstellen.
services: advisor
documentationcenter: NA
author: kasparks
ms.author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 42627649145b568b2b25411d182e5a36cdb025b0
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881187"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>Verbessern der Verfügbarkeit Ihrer Anwendung mit Azure Advisor

Der Azure Advisor hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Empfehlungen für Hochverfügbarkeit von Advisor erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Hochverfügbarkeit**.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für virtuelle Computer

Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Advisor identifiziert virtuelle Computer, die nicht zu einer Verfügbarkeitsgruppe gehören und empfiehlt, diese in eine Verfügbarkeitsgruppe zu verschieben. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können eine Verfügbarkeitsgruppe für den virtuellen Computer erstellen oder diesen einer vorhandenen Verfügbarkeitsgruppe hinzufügen.

> [!NOTE]
> Wenn Sie eine Verfügbarkeitsgruppe erstellen möchten, müssen Sie ihr mindestens einen weiteren virtuellen Computer hinzufügen. Wir empfehlen das Gruppieren von mindestens zwei virtuellen Computern in einer Verfügbarkeitsgruppe, um sicherzustellen, dass mindestens ein Computer bei einem Ausfall verfügbar ist.

## <a name="ensure-availability-set-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Verfügbarkeitsgruppen

Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Der Advisor ermittelt Verfügbarkeitsgruppen mit nur einem virtuellen Computer und empfiehlt, dieser weitere hinzuzufügen. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können einen virtuellen Computer erstellen oder der Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzufügen.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Verwenden von Managed Disks zur Erhöhung der Datenzuverlässigkeit

Virtuelle Computer, die sich in einer Verfügbarkeitsgruppe mit Datenträgern befinden, die entweder Speicherkonten oder Speicherskalierungseinheiten gemeinsam nutzen, sind nicht widerstandsfähig gegen Ausfälle einzelner Speicherskalierungseinheiten während eines Ausfalls. Advisor identifiziert diese Verfügbarkeitsgruppen und empfehlen die Migration zu Azure Managed Disks. Dadurch wird sichergestellt, dass die Datenträger der verschiedenen virtuellen Computer in der Verfügbarkeitsgruppe ausreichend isoliert sind, um einen Single Point of Failure zu vermeiden. 

## <a name="ensure-application-gateway-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Anwendungsgateways

Bei Einhaltung dieser Empfehlung wird die Geschäftskontinuität unternehmenskritischer Anwendungen sichergestellt, die von Anwendungsgateways unterstützt werden. Advisor identifiziert die Anwendungsgatewayinstanzen, die nicht für die Fehlertoleranz konfiguriert sind, und schlägt Aktionen vor, die Sie zur Problembehebung ausführen können. Der Advisor identifiziert mittelgroße oder große Einzelinstanz-Anwendungsgateways und empfiehlt, mindestens eine weitere Instanz hinzufügen. Identifiziert außerdem kleine Einzel- und Mehrinstanz-Anwendungsgateways und empfiehlt die Migration zu mittelgroßen oder großen SKUs. Der Advisor empfiehlt diese Aktionen, um sicherzustellen, dass Ihre Anwendungsgatewayinstanzen so konfiguriert sind, dass sie die aktuellen SLA-Anforderungen für diese Ressourcen erfüllen.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Schützen Ihrer VM-Daten vor zufälligem Löschen

Durch das Einrichten der Sicherung eines virtuellen Computers wird die Verfügbarkeit Ihrer geschäftskritischen Daten sichergestellt, und Ihre Daten werden vor versehentlichem Löschen oder vor Beschädigung geschützt. Der Advisor identifiziert virtuelle Computer, auf denen die Sicherung nicht aktiviert ist, und empfiehlt das Aktivieren der Sicherung. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Sicherstellen, dass Sie bei Bedarf Zugang zu Azure-Cloudexperten haben

Bei unternehmenskritischen Workloads ist es wichtig, im Bedarfsfall Zugang zu technischem Support zu haben. Der Advisor ermittelt potenziell unternehmenskritische Abonnements, deren Supportplan keinen technischen Support enthält, und empfiehlt ein Upgrade auf eine Option mit technischem Support.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Erstellen von Azure Service Health-Warnungen für Benachrichtigungen bei Azure-Dienstproblemen

Es wird empfohlen, Azure Service Health-Warnungen einzurichten, um über Azure-Dienstprobleme benachrichtigt zu werden. [Azure Service Health](https://azure.microsoft.com/features/service-health/) ist ein kostenloser Dienst, der personalisierte Anleitungen und Unterstützung bietet, wenn Sie von einem Azure-Dienstproblem betroffen sind. Advisor ermittelt Abonnements, für die keine Warnungen konfiguriert sind, und empfiehlt, solche zu erstellen.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Konfigurieren von Traffic Manager-Endpunkten für Resilienz

Traffic Manager-Profile mit mehr als einem Endpunkt haben eine höhere Verfügbarkeit, wenn ein bestimmter Endpunkt ausfällt. Das Platzieren von Endpunkten in verschiedenen Regionen verbessert die Dienstzuverlässigkeit noch mehr. Advisor ermittelt Traffic Manager-Profile mit nur einem Endpunkt und empfiehlt, mindestens einen weiteren Endpunkt in einer anderen Region hinzuzufügen.

Wenn sich alle Endpunkte in einem Traffic Manager-Profil, das für das Näherungsrouting konfiguriert ist, in derselben Region befinden, kann es bei Benutzern in anderen Regionen zu Verbindungsverzögerungen kommen. Das Hinzufügen oder Verschieben eines Endpunkts in eine andere Region verbessert die Gesamtleistung und bietet eine höhere Verfügbarkeit, wenn alle Endpunkte in einer Region ausfallen. Advisor identifiziert für das Näherungsrouting konfigurierte Traffic Manager-Profile, bei denen sich alle Endpunkte in derselben Region befinden. Advisor empfiehlt das Hinzufügen oder Verschieben eines Endpunkts in eine andere Azure-Region.

Wenn ein Traffic Manager-Profil für das geografische Routing konfiguriert ist, wird der Datenverkehr basierend auf definierten Regionen an die Endpunkte weitergeleitet. Wenn eine Region ausfällt, ist kein vordefiniertes Failover verfügbar. Durch das Konfigurieren eines Endpunkts mit der regionalen Gruppierung „Alle (Welt)“ wird das Verwerfen von Datenverkehr vermieden und die Dienstverfügbarkeit verbessert. Advisor identifiziert Traffic Manager-Profile, die für das geografische Routing konfiguriert sind und keinen Endpunkt haben, der mit der regionalen Gruppierung „Alle (Welt)“ konfiguriert ist. Advisor empfiehlt, die Konfiguration zu ändern, sodass ein Endpunkt mit „Alle (Welt)“ konfiguriert ist.

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-after-accidental-overwrite-or-deletion"></a>Speichern und Wiederherstellen von Daten nach versehentlichem Überschreiben oder Löschen durch vorläufiges Löschen in Ihrem Azure Storage-Konto

Aktivieren Sie [vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) in Ihrem Speicherkonto, sodass gelöschte Blobs in einen vorläufig gelöschten Zustand übergehen, anstatt endgültig gelöscht zu werden. Wenn Daten überschrieben werden, wird eine vorläufig gelöschte Momentaufnahme generiert, um den Zustand der überschriebenen Daten zu speichern. Durch vorläufiges Löschen können Sie Daten wiederherstellen, wenn diese versehentlich gelöscht oder überschrieben wurden. Advisor identifiziert Azure Storage-Konten, bei denen das vorläufige Löschen deaktiviert ist, und schlägt vor, es zu aktivieren.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>Konfigurieren des VPN-Gateway zu aktiv-aktiv für Verbindungsresilienz

In der Aktiv-Aktiv-Konfiguration richten beide Instanzen eines VPN-Gateways S2S-VPN-Tunnel zu Ihrem lokalen VPN-Gerät ein. Wenn ein geplantes Wartungsereignis oder ein ungeplantes Ereignis mit einer Gateway-Instanz eintritt, wird der Datenverkehr automatisch auf den anderen aktiven IPsec-Tunnel umgeschaltet. Azure Advisor identifiziert VPN-Gateways, die nicht als aktiv-aktiv konfiguriert sind, und schlägt vor, dass Sie sie für Hochverfügbarkeit konfigurieren.

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

