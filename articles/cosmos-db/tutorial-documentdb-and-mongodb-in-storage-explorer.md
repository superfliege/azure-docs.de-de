---
title: Verwalten von Azure Cosmos DB in Azure Storage-Explorer
description: Erfahren Sie, wie Sie Azure Cosmos DB in Azure Storage-Explorer verwalten.
Keywords: Azure Cosmos DB, Azure Storage-Explorer, DocumentDB, MongoDB, DocumentDB
services: cosmos-db
documentationcenter: 
author: Jiaj-Li
manager: omafnan
editor: 
tags: Azure Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: Jiaj-Li
ms.openlocfilehash: 2cd3656156b77c71be85a1a18567232f4466fc68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Verwalten von Azure Cosmos DB in Azure Storage-Explorer (Vorschau)

Mit der Verwendung von Azure Cosmos DB in Azure Storage-Explorer können Benutzer Azure Cosmos DB-Entitäten verwalten, Daten bearbeiten und gespeicherte Prozeduren und Trigger zusammen mit anderen Azure-Entitäten wie Speicherblobs und Warteschlangen aktualisieren. Nun können Sie mit demselben Tool Ihre verschiedenen Azure-Entitäten an einem Ort verwalten. Derzeit unterstützt Azure Storage-Explorer SQL- (DocumentDB) und MongoDB-Konten.

In diesem Artikel erfahren Sie, wie Sie Storage-Explorer zum Verwalten von Azure Cosmos DB verwenden.


## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Cosmos DB-Konto für eine SQL- (DocumentDB) oder MongoDB-Datenbank. Wenn Sie kein Konto haben, können Sie im Azure-Portal eines erstellen, wie unter [Azure Cosmos DB: Erstellen einer Web-App mit einer DocumentDB-API mit .NET und dem Azure-Portal](create-documentdb-dotnet.md) beschrieben.
- Installieren Sie die neuesten Komponenten von Azure Storage-Explorer. Sie können diese mithilfe der folgenden Links installieren: [Linux](https://go.microsoft.com/fwlink/?linkid=858559), [Mac](https://go.microsoft.com/fwlink/?linkid=858561), [Windows](https://go.microsoft.com/fwlink/?linkid=858562).

## <a name="connect-to-an-azure-subscription"></a>Herstellen einer Verbindung mit einem Azure-Abonnement

1. Klicken Sie nach dem Installieren von **Azure Storage-Explorer** auf das Symbol **Plug-In** auf der linken Seite, wie in der folgenden Abbildung dargestellt.
       
   ![Plug-In-Symbol](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/plug-in-icon.png)
 
2. Wählen Sie **Azure-Konto hinzufügen** aus, und klicken Sie dann auf **Anmelden**.

   ![Herstellen einer Verbindung mit einem Azure-Abonnement](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-azure-subscription.png)

2. Wählen Sie im Dialogfeld **Azure-Anmeldung** die Option **Anmelden** aus, und geben Sie Ihre Azure-Anmeldeinformationen ein.

    ![Anmelden](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/sign-in.png)

3. Wählen Sie Ihr Abonnement in der Liste aus, und klicken Sie dann auf **Anwenden**.

    ![Anwenden](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/apply-subscription.png)

    Der Explorer-Bereich wird aktualisiert und zeigt die Konten im ausgewählten Abonnement an.

    ![Kontoliste](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/account-list.png)

    Sie haben nun Ihr **Azure Cosmos DB-Konto** mit Ihrem Azure-Abonnement verbunden.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Herstellen einer Verbindung mit Azure Cosmos DB mithilfe einer Verbindungszeichenfolge

Eine alternative Möglichkeit zum Herstellen einer Verbindung mit einer Azure Cosmos DB ist die Verwendung einer Verbindungszeichenfolge. Gehen Sie folgendermaßen vor, um eine Verbindung mithilfe einer Verbindungszeichenfolge herzustellen.

1. Suchen Sie in der linken Struktur nach **Lokal und angefügt**, klicken Sie mit der rechten Maustaste auf **Azure Cosmos DB-Konten**, und wählen Sie **Mit Azure-Cosmos-Datenbank verbinden** aus.

    ![Herstellen einer Verbindung mit Azure Cosmos DB mithilfe einer Verbindungszeichenfolge](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-db-by-connection-string.png)

2. Wählen Sie die passende **Standardoberfläche** für Ihren Kontotyp aus, entweder **DocumentDB** oder **MongoDB**. Fügen Sie Ihre **Verbindungszeichenfolge** ein, und klicken Sie dann auf **OK**, um die Verbindung mit Ihrem Azure Cosmos DB-Konto herzustellen. Informationen zum Abrufen der Verbindungszeichenfolge finden Sie unter [Abrufen der Verbindungszeichenfolge](https://docs.microsoft.com/en-us/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Verbindungszeichenfolge](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Azure Cosmos DB-Ressourcenverwaltung

Sie können ein Azure Cosmos DB-Konto über die folgenden Vorgänge verwalten:
* Öffnen des Kontos im Azure-Portal
* Hinzufügen der Ressource zur Schnellzugriffsliste
* Suchen und Aktualisieren von Ressourcen
* Erstellen und Löschen von Datenbanken
* Erstellen und Löschen von Sammlungen
* Erstellen, Bearbeiten, Löschen und Filtern von Dokumenten
* Verwalten von gespeicherten Prozeduren, Triggern und benutzerdefinierten Funktionen

### <a name="quick-access-tasks"></a>Schnellzugriffsaufgaben

Indem Sie mit der rechten Maustaste auf ein Abonnement im Explorer-Bereich klicken, können Sie viele schnelle Aktionen ausführen:

* Wenn Sie mit der rechten Maustaste auf ein Azure Cosmos DB-Konto oder eine Datenbank klicken, können Sie **Im Portal öffnen** auswählen und die Ressource über den Browser im Azure-Portal verwalten.

     ![Im Portal öffnen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/open-in-portal.png)

* Sie können auch die Azure Cosmos DB-Konten, Datenbanken oder Sammlungen als **Schnellzugriff** hinzufügen.
* **Ab hier suchen** ermöglicht eine Schlüsselwortsuche unter dem ausgewählten Pfad.

    ![Ab hier suchen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Datenbank- und Sammlungsverwaltung
#### <a name="create-a-database"></a>Erstellen einer Datenbank 
Klicken Sie mit der rechten Maustaste auf das Azure Cosmos DB-Konto, wählen Sie **Datenbank erstellen** aus, geben Sie den Datenbanknamen ein, und drücken Sie zum Abschluss die **Eingabetaste**.

![Erstellen einer Datenbank](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Löschen einer Datenbank
Klicken Sie mit der rechten Maustaste auf die Datenbank, klicken Sie auf **Datenbank löschen**, und klicken Sie im Popupfenster auf **Ja**. Der Datenbankknoten wird gelöscht, und das Azure Cosmos DB-Konto wird automatisch aktualisiert.

![database1 löschen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database1.png)  

![database2 löschen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Erstellen einer Sammlung
Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie **Sammlung erstellen** aus, und geben Sie dann die benötigten Informationen ein, z.B. **Sammlungs-ID**, **Speicherkapazität** usw. Klicken Sie auf **OK**, um den Vorgang abzuschließen. Informationen über Partitionsschlüsseleinstellungen finden Sie unter [Entwerfen für Partitionierung](partition-data.md#designing-for-partitioning).

Wenn zum Erstellen einer Sammlung ein Partitionsschlüssel verwendet wird, kann der Partitionsschlüsselwert für die Auflistung nach Abschluss der Erstellung nicht mehr geändert werden.

![collection1 erstellen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection.png)

![collection2 erstellen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Löschen einer Sammlung
Klicken Sie mit der rechten Maustaste auf die Sammlung, klicken Sie auf **Sammlung löschen**, und klicken Sie dann im Popupfenster auf **Ja**. 

Der Sammlungsknoten wird gelöscht, und die Datenbank wird automatisch aktualisiert.  

![Sammlung löschen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Dokumentverwaltung

#### <a name="create-and-modify-documents"></a>Erstellen und Ändern von Dokumenten
Um ein neues Dokument zu erstellen, öffnen Sie im linken Fenster **Dokumente**, klicken Sie auf **Neues Dokument**. Bearbeiten Sie den Inhalt im rechten Bereich, und klicken Sie auf **Speichern**. Sie können auch ein vorhandenes Dokument aktualisieren und dann auf **Speichern** klicken. Änderungen können durch Klicken auf **Verwerfen** verworfen werden.

![Dokument](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/document.png)

#### <a name="delete-a-document"></a>Löschen eines Dokuments
Klicken Sie auf die Schaltfläche **Löschen**, um das ausgewählte Dokument zu löschen.
#### <a name="query-for-documents"></a>Abfragen von Dokumenten
Bearbeiten Sie den Dokumentfilter durch Eingeben einer [SQL-Abfrage](documentdb-sql-query.md), und klicken Sie dann auf **Anwenden**.

![Filter](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Verwalten von gespeicherten Prozeduren, Triggern und UDFs
* Um eine gespeicherte Prozedur zu erstellen, klicken Sie in der linken Struktur mit der rechten Maustaste auf **Gespeicherte Prozedur**, wählen Sie **Gespeicherte Prozedur erstellen** aus, geben Sie links einen Namen ein, geben Sie die Skripts für die gespeicherte Prozedur im rechten Fenster ein, und klicken Sie dann auf **Erstellen**. 
* Sie können auch vorhandene gespeicherte Prozeduren bearbeiten, indem Sie doppelklicken, die Aktualisierung vornehmen und dann auf **Aktualisieren** klicken, um sie zu speichern, oder auf **Verwerfen**, um die Änderungen zu verwerfen.

![Gespeicherte Prozedur](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/stored-procedure.png)

* Die Vorgänge für **Trigger** und **UDFs** ähneln denen für **Gespeicherte Prozeduren**.

## <a name="demo"></a>Demo
* Im folgenden Video erfahren Sie, wie Sie Azure Cosmos DB in Azure Storage-Explorer verwenden: [Azure Cosmos DB in Azure Storage Explorer](https://go.microsoft.com/fwlink/?linkid=858710).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Azure Storage-Explorer mit Ihrem Azure Cosmos DB-Konto verbunden haben, erfahren Sie in [Erste Schritte mit Storage-Explorer (Vorschau)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer) mehr über Storage-Explorer und verbinden weitere Dienste.

