---
title: Erstellen von hochverfügbaren Azure Storage-Anwendungen für zonenredundante Speicher (ZRS) | Microsoft-Dokumentation
description: Zonenredundante Speicher (ZRS) bieten eine einfache Möglichkeit zum Erstellen von hochverfügbaren Anwendungen. ZRS schützen vor Hardwareausfällen im Rechenzentrum und vor regionalen Notfällen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/24/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 1b39de45d5046ce5a59dcaf0648b87aca2a5c6f5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52868343"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Supportabdeckung und regionale Verfügbarkeit
ZRS unterstützt derzeit Standardkonten vom Typ „Allgemein v2“. Weitere Informationen zu Arten von Speicherkontotypen finden Sie unter [Übersicht über Azure Storage-Konten](storage-account-overview.md).

ZRS ist für Blockblobs, Seitenblobs ohne Datenträger, Dateien, Tabellen und Warteschlangen verfügbar

ZRS ist in den folgenden Regionen verfügbar:

- USA, Osten
- USA (Ost 2)
- USA, Westen 2
- USA, Mitte
- Nordeuropa
- Europa, Westen
- Frankreich, Mitte
- Asien, Südosten

Microsoft arbeitet daran, ZRS in weiteren Azure-Regionen zu aktivieren. Sehen Sie regelmäßig auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/) nach, um Informationen zu neuen Regionen zu erhalten.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Was geschieht, wenn eine Zone nicht mehr verfügbar ist?
Auf Ihre Daten kann weiterhin zugegriffen werden, auch wenn eine Zone nicht mehr verfügbar ist. Microsoft empfiehlt, Vorgehensweisen für die Behandlung vorübergehender Fehler anzuwenden. Zu diesen Vorgehensweisen gehört die Implementierung von Wiederholungsrichtlinien mit exponentiellem Backoff.

Wenn eine Zone nicht verfügbar ist, führt Azure Netzwerkupdates durch, z.B. durch die Festlegung neuer DNS-Ziele. Diese Updates können sich auf Ihre Anwendung auswirken, wenn Sie auf Ihre Daten zugreifen, bevor die Updates abgeschlossen sind.

