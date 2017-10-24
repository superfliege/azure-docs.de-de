---
title: "Erstellen einer durch Azure Cosmos DB ausgelösten Funktion| Microsoft-Dokumentation"
description: "Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die aufgerufen wird, wenn in Azure Cosmos DB Daten einer Datenbank hinzugefügt werden."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/02/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 1ff4c2e024faba777fc479b3cd5864e097bbfce1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Erstellen einer durch Azure Cosmos DB ausgelösten Funktion

Informationen zum Erstellen einer Funktion, die ausgelöst wird, wenn in Azure Cosmos DB einer Datenbank Daten hinzugefügt oder Daten in ihr geändert werden. Weitere Informationen zu Azure Cosmos DB finden Sie unter [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](..\cosmos-db\serverless-computing-database.md).

![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Erstellen eines Azure Cosmos DB-Triggers

1. Erweitern Sie die Funktionen-App, und klicken Sie auf die Schaltfläche **+** neben **Functions**. Wenn dies die erste Funktion in Ihrer Funktionen-App ist, wählen Sie **Benutzerdefinierte Funktion**. Hiermit wird der vollständige Satz von Funktionsvorlagen angezeigt.

    ![Schnellstartseite für Funktionen im Azure-Portal](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

2. Suchen und wählen Sie die **Azure CosmosDBTrigger**-Vorlage für Ihre gewünschte Sprache.

    ![Erstellen der ausgelösten Azure Cosmos DB-Funktion](./media/functions-create-cosmos-db-triggered-function/select-cosmos-db-trigger-portal.png)

3. Konfigurieren Sie den neuen Trigger mit den Einstellungen, die in der Tabelle unter der folgenden Abbildung enthalten sind.

    ![Erstellen der ausgelösten Azure Cosmos DB-Funktion](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)
    
    | Einstellung      | Empfohlener Wert  | Beschreibung                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Name Ihrer Funktion** | Standard | Verwenden Sie den von der Vorlage vorgeschlagenen Standardfunktionsnamen. |
    | **Datenbankname** | Aufgaben | Name der Datenbank mit der zu überwachenden Sammlung. |
    | **Sammlungsname** | Items | Name der zu überwachenden Sammlung. |
    | **Erstellen der Leasesammlung, wenn sie nicht vorhanden ist** | Aktiviert | Die Sammlung ist noch nicht vorhanden und muss erstellt werden. |

4. Wählen Sie neben **Azure Cosmos DB-Kontoverbindung** die Option **Neu** und anschließend ein bestehendes Cosmos DB-Konto oder **+ Neu erstellen** aus. 
 
    ![Konfigurieren der Azure Cosmos DB-Verbindung](./media/functions-create-cosmos-db-triggered-function/functions-create-CosmosDB.png)

6. Wenn Sie ein neues Cosmos DB-Konto erstellen, verwenden Sie die **Neues Konto**-Einstellungen wie in der Tabelle angegeben.

    | Einstellung      | Empfohlener Wert  | Beschreibung                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Name der Datenbank | Eindeutige ID für die Azure Cosmos DB-Datenbank  |
    | **API** | SQL (DocumentDB) | In diesem Thema wird die Dokumentdatenbank-API verwendet.  |
    | **Abonnement** | Azure-Abonnement | Azure-Abonnement  |
    | **Ressourcengruppe** | myResourceGroup |  Verwenden Sie die vorhandene Ressourcengruppe, die Ihre Funktions-App enthält. |
    | **Standort**  | Europa, Westen | Wählen Sie einen Standort in der Nähe Ihrer Funktions-App oder in der Nähe anderer Apps aus, die die gespeicherten Dokumente verwenden.  |

6. Klicken Sie auf **OK**, um die Datenbank zu erstellen. Die Datenbankerstellung dauert unter Umständen einige Minuten. Nach Abschluss der Datenbankerstellung wird die Datenbankverbindungszeichenfolge als Einstellung der Funktions-App gespeichert. Der Name dieser App-Einstellung wird in **Azure Cosmos DB-Kontoverbindung** eingefügt. 

7. Klicken Sie auf **Erstellen**, um Ihre durch Azure Cosmos DB ausgelöste Funktion zu erstellen. Nachdem die Funktion erstellt ist, wird der vorlagenbasierte Funktionscode angezeigt.  

    ![Cosmos DB-Funktionsvorlage in C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Diese Funktionsvorlage schreibt die Anzahl von Dokumenten und die erste Dokument-ID in die Protokolle. 

Als Nächstes stellen Sie eine Verbindung mit Ihrem Azure Cosmos DB-Konto her und erstellen die Sammlung **Tasks** in der Datenbank. 

## <a name="create-the-items-collection"></a>Erstellen der Sammlung „Items“

1. Öffnen Sie im Browser in einer neuen Registerkarte eine zweite Instanz des [Azure-Portals](https://portal.azure.com). 

2. Erweitern Sie links im Portal die Symbolleiste, geben Sie `cosmos` in das Suchfeld ein, und wählen Sie **Azure Cosmos DB** aus.

    ![Suchen nach dem Azure Cosmos DB-Dienst](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

2. Wählen Sie Ihr Azure Cosmos DB-Konto aus, und wählen Sie dann den **Daten-Explorer**. 
 
3. Wählen Sie in **Sammlungen** **taskDatabase**, und wählen Sie dann **Neue Sammlung**.

    ![Erstellen einer Sammlung](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection.png)

4. Verwenden Sie in **Sammlung hinzufügen** die Einstellungen, die in der Tabelle unten in der Abbildung gezeigt werden. 
 
    ![Definieren von taskCollection](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection2.png)
 
    | Einstellung|Empfohlener Wert|Beschreibung |
    | ---|---|--- |
    | **Datenbank-ID** | Aufgaben |Der Name Ihrer neuen Datenbank. Dieser muss mit dem in der Funktionsbindung definierten Namen übereinstimmen. |
    | **Sammlungs-ID** | Items | Der Name der neuen Sammlung. Dieser muss mit dem in der Funktionsbindung definierten Namen übereinstimmen.  |
    | **Speicherkapazität** | Fixed (10 GB)|Verwenden Sie den Standardwert. Dieser Wert gibt die Speicherkapazität der Datenbank an. |
    | **Durchsatz** |400 RU| Verwenden Sie den Standardwert. Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten reduzieren möchten. |
    | **[Partitionsschlüssel](../cosmos-db/partition-data.md#design-for-partitioning)** | /category|Ein Partitionsschlüssel, der Daten gleichmäßig auf alle Partitionen verteilt. Die Auswahl des richtigen Partitionsschlüssels ist wichtig für die Erstellung einer leistungsfähigen Sammlung. | 

1. Klicken Sie auf **OK**, um die Sammlung **Tasks** zu erstellen. Es dauert möglicherweise kurze Zeit, bis die Sammlung erstellt ist.

Sobald die in der Sammlung angegebene Funktionsbindung vorhanden ist, können Sie die Funktion durch Hinzufügen von Dokumenten zu dieser neuen Sammlung testen.

## <a name="test-the-function"></a>Testen der Funktion

1. Erweitern Sie die neue Sammlung **taskCollection** im Daten-Explorer, wählen Sie **Dokumente** und dann **Neues Dokument**.

    ![Erstellen eines Dokuments in taskCollection](./media/functions-create-cosmos-db-triggered-function/create-document-in-collection.png)

2. Ersetzen Sie den Inhalt des neuen Dokuments mit dem folgenden Inhalt, und wählen Sie dann **Speichern**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Wechseln Sie zur ersten Browserregisterkarte, die die Funktion im Portal enthält. Erweitern Sie die Funktionsprotokolle, und stellen Sie sicher, dass das neue Dokument die Funktion ausgelöst hat. Vergewissern Sie sich, dass der `task1`-Dokument-ID-Wert in die Protokolle geschrieben wurde. 

    ![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

4. (Optional) Wechseln Sie zurück zu Ihrem Dokument, nehmen Sie eine Änderung vor, und klicken Sie auf **Aktualisieren**. Wechseln Sie anschließend zurück zu den Funktionsprotokollen, und stellen Sie sicher, dass die Aktualisierung auch die Funktion ausgelöst hat.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn Ihrer Azure Cosmos DB ein Dokument hinzugefügt oder ein Dokument darin geändert wird.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen zu Queue Storage-Auslösern finden Sie unter [Azure Storage-Warteschlangenbindungen in Azure Functions](functions-bindings-storage-queue.md).
