---
title: Verwenden Sie Data Box und andere Methoden für die Offlineerfassung in Azure File Sync.
description: Prozesse und bewährte Methoden, die die mit Azure File Sync konforme Massenmigration unterstützen.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.component: files
ms.openlocfilehash: a184e0563d1ad26671c38cabe07f42d97cbe2885
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212667"
---
# <a name="migrate-to-azure-file-sync"></a>Migrieren zur Azure-Dateisynchronisierung
Es gibt mehrere Möglichkeiten, zu Daten zu Azure File Sync zu verschieben:

### <a name="uploading-files-via-azure-file-sync"></a>Hochladen von Dateien über Azure File Sync
Die einfachste Möglichkeit ist, dass Sie Ihre Dateien lokal auf einen Windows Server 2012 R2 oder höher verschieben und den Azure File Sync-Agent installieren. Sobald die Synchronisierung konfiguriert ist, werden Ihre Dateien vom Server hochladen. Wir beobachten derzeit eine durchschnittliche Uploadgeschwindigkeit bei allen unseren Kunden von 1TB alle 2 Tage.
Erwägen Sie einen [Zeitplan für die Bandbreitenbeschränkung](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter), um sicherzustellen, dass Ihr Server ein „braver Bürger“ in Ihrem Rechenzentrum ist.

