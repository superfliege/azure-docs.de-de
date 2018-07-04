---
title: Azure Cosmos DB-BulkExecutor-Bibliothek – Übersicht | Microsoft-Dokumentation
description: Dieser Artikel bietet Informationen zur Azure Cosmos DB-BulkExecutor-Bibliothek, ihren Vorteilen und ihrer Architektur.
keywords: Java-BulkExecutor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 893888904b6afc583c3c20b94d08eb3255a98cad
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36303991"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB-BulkExecutor-Bibliothek – Übersicht
 
Azure Cosmos DB ist ein schneller, flexibler und global verteilter Datenbankdienst, der so konzipiert ist, dass er sich elastisch horizontal hochskalieren lässt und damit Folgendes unterstützt: 

* Hohe Durchsätze für Lese- und Schreibvorgänge (Millionen von Vorgängen pro Sekunde).  
* Speichern großer Mengen von Transaktions- und Vorgangsdaten (Hunderte von Terabytes oder sogar noch mehr) mit vorhersagbarer Latenz im Millisekundenbereich.  

Mit der BulkExecutor-Bibliothek können Sie diese riesigen Durchsatz- und Speichermengen optimal nutzen und über APIs für Massenimport und Massenaktualisierung die entsprechenden Massenvorgänge in Azure Cosmos DB ausführen. In den folgenden Abschnitten erfahren Sie mehr über die Funktionen der BulkExecutor-Bibliothek. 

> [!NOTE] 
> Zurzeit unterstützt die BulkExecutor-Bibliothek Import- und Updatevorgänge, und die Bibliothek wird nur von Azure Cosmos DB-SQL API-Konten unterstützt. Informationen zu Updates für die Bibliothek finden Sie in den Versionsanmerkungen für [.NET](sql-api-sdk-bulk-executor-dot-net.md) und [Java](sql-api-sdk-bulk-executor-java.md).
 
## <a name="key-features-of-the-bulk-executor-library"></a>Wichtigste Funktionen der BulkExecutor-Bibliothek  
 
* Die Bibliothek reduziert die Computeressourcen auf Clientseite, die zur vollständigen Nutzung des einem Container zugeordneten Durchsatzes erforderlich sind. Eine Singlethreadanwendung, die Daten über die Massenimport-API schreibt, erzielt einen zehnmal höheren Schreibdurchsatz im Vergleich zu einer Multithreadanwendung, die Daten parallel schreibt und gleichzeitig die CPU des Clientcomputers vollständig nutzt.  

* Die Bibliothek verarbeitet die Anforderungsdrosselung, Anforderungstimeouts und andere vorübergehende Ausnahmen auf effiziente Weise, sodass Benutzer nicht mehr mühsam die Anwendungslogik dafür schreiben müssen.  

* Zudem bietet sie einen vereinfachten Mechanismus, mit dem Anwendungen Massenvorgänge zum horizontalen Hochskalieren ausführen können. Eine einzige BulkExecutor-Instanz, die auf einem virtuellen Azure-Computer ausgeführt wird, kann mehr als 500.000 Anforderungseinheiten/Sek. verbrauchen, und Sie können eine höhere Durchsatzrate erzielen, indem Sie zusätzliche Instanzen auf einzelnen virtuellen Clientcomputern hinzufügen.  
 
* Dank einer Architektur für die horizontale Skalierung kann eine solche Instanz innerhalb einer Stunde mehr als ein Terabyte an Daten importieren.  

* Vorhandene Daten in Azure Cosmos DB-Containern können per Massenvorgang in Form von Patches aktualisiert werden. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Wie funktioniert BulkExecutor? 

Wenn ein Massenvorgang zum Importieren oder Aktualisieren von Dokumenten mit einem Batch von Entitäten ausgelöst wird, werden die Dokumente anfänglich entsprechend dem Schlüsselbereich der zugehörigen Azure Cosmos DB-Partition in Buckets gemischt. In jedem Bucket, der einem Partitionsschlüsselbereich entspricht, werden die Dokumente in Minibatches aufgeteilt, und jedes Minibatch ist eine Nutzlast, für die auf Serverseite ein Commit ausgeführt wird. Die BulkExecutor-Bibliothek verfügt über integrierte Optimierungen für die gleichzeitige Ausführung dieser Minibatches sowohl innerhalb der Partitionsschlüsselbereiche als auch übergreifend über diese Bereiche. Die folgende Abbildung zeigt, wie BulkExecutor Daten in Batches für verschiedene Partitionsschlüssel aufteilt:  

![BulkExecutor-Architektur](./media/bulk-executor-overview/bulk-executor-architecture.png)

Die BulkExecutor-Bibliothek stellt sicher, dass der Durchsatz, der einer Sammlung zugeordnet ist, optimal genutzt wird. Sie verwendet für jeden Azure Cosmos DB-Partitionsschlüsselbereich einen  [AIMD-Steuerungsmechanismus für Überlastungen](https://tools.ietf.org/html/rfc5681), um Drosselungen und Zeitüberschreitungen effizient zu verarbeiten. 

## <a name="next-steps"></a>Nächste Schritte 
  
* Erfahren Sie mehr darüber, indem Sie die Beispielanwendungen ausprobieren, die die BulkExecutor-Bibliothek in [.NET](bulk-executor-dot-net.md) und [Java](bulk-executor-java.md) nutzen.  
* Sehen Sie sich die Informationen und Versionsanmerkungen zum BulkExecutor SDK in [.NET](sql-api-sdk-bulk-executor-dot-net.md) und [Java](sql-api-sdk-bulk-executor-java.md) an.
* Die BulkExecutor-Bibliothek ist in den Cosmos DB Spark-Connector integriert. Weitere Informationen finden Sie im Artikel [Spark-Connector für Azure Cosmos DB](spark-connector.md).  
* Die BulkExecutor-Bibliothek ist auch in eine neue Version des [Azure Cosmos DB-Connectors](https://aka.ms/bulkexecutor-adf-v2) integriert, damit Azure Data Factory Daten kopieren kann.
