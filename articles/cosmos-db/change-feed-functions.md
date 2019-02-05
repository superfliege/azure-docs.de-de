---
title: Verwenden des Azure Cosmos DB-Änderungsfeeds in Azure Functions
description: Verwenden des Azure Cosmos DB-Änderungsfeeds in Azure Functions
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 93cd93b40c142d504c52f08f9005d082fb5a2a20
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469480"
---
# <a name="trigger-azure-functions-from-azure-cosmos-db"></a>Auslösen von Azure Functions aus Azure Cosmos DB

Wenn Sie Azure Functions verwenden, ist die einfachste Möglichkeit zum Herstellen einer Verbindung mit einem Änderungsfeed das Hinzufügen eines [Azure Cosmos DB-Triggers](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger) zu Ihrer Azure Functions-App. Wenn Sie einen Cosmos DB-Trigger in einer Azure Functions-App erstellen, wählen Sie den Cosmos-Container aus, mit dem eine Verbindung hergestellt werden soll. Anschließend wird die Funktion immer dann ausgelöst, wenn eine Änderung im Container erfolgt.

Trigger können im Azure Functions-Portal, im Azure Cosmos DB-Portal oder programmgesteuert erstellt werden. Weitere Informationen finden Sie unter [Serverloses Datenbankcomputing mit Azure Cosmos DB und Azure Functions](serverless-computing-database.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region"></a>Wie kann ich Azure Functions für das Lesen aus einer bestimmten Region konfigurieren?

Es ist möglich, [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations) zu definieren, wenn Sie den Azure Cosmos DB-Trigger zur Angabe einer Liste mit Regionen verwenden. Ebenso können Sie die ConnectionPolicy anpassen, damit der Trigger die bevorzugten Regionen ausliest. Im Idealfall möchten Sie den Lesevorgang in der nächstgelegenen Region durchführen, in der Ihre Azure Functions-Instanz bereitgestellt wird.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Was ist die Standardgröße von Batches in Azure Functions?

Die Standardgröße beträgt 100 Elemente für jeden Aufruf von Azure Functions. Diese Zahl kann jedoch in der JSON-Datei der Funktion konfiguriert werden. Hier finden Sie die vollständige [Liste der Konfigurationsoptionen](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Bei der lokalen Entwicklung aktualisieren Sie die Anwendungseinstellungen in der Datei „local.settings.json“.

### <a name="i-am-monitoring-a-container-and-reading-its-change-feed-however-i-dont-get-all-the-inserted-documents-some-items-are-missing"></a>Ich überwache einen Container und lese den entsprechenden Änderungsfeed, aber ich erhalte nicht alle eingefügten Dokumente, einige Dokumente fehlen.

Vergewissern sie sich, dass keine andere Azure-Funktion denselben Container mit demselben Leasecontainer liest. Die fehlenden Dokumente werden von den anderen Azure-Funktionen verarbeitet, die die gleiche Lease verwenden.

Wenn Sie mehrere Azure-Funktionen zum Lesen desselben Änderungsfeeds erstellen, müssen diese verschiedene Leasecontainer oder die leasePrefix-Konfiguration verwenden, wenn sie denselben Container verwenden sollen. Beim Einsatz der Änderungsfeed-Verarbeitungsbibliothek können Sie allerdings mehrere Instanzen Ihrer Azure-Funktion starten, und das SDK übernimmt automatisch die Aufteilung der Dokumente auf die verschiedenen Instanzen.

### <a name="azure-cosmos-item-is-updated-every-second-and-i-dont-get-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Das Azure Cosmos-Element wird jede Sekunde aktualisiert, ich erhalte aber beim Lauschen am Änderungsfeed nicht alle Änderungen in Azure Functions.

Azure Functions fragt den Änderungsfeed mit einer maximalen Standardverzögerung von 5 Sekunden fortlaufend auf Änderungen ab. Wenn keine ausstehenden Änderungen zum Lesen vorliegen oder wenn Änderungen ausstehen, nachdem der Trigger angewendet wurde, werden diese sofort von der Funktion gelesen. Wenn jedoch keine Änderungen ausstehen, wartet die Funktion 5 Sekunden und prüft dann auf weitere Änderungen.

Wenn das Dokument mehrere Änderungen in demselben Intervall erhält, das der Trigger zum Abfragen neuer Änderungen benötigt, erhalten Sie möglicherweise die neueste Version des Dokuments, aber nicht die Zwischenversion.

Wenn Sie den Änderungsfeed in kürzeren Abständen als 5 Sekunden (beispielsweise jede Sekunde) abfragen möchten, können Sie das Abfrageintervall „FeedPollDelay“ konfigurieren. Informationen zur gesamten Konfiguration finden Sie [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations). Sie ist in Millisekunden mit dem Standardwert 5000 definiert. Abfragen in kürzeren Abständen als 1 Sekunde sind möglich, werden aber nicht empfohlen, weil mehr CPU-Arbeitsspeicher beansprucht wird.

### <a name="can-multiple-azure-functions-read-one-containers-change-feed"></a>Können mehrere Azure-Funktionen den Änderungsfeed eines Containers lesen?

Ja. Mehrere Azure-Funktionen können den Änderungsfeed desselben Containers lesen. Allerdings muss für die Azure-Funktionen ein separates „leaseCollectionPrefix“ definiert sein.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-in-a-batch-of-10-documents-and-i-get-an-error-at-seventh-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-document-ie-seventh-document-in-my-next-feed"></a>Angenommen, ich verarbeite einen Änderungsfeed mithilfe von Azure Functions, z.B. einen Stapel von zehn Dokumenten, und beim siebten Dokument wird ein Fehler ausgegeben. In diesem Fall werden die letzten drei Dokumente nicht verarbeitet. Wie kann ich in meinem nächsten Feed mit der Verarbeitung ab dem Dokument mit dem Fehler (d.h. dem siebten Dokument) beginnen?

Zum Behandeln des Fehlers empfiehlt sich folgendes Muster: Schließen Sie den Code in einen try-catch-Block ein. Wenn Sie die Liste der Dokumente durchlaufen, umschließen Sie die einzelnen Iterationen jeweils in einem eigenen try-catch-Block. Fangen Sie den Fehler ab, und stellen Sie das Dokument in eine Warteschlange (unzustellbare Nachrichten) ein. Definieren Sie anschließend Logik zum Verarbeiten der Dokumente, die den Fehler verursacht haben. Mit dieser Methode brauchen Sie bei einem Stapel aus 200 Dokumenten, von denen nur bei einem Dokument ein Fehler aufgetreten ist, nicht den gesamten Stapel wegzuwerfen.

Im Fehlerfall sollten Sie den Prüfpunkt nicht auf den Anfang zurücksetzen, da Sie diese Dokumente andernfalls erneut aus dem Änderungsfeed erhalten. Beachten Sie, dass der Änderungsfeed die letzte endgültige Momentaufnahme der Dokumente aufbewahrt, und Sie so die vorhergehende Momentaufnahme des Dokuments verlieren können. Der Änderungsfeed bewahrt nur eine letzte Version des Dokuments aus, und zwischenzeitlich kann das Dokument von anderen Prozessen verändert worden sein.

Sie werden während der Problembehebung an Ihrem Code bald keine Dokumente mehr in der Warteschlange für unzustellbare Nachrichten finden. Azure Functions wird vom Änderungsfeedsystem automatisch aufgerufen, und der Prüfpunkt wird intern von Azure Functions verwaltet. Wenn Sie einen Rollback des Prüfpunkts ausführen und jeden Aspekt davon steuern möchten, sollten Sie die Verwendung des Change Feed Processor-SDKs in Erwägung ziehen.

### <a name="are-there-any-extra-costs-for-using-the-azure-cosmos-db-trigger"></a>Fallen für die Verwendung des Azure Cosmos DB-Triggers zusätzliche Kosten an?

Die Änderungsfeed-Verarbeitungsbibliothek nutzt den Azure Cosmos DB-Trigger intern. Daher ist dafür eine zusätzliche Sammlung namens Leasesammlung erforderlich, um Status- und partielle Prüfpunkte zu verwalten. Die Statusverwaltung ist für eine dynamische Skalierung und Fortsetzung erforderlich, falls Sie Ihre Azure-Funktionen beenden und die Verarbeitung zu einem späteren Zeitpunkt fortsetzen möchten. Weitere Informationen finden Sie unter [Arbeiten mit der Änderungsfeed-Verarbeitungsbibliothek](change-feed-processor.md).

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über Änderungsfeeds:

* [Übersicht über Änderungsfeeds](change-feed.md)
* [Möglichkeiten zum Lesen von Änderungsfeeds](read-change-feed.md)
* [Verwenden der Änderungsfeed-Verarbeitungsbibliothek](change-feed-processor.md)
* [Arbeiten mit der Änderungsfeed-Verarbeitungsbibliothek](change-feed-processor.md)
* [Serverloses Datenbankcomputing mit Azure Cosmos DB und Azure Functions](serverless-computing-database.md)