ZRS kann Ihre Daten nicht vor einem regionalen Notfall schützen, bei dem mehrere Zonen dauerhaft betroffen sind. ZRS bietet stattdessen Resilienz für Ihre Daten bei einem temporären Ausfall. Für den Schutz vor regionalen Ausfällen empfiehlt Microsoft die Verwendung von georedundantem Speicher (GRS). Weitere Informationen zum georedundanten Speicher finden Sie unter [Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Wechseln zur ZRS-Replikation
Die Migration von oder zu LRS, GRS und RA-GRS ist ein einfacher Prozess. Verwenden Sie das Azure-Portal oder die Speicherressourcenanbieter-API, um den Redundanztyp Ihres Kontos zu ändern. Azure repliziert Ihre Daten dann entsprechend. 

Für die Migration von Daten von oder zu ZRS ist eine andere Strategie erforderlich. Die ZRS-Migration umfasst die physische Verlagerung von Daten von einem einzelnen Speicherstempel auf mehrere Stempel in einer Region.

Es gibt zwei primäre Optionen für die Migration zu oder von ZRS: 

- Kopieren oder verschieben Sie Daten manuell aus einem vorhandenen Konto in ein neues ZRS-Konto.
- Fordern Sie eine Livemigration an.

Microsoft empfiehlt dringend die Ausführung einer manuellen Migration. Eine manuelle Migration bietet mehr Flexibilität als eine Livemigration. Bei einer manuellen Migration können Sie den zeitlichen Ablauf steuern.

Für eine manuelle Migration stehen Ihnen zwei Optionen zur Verfügung:
- Verwenden Sie vorhandene Tools wie AzCopy, eine der Azure Storage-Clientbibliotheken oder zuverlässige Drittanbietertools.
- Wenn Sie mit Hadoop oder HDInsight vertraut sind, fügen Sie sowohl das Quell- als auch das Zielkonto (ZRS) an Ihren Cluster an. Danach parallelisieren Sie den Datenkopiervorgang mit einem Tool wie z.B. DistCp.
- Verwenden Sie eine der Azure Storage-Clientbibliotheken, um eigene Tools zu erstellen.

Eine manuelle Migration kann zu Ausfallzeiten der Anwendung führen. Wenn Ihre Anwendung eine hohe Verfügbarkeit erfordert, bietet Microsoft auch eine Option für eine Livemigration. Eine Livemigration ist eine direkte Migration. 

Während einer Livemigration können Sie Ihr Speicherkonto weiter verwenden, während Ihre Daten zwischen dem Quell- und dem Zielspeicher migriert werden. Während des Migrationsprozesses verfügen Sie über das gleiche Maß an Dauerhaftigkeit und Verfügbarkeit gemäß SLA, wie es normalerweise der Fall ist.

Beachten Sie die folgenden Einschränkungen bei Livemigrationen:

- Auch wenn Microsoft auf Ihre Anforderung zur Livemigration sofort reagiert, gibt es keine Garantie, zu welchem Zeitpunkt eine Livemigration abgeschlossen sein wird. Wenn Ihre Daten bis zu einem bestimmten Datum zu ZRS migriert werden müssen, empfiehlt Microsoft die Durchführung einer manuellen Migration. Grundsätzlich gilt: Je mehr Daten in Ihrem Konto vorhanden sind, desto länger dauert die Migration dieser Daten. 
- Die Livemigration wird nur für Speicherkonten unterstützt, die LRS- oder GRS-Replikation nutzen. Wenn Ihr Konto RA-GRS verwendet, müssen Sie zuerst den Replikationstyp Ihres Kontos auf LRS oder GRS ändern, bevor Sie fortfahren. Dieser Zwischenschritt entfernt vor der Migration den sekundären schreibgeschützten Endpunkt, der von RA-GRS bereitgestellt wird.
- Ihr Konto muss Daten enthalten.
- Sie können Daten nur innerhalb der gleichen Region migrieren. Wenn Sie Ihre Daten in ein ZRS-Konto zu einer anderen Region als das Quellkonto migrieren möchten, müssen Sie eine manuelle Migration ausführen.
- Nur Standard-Speicherkontotypen unterstützen die Livemigration. Premium-Speicherkonten müssen manuelle migriert werden.

Sie können eine Livemigration über das [Azure-Support-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) anfordern. Wählen Sie im Portal das Speicherkonto aus, das Sie in ZRS konvertieren möchten.
1. Wählen Sie **Neue Supportanfrage** aus.
2. Geben Sie die **Grundlagen** basierend auf den Informationen Ihres Kontos an. Wählen Sie im Abschnitt **Dienst** den Eintrag **Speicherkontoverwaltung** und die Ressource aus, die Sie in ZRS konvertieren möchten. 
3. Klicken Sie auf **Weiter**. 
4. Geben Sie im Abschnitt **Problem** die folgenden Werte an: 
    - **Schweregrad**: Behalten Sie den Standardwert bei.
    - **Problemtyp**: Wählen Sie **Datenmigration** aus.
    - **Kategorie**: Wählen Sie **Innerhalb einer Region zu ZRS migrieren** aus.
    - **Titel**: Geben Sie einen aussagekräftigen Titel ein, z.B. **ZRS-Kontomigration**.
    - **Details**: Geben Sie im Feld **Details** weitere Informationen ein, z.B. „Ich möchte von [LRS, GRS] in der Region „\_\_“ zu ZRS migrieren“. 
5. Klicken Sie auf **Weiter**.
6. Überprüfen Sie, ob die Kontaktinformationen auf dem Blatt **Kontaktinformationen** korrekt sind.
7. Klicken Sie auf **Erstellen**.

Ein Supportmitarbeiter wird sich mit Ihnen in Verbindung setzen und Sie nach Bedarf und Wunsch unterstützen. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS (klassisch): Eine ältere Option für die Redundanz von Blockblobs
> [!NOTE]
> Microsoft wird ZRS Classic-Konten am 31. März 2021 als veraltet markieren und migrieren. Bevor die Option als veraltet gekennzeichnet wird, erhalten ZRS Classic-Kunden weitere Informationen. 
>
> Sobald der zonenredundante Speicher in einer Region [allgemein verfügbar](#support-coverage-and-regional-availability) wird, können Kunden im Portal in dieser Region kein ZRS Classic-Konto mehr erstellen. Die Verwendung von Microsoft PowerShell und der Azure CLI zum Erstellen von ZRS Classic-Konten ist eine Option, bis ZRS Classic als veraltet gekennzeichnet wird.

ZRS Classic repliziert Daten asynchron zwischen Rechenzentren in einer oder zwei Regionen. Replizierte Daten sind möglicherweise erst wieder verfügbar, wenn Microsoft das Failover auf die sekundäre Region initiiert. Ein ZRS Classic-Konto kann nicht zu oder aus LRS, GRS oder RA-GRS konvertiert werden. ZRS Classic-Konten unterstützen zudem weder Metriken noch Protokollierungen.

ZRS Classic ist nur für **Blockblobs** in Speicherkonten vom Typ „Allgemein v1 (GPv1)“ verfügbar. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](storage-account-overview.md).

Um ZRS-Kontodaten manuell zu oder von einem LRS-, ZRS (klassisch)-, GRS- oder RA-GRS-Konto zu migrieren, verwenden Sie eins der folgenden Tools: AzCopy, Azure Storage-Explorer, Azure PowerShell oder Azure-Befehlszeilenschnittstelle. Sie können auch Ihre eigene Migrationslösung mit einer der Azure Storage-Clientbibliotheken erstellen.

## <a name="see-also"></a>Weitere Informationen
- [Azure Storage-Replikation](storage-redundancy.md)
- [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](storage-redundancy-lrs.md)
- [Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md)