### <a name="offline-bulk-transfer"></a>Offlinemassenübertragung
Hochladen ist sicherlich die einfachste Möglichkeit, aber vielleicht funktioniert dies bei Ihnen nicht, wenn Ihre verfügbare Bandbreite das Synchronisieren Ihrer Dateien in Azure nicht in einem angemessenen Zeitraum erlaubt. Die erste Synchronisierung des gesamten Satzes von Dateien ist die Herausforderung, die hier überwunden werden muss. Azure File Sync verschiebt anschließend nur Änderungen, die im Namespace auftreten, was in der Regel viel weniger Bandbreite beansprucht.
Um diese anfängliche Uploadherausforderung zu umgehen, können Offline-Massenmigrationstools wie z.B. die [Azure Data Box-Familie](https://azure.microsoft.com/services/storage/databox) verwendet werden. Der folgende Artikel konzentriert sich auf den Prozess, den Sie einhalten müssen, um die Offlinemigration in einer mit Azure File Sync konformen Weise zu nutzen. Dort werden die bewährten Methoden beschrieben, mit denen Sie Dateikonflikte vermeiden und Ihre Datei- und Ordner-Zugriffssteuerungslisten sowie Zeitstempel beibehalten können, nachdem Sie die Synchronisierung aktiviert haben.

### <a name="online-migration-tools"></a>Onlinemigrationstools
Das unten beschriebene Verfahren funktioniert nicht nur für Data Box. Es funktioniert für alle Offlinemigrationstools (z.B. Data Box) oder Onlinetools wie AzCopy, Robocopy bzw. Tools und Dienste von Drittanbietern. Unabhängig von der Methode, die anfängliche Uploadherausforderung zu überwinden, müssen noch die folgenden Schritte ausgeführt werden, um diese Tools in einer mit Azure File Sync konformen Weise einzusetzen.


## <a name="offline-data-transfer-benefits"></a>Vorteile der Offlinedatenübertragung
Die Hauptvorteile der Offlinemigration, wenn Sie eine Data Box verwenden, sind folgende:

- Bei der Migration von Dateien zu Azure mittels eines Offline-Massenübertragungsprozesses, z.B. Data Box, müssen Sie nicht alle Dateien von Ihrem Server über das Netzwerk hochladen. Bei großen Namespaces könnte dies bedeutende Einsparungen von Netzwerkbandbreite und Zeit bedeuten.
- Wenn Sie Azure File Sync verwenden, lädt Ihr Liveserver unabhängig vom verwendeten Transportmodus (Data Box, Azure-Import usw.) nur die Dateien hoch, die geändert wurden, seit Sie die Daten zu Azure verschoben haben.
- Azure File Sync stellt sicher, dass Ihre Datei- und Ordner-ACLs ebenfalls synchronisiert werden, auch wenn das Offline-Massenmigrationsprodukt ACLs nicht überträgt.
- Bei Verwendung von Azure Data Box und Azure File Sync treten keine Ausfallzeiten auf. Bei der Übertragung von Daten zu Azure mit Data Box wird die Netzwerkbandbreite effizient genutzt und gleichzeitig die Dateigenauigkeit beibehalten. Auch Ihr Namespace wird auf dem neuesten Stand gehalten, indem nur die Dateien hochgeladen werden, die geändert wurden, seit die Data Box gesendet wurde.

## <a name="plan-your-offline-data-transfer"></a>Planen Ihrer Offlinedatenübertragung
Bevor Sie beginnen, beachten Sie Folgendes:

- Schließen Sie die Massenmigration zu einer oder mehreren Azure-Dateifreigaben ab, bevor Sie die Synchronisierung mit Azure File Sync aktivieren.
- Wenn Sie Data Box für die Massenmigration verwenden möchten: Machen Sie sich mit den [Voraussetzungen für die Bereitstellung von Data Box](../../databox/data-box-deploy-ordered.md#prerequisites) vertraut.
- Planen Ihrer endgültigen Azure File Sync-Topologie: [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- Wählen Sie Azure Storage-Konten aus, die die Dateifreigaben enthalten sollen, mit denen Sie die Synchronisierung durchführen möchten. Stellen Sie sicher, dass die Massenmigration zu temporären Stagingdateifreigaben im gleichen Speicherkonto stattfindet. Massenmigration kann nur mithilfe einer endgültigen und einer Stagingfreigabe aktiviert werden, die sich im gleichen Speicherkonto befinden.
- Eine Massenmigration kann nur beim Erstellen einer neuen Synchronisierungsbeziehung mit einem Server genutzt werden. Sie können keine Massenmigration mit einer vorhandenen Synchronisierungsbeziehung aktivieren.

## <a name="offline-data-transfer-process"></a>Offlinedatenübertragungs-Prozess
Dieser Abschnitt beschreibt den Prozess der Einrichtung von Azure File Sync in einer mit Massenmigrationstools wie Azure Data Box konformen Weise.

![Visualisieren von Prozessschritten, die auch ausführlich in einem folgenden Absatz erläutert werden](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Schritt | Detail |
|---|---------------------------------------------------------------------------------------|
| ![Prozessschritt 1](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Bestellen Sie Ihre Data Box-Datenträger.](../../databox/data-box-deploy-ordered.md) Es gibt [verschiedene Angebote innerhalb der Data Box-Familie](https://azure.microsoft.com/services/storage/databox/data) zur Anpassung an Ihre Anforderungen. Wenn Sie Ihre Data Box erhalten, befolgen Sie die zugehörige [Dokumentation zum Kopieren Ihrer Daten](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box). Stellen Sie sicher, dass die Daten auf diesem UNC-Pfad in die Data Box kopiert werden: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>`, wobei `ShareName` der Name der Stagingfreigabe ist. Senden Sie die Data Box zurück an Azure. |
| ![Prozessschritt 2](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Warten Sie, bis die Dateien in den Azure-Dateifreigaben angezeigt werden, die Sie als temporäre Stagingfreigaben festgelegt haben. **Aktivieren Sie keine Synchronisierung mit diesen Freigaben!** |
| ![Prozessschritt 3](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Erstellen Sie eine neue Freigabe, die für jede Dateifreigabe leer ist, die Data Box für Sie erstellt hat. Stellen Sie sicher, dass diese neue Freigabe sich im gleichen Speicherkonto wie die Data Box-Dateifreigabe befindet. [Erstellen einer Dateifreigabe in Azure Files](storage-how-to-create-file-share.md). |
| ![Prozessschritt 4](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Erstellen Sie eine Synchronisierungsgruppe](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) in einem Speichersynchronisierungsdienst, und verweisen Sie auf die leere Freigabe als Cloudendpunkt. Wiederholen Sie diesen Schritt für jede Data Box-Dateifreigabe. Lesen Sie die Anleitung [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md), und befolgen Sie die erforderlichen Schritte zum Einrichten von Azure File Sync. |
| ![Prozessschritt 5](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Fügen Sie Ihr Liveserververzeichnis als Serverendpunkt](storage-sync-files-deployment-guide.md#create-a-server-endpoint) hinzu. Geben Sie im Prozess an, dass Sie die Dateien bereits zu Azure verschoben haben, und verweisen Sie auf die Stagingfreigaben. Sie haben die Wahl, das Cloudtiering je nach Bedarf zu aktivieren oder deaktivieren. Beim Erstellen eines Serverendpunkts für Ihren Liveserver müssen Sie auf die Stagingfreigabe verweisen. Aktivieren Sie „Offlinedatenübertragung“ (Abbildung unten) auf dem neuen Serverendpunkt-Blatt, und verweisen Sie auf die Stagingfreigabe, die sich im gleichen Speicherkonto wie der Cloudendpunkt befinden muss. Die Liste der verfügbaren Freigaben wird nach dem Speicherkonto und noch nicht synchronisierten Freigaben gefiltert. |

![Visualisieren der Azure Portal-Benutzeroberfläche für die Aktivierung der Offlinedatenübertragung während der Erstellung eines neuen Serverendpunkts.](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Synchronisieren der Freigabe
Nachdem Sie Ihren Serverendpunkt erstellt haben, beginnt Azure File Sync. Für jede auf dem Server vorhandene Datei bestimmt Azure File Sync, ob diese Datei auch in der Stagingfreigabe vorhanden ist, wo Data Box die Dateien abgelegt hat, und wenn ja, kopiert Azure File Sync die Datei aus der Stagingfreigabe, anstatt sie vom Server hochzuladen. Wenn die Datei nicht in der Stagingfreigabe vorhanden oder eine neuere Version auf dem lokalen Server verfügbar ist, lädt Azure File Sync die Datei vom lokalen Server hoch.

> [!IMPORTANT]
> Sie können den Massenmigrationsmodus nur während der Erstellung eines Serverendpunkts aktivieren. Es gibt derzeit keine Möglichkeit, nach Einrichtung eines Serverendpunkts massenmigrierte Daten von einem bereits synchronisierenden Server in den Namespace zu integrieren.

## <a name="acls-and-timestamps-on-files-and-folders"></a>Zugriffssteuerungslisten und Zeitstempel auf Dateien und Ordnern
Azure File Sync stellt sicher, dass ACLs für Dateien und Ordner auch dann vom Liveserver synchronisiert werden, wenn das verwendete Massenmigrationstool ursprünglich keine ACLs transportiert hat. Dies bedeutet, dass es OK ist, wenn die Stagingfreigabe keine ACLs für Dateien und Ordner enthält. Wenn Sie das Offlinedatenmigrations-Feature während der Erstellung eines neuen Serverendpunkts aktivieren, werden ACLs zu diesem Zeitpunkt für alle Dateien auf dem Server synchronisiert. Dasselbe gilt für die Erstellungs- und Änderungszeitstempel.

## <a name="shape-of-the-namespace"></a>Form des Namespaces
Was bei aktivierter Synchronisierung auf dem Server ist, bestimmt die Form des Namespaces. Wenn nach Data Box-“Momentaufnahme“ und -Migration Dateien auf dem lokalen Server gelöscht werden, werden diese Dateien nicht in den synchronisierenden Livenamespace eingebunden. Sie befinden sich weiterhin in der Stagingfreigabe, werden aber nicht kopiert. Das ist das gewünschte Verhalten, da Azure File Sync den Namespace dem Liveserver entsprechend beibehält. Die Data Box-“Momentaufnahme“ ist nur eine Stagingbasis für das effiziente Kopieren von Dateien und nicht die Autorität für die Form des Livenamespaces.

## <a name="finishing-bulk-migration-and-clean-up"></a>Abschließen der Massenmigration und Bereinigung
Der folgende Screenshot zeigt das Blatt für die Serverendpunkt-Eigenschaften im Azure-Portal. Im Offlinedatenübertragungs-Abschnitt können Sie den Status des Prozesses beobachten. Es wird entweder „In Bearbeitung“ oder „Abgeschlossen“ angezeigt.

Nachdem der Server die erste Synchronisierung des gesamten Namespaces abgeschlossen hat, hat er die Nutzung der Stagingdateifreigabe für die mit der Data Box massenmigrierten Dateien beendet. Beachten Sie in den Serverendpunkt-Eigenschaften für die Offlinedatenübertragung, dass der Status sich in „Abgeschlossen“ ändert. Zu diesem Zeitpunkt können Sie die Stagingfreigabe bereinigen, um Kosten zu sparen:

1. Wählen Sie „Offlinedatenübertragung deaktivieren“ in den Serverendpunkt-Eigenschaften aus, wenn der Status „Abgeschlossen“ lautet.
2. Erwägen Sie, die Stagingfreigabe zu löschen, um Kosten zu sparen. Es ist unwahrscheinlich, dass die Stagingfreigabe Datei- und Ordner-ACLs enthält, und ist daher nur von begrenztem Nutzen. Erstellen Sie für „Zeitpunkt“-Sicherungszwecke stattdessen eine echte [Momentaufnahme der synchronisierenden Azure-Dateifreigabe](storage-snapshots-files.md). Sie können [Azure Backup für Momentaufnahmen aktivieren]( ../../backup/backup-azure-files.md), die nach einem Zeitplan erstellt werden.

![Visualisieren der Azure Portal-Benutzeroberfläche für Serverendpunkt-Eigenschaften, wo sich die Status- und Deaktivierungssteuerelemente für die Offlinedatenübertragung befinden.](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Sie sollten diesen Modus nur deaktivieren, wenn der Status „Abgeschlossen“ lautet, oder Sie wirklich aufgrund einer Fehlkonfiguration abbrechen möchten. Wenn Sie den Modus mitten in einer legitimen Bereitstellung deaktivieren, werden Dateien auch dann zum Server hochgeladen, wenn Ihre Stagingfreigabe noch verfügbar ist.

> [!IMPORTANT]
> Nach dem Deaktivieren der Offlinedatenübertragung besteht keine Möglichkeit, sie erneut zu aktivieren, wenn die Stagingfreigabe von der Massenmigration immer noch verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md)
