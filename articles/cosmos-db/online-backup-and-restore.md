---
title: Onlinesicherung und -wiederherstellung mit Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie mehr über das automatische Sichern und Wiederherstellen einer Azure Cosmos DB-Datenbank.
keywords: Sicherung und Wiederherstellung, Onlinesicherung
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 657b75e5e3bb5c35bb23221235e62298fc797046
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902670"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatische Onlinesicherung und -wiederherstellung mit Azure Cosmos DB
Azure Cosmos DB erstellt in regelmäßigen Abständen automatisch Sicherungen aller Daten. Die automatischen Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit des Betriebs Ihrer Datenbanken. Alle Sicherungskopien werden in einem anderen Speicherdienst getrennt gespeichert, und diese Sicherungen werden zum besseren Schutz vor regionalen Ausfällen global repliziert. Die automatischen Sicherungen sind für den Fall vorgesehen, dass Sie Ihren Cosmos DB-Container versehentlich löschen und später eine Datenwiederherstellungslösung benötigt wird.  

Dieser Artikel beginnt mit einer Kurzübersicht über die Redundanz und Verfügbarkeit von Daten in Cosmos DB, an die sich eine Erörterung von Sicherungen anschließt. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Hochverfügbarkeit mit Cosmos DB: Kurzübersicht
Cosmos DB ist auf eine [globale Verteilung](distribute-data-globally.md) ausgelegt und ermöglicht eine Skalierung des Durchsatzes in mehreren Azure-Regionen sowie ein durch Richtlinien gesteuertes Failover und transparente Multihosting-APIs. Azure Cosmos DB bietet [SLAs für eine Verfügbarkeit von 99,99 Prozent](https://azure.microsoft.com/support/legal/sla/cosmos-db) für alle Konten mit einer einzelnen Region und alle Konten mit mehreren Regionen mit gelockerter Konsistenz sowie eine Leseverfügbarkeit von 99,999 Prozent für alle Datenbankkonten mit mehreren Regionen. Alle Schreibvorgänge in Azure Cosmos DB werden mithilfe eines Quorums von Replikaten in einem lokalen Rechenzentrum beständig auf lokalen Datenträgern gespeichert, bevor der Client eine Bestätigung erhält. Die Hochverfügbarkeit von Cosmos DB ist von lokalem Speicher und nicht von externen Speichertechnologien abhängig. Wenn Ihr Konto darüber hinaus mehreren Azure-Regionen zugeordnet ist, werden Ihre Schreibvorgänge auch in andere Regionen repliziert. Um Ihren Durchsatz zu skalieren und mit kurzer Latenz auf Daten zuzugreifen, können Sie in Ihrem Datenbankkonto über so viele Leseregionen wie gewünscht verfügen. In jeder Leseregion werden die (replizierten) Daten dauerhaft in einer Replikatgruppe gespeichert.  

Wie im folgenden Diagramm dargestellt, wird ein einzelner Cosmos DB-Container [horizontal partitioniert](partition-data.md). Eine „Partition“ ist im folgenden Diagramm durch einen Kreis dargestellt, und jede Partition bietet mithilfe einer Replikatgruppe hohe Verfügbarkeit. Dies ist die lokale Verteilung innerhalb einer einzelnen Azure-Region (siehe die x-Achse). Zusätzlich wird jede Partition (samt zugehöriger Replikatgruppe) anschließend auf mehrere Regionen verteilt, die Ihrem Datenbankkonto zugeordnet sind (in dieser Abbildung z.B. auf die drei Regionen USA, Osten, USA, Westen und Indien, Mitte). Die „Partitionsgruppe“ ist eine global verteilte Entität, die aus mehreren Kopien Ihrer Daten in jeder Region besteht (siehe die Y-Achse). Sie können den Ihrem Datenbankkonto zugeordneten Regionen eine Priorität zuweisen. Bei einem Notfall führt Cosmos DB ein transparentes Failover in die nächste Region durch. Sie können ein Failover auch manuell simulieren, um zu testen, ob Ihre Anwendung lückenlos verfügbar ist.  

Die folgende Abbildung veranschaulicht das hohe Maß an Redundanz dank Cosmos DB.

![Hohes Maß an Redundanz mit Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Hohes Maß an Redundanz mit Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Vollständige, automatische Onlinesicherungen
Huch, ich habe leider meinen Container bzw. meine Datenbank gelöscht! Cosmos DB sorgt nicht nur dafür, dass Ihre Daten, sondern auch die Sicherungen Ihrer Daten überaus redundant und gegen regionale Katastrophen geschützt sind. Diese automatisierten Sicherungen werden derzeit ungefähr alle vier Stunden ausgeführt, und es werden immer die beiden neuesten Sicherungen gespeichert. Wenn die Daten aus Versehen gelöscht oder beschädigt werden, wenden Sie sich innerhalb von acht Stunden an den [Azure-Support](https://azure.microsoft.com/support/options/). 

Die Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit Ihrer Datenbanken. Cosmos DB erstellt die Sicherung im Hintergrund, ohne Ihre bereitgestellten Anforderungseinheiten zu beanspruchen bzw. die Leistung oder die Verfügbarkeit Ihrer Datenbank zu beeinträchtigen. 

Im Gegensatz zu Ihren Daten, die in Cosmos DB gespeichert sind, werden automatische Sicherungen im Azure Blob Storage-Dienst gespeichert. Um niedrige Latenz und ein effizientes Hochladen zu gewährleisten, wird die Momentaufnahme Ihrer Sicherung in eine Instanz von Azure Blob Storage in der Region hochgeladen, in der sich auch die aktuelle Schreibregion Ihres Cosmos DB-Datenbankkontos befindet. Zum besseren Schutz vor regionalen Katastrophen wird jede Momentaufnahme Ihrer Sicherungsdaten in Azure Blob Storage nochmals mithilfe von georedundantem Speicher (GRS) in eine andere Region repliziert. Das folgende Diagramm zeigt, dass der gesamte Cosmos DB-Container (mit allen drei primären Partitionen in USA, Westen in diesem Beispiel) in einem von Azure Blob Storage-Remotekonto in USA, Westen gesichert wird und anschließend mithilfe von GRS nach USA, Osten repliziert wird. 

Das folgende Bild veranschaulicht regelmäßige vollständige Sicherungen aller Cosmos DB-Entitäten in georedundantem Azure Storage.

![Regelmäßige vollständige Sicherungen aller Cosmos DB-Entitäten in georedundantem Azure Storage](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Aufbewahrungszeitraum der Sicherung
Wie oben beschrieben, erstellt Azure Cosmos DB alle vier Stunden Momentaufnahmen auf Partitionsebene. Es werden jeweils nur die letzten zwei Momentaufnahmen aufbewahrt. Aber wenn der Container bzw. die Datenbank gelöscht wird, bewahrt Azure Cosmos DB die vorhandenen Momentaufnahmen für alle gelöschten Partitionen innerhalb des angegebenen Containers bzw. der Datenbank 30 Tage lang auf.

Wenn Sie für die SQL-API eigene Momentaufnahmen verwalten möchten, können Sie dazu die folgenden Optionen verwenden:

* Verwenden Sie die Option für den Export in eine JSON-Datei im [Datenmigrationstool](import-data.md#export-to-json-file) von Azure Cosmos DB, um zusätzliche Sicherungen zu planen.

* Verwenden Sie [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) zum Verschieben von Daten in regelmäßigen Abständen.

* Verwenden Sie den [Änderungsfeed](change-feed.md) von Azure Cosmos DB, um Daten periodisch für eine vollständige Sicherung und separat für inkrementelle Änderungen zu lesen und in Ihr Blobziel zu verschieben. 

* Für die Verwaltung von dynamischen Sicherungen ist es möglich, Daten in regelmäßigen Abständen aus dem Änderungsfeed zu lesen und das Schreiben dieser Daten in eine andere Sammlung zu verzögern. So wird sichergestellt, dass Sie die Daten nicht wiederherstellen müssen, und Sie können sich die Daten sofort ansehen. 

> [!NOTE]
> Beachten Sie bei der „Bereitstellung des Durchsatzes für mehrere Container auf Datenbankebene“, dass die Wiederherstellung für das gesamte Datenbankkonto erfolgt. Darüber hinaus müssen Sie sich unbedingt innerhalb von acht Stunden an das Supportteam wenden, wenn Sie Ihren Container versehentlich löschen. Daten können nicht wiederhergestellt werden, wenn Sie sich nicht innerhalb von acht Stunden an das Supportteam wenden.

## <a name="restoring-a-database-from-an-online-backup"></a>Wiederherstellen einer Datenbank von einer Onlinesicherung

Falls Sie Ihre Datenbank oder Ihren Container versehentlich löschen, können Sie ein [Supportticket erstellen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oder sich [an den Azure Support wenden](https://azure.microsoft.com/support/options/), um die Daten aus der letzten automatischen Sicherung wiederherstellen zu lassen. Der Azure-Support steht nur für ausgewählte Tarife wie Standard und Developer zur Verfügung. Für den Basic-Tarif ist kein Support verfügbar. Weitere Informationen zu anderen Supportplänen finden Sie auf der Seite [Azure-Supportpläne](https://azure.microsoft.com/support/plans/). 

Wenn Sie die Datenbank aufgrund einer Datenbeschädigung wiederherstellen müssen (einschließlich Fälle, bei denen Dokumente innerhalb eines Containers gelöscht werden), helfen Ihnen die Informationen unter [Umgang mit Datenbeschädigung](#handling-data-corruption) weiter. Sie müssen zusätzliche Schritte ausführen, um zu verhindern, dass die vorhandenen Sicherungen durch die beschädigten Daten überschrieben werden. Für die Wiederherstellung einer bestimmten Momentaufnahme Ihrer Sicherung setzt Cosmos DB voraus, dass die Daten für die Dauer des Sicherungszyklus dieser Momentaufnahme verfügbar waren.

> [!NOTE]
> Sammlungen oder Datenbanken können nur bei expliziten Kundenanfragen wiederhergestellt werden. Es liegt in der Verantwortung des Kunden, den Container oder die Datenbank unmittelbar nach dem Abgleich der Daten zu löschen. Wenn Sie die wiederhergestellten Datenbanken oder Sammlungen nicht löschen, entstehen Kosten für Anforderungseinheiten, Speicherung und Erfassung.

## <a name="handling-data-corruption"></a>Umgang mit Datenbeschädigung

Azure Cosmos DB bewahrt die letzten beiden Sicherungen jeder Partition im Datenbankkonto auf. Dieses Modell funktioniert gut, wenn Sie einen Container (Sammlung von Dokumenten, Diagramm, Tabelle) oder eine Datenbank versehentlich gelöscht haben, da eine der letzten Versionen wiederhergestellt werden kann. Wenn jedoch eine Datenbeschädigung auftritt, ist Azure Cosmos DB möglicherweise nicht über die Datenbeschädigung informiert, und diese könnte in die vorhandenen Sicherungen überschrieben worden sein. 

Sobald Sie eine Beschädigung feststellen, sollten Benutzer den beschädigten Container (Sammlung/Diagramm/Tabelle) löschen, sodass Sicherungen vor dem Überschreiben mit beschädigten Daten geschützt sind. Und am wichtigsten ist, sich an den Microsoft-Support zu wenden und ein Ticket mit einer spezifischen Anfrage mit dem Schweregrad 2 zu erstellen. 

In der folgenden Abbildung ist die Erstellung der Supportanfrage für die Containerwiederherstellung (Sammlung/Graph/Tabelle) über das Azure-Portal dargestellt, falls Daten in einem Container versehentlich gelöscht oder aktualisiert wurden.

![Wiederherstellen eines Containers nach dem fehlerhaften Aktualisieren oder Löschen von Daten in Cosmos DB](./media/online-backup-and-restore/backup-restore-support.png)

Wenn die Wiederherstellung für diese Art von Szenarien durchgeführt wird, werden Daten in einem anderen Konto (mit dem Suffix „-restored“) und einem anderen Container wiederhergestellt. Dies ist keine direkte Wiederherstellung, damit Kunden die Möglichkeit haben, die Daten zu überprüfen und wie gewünscht zu verschieben. Der wiederhergestellte Container befindet sich in derselben Region und verfügt über dieselben RUs und Indizierungsrichtlinien. Ein Benutzer, der Abonnementadministrator oder Co-Administrator ist, kann das wiederhergestellte Konto anzeigen.


> [!NOTE]
> Wenn Sie die Daten zur Behebung von Beschädigungsproblemen oder nur zum Testen wiederherstellen, planen Sie unbedingt, sie zu entfernen, sobald Ihre Aufgabe erledigt ist, da wiederhergestellte Container oder eine Datenbank zusätzliche Kosten basierend auf dem bereitgestellten Durchsatz verursachen. 
## <a name="next-steps"></a>Nächste Schritte

Wie Sie die Datenbank in mehrere Rechenzentren replizieren, erfahren Sie unter [Globale Verteilung von Daten mit Cosmos DB](distribute-data-globally.md). 

Zum Kontaktieren des Azure-Supports [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

