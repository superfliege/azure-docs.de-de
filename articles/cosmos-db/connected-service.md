---
title: "Verbundener Visual Studio-Dienst für Azure Cosmos DB"
description: "Ermöglicht es Entwicklern, ihr Azure Cosmos DB-Konto auf einfache Weise zu verbinden und Ressourcen über verbundene Visual Studio-Dienste zu verwalten."
services: cosmos-db
documentationcenter: 
author: jejiang
manager: DJ
+tags: cosmos-db
editor: Jenny Jiang
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jejiang
ms.openlocfilehash: de0c83e4c99894f98de18eb089ce11cdf5c70f2e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-visual-studio-connected-service-preview"></a>Azure Cosmos DB: Verbundener Visual Studio-Dienst (Vorschauversion)

Mit verbundenen Visual Studio-Diensten können Entwickler ihr Azure Cosmos DB-Konto leicht verbinden und ihre Ressourcen verwalten.

Sie können auch den Daten-Explorer im verbundenen Dienst zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern verwenden, um serverseitige Geschäftslogik auszuführen. Der Daten-Explorer stellt den gesamten integrierten programmgesteuerten Datenzugriff in den APIs zur Verfügung, ermöglicht aber den einfachen Zugriff auf Ihre Daten.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren. 
* Ein Azure Cosmos DB-Konto. Gehen Sie wie folgt vor, wenn Sie noch kein Konto besitzen: Führen Sie die Schritte zum [Erstellen eines Azure Cosmos DB-Kontos](create-documentdb-dotnet.md) aus, um ein Konto über das Portal zu erstellen, oder verwenden Sie die Informationen unter [Erstellen eines Azure Cosmos DB-Kontos im Tool für verbundene Dienste](#Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool). 
* Wenn Sie eine lokale Umgebung für Entwicklungszwecke nutzen möchten, können Sie den [Azure Cosmos DB-Emulator](local-emulator.md) verwenden. Diese Umgebung emuliert den Azure Cosmos DB-Dienst.
* [Visual Studio](http://www.visualstudio.com/)
* Die aktuellen Komponenten des verbundenen Diensts für Azure Cosmos DB. Sie können den verbundenen Dienst für Azure Cosmos DB vom Visual Studio Marketplace herunterladen. Dies ist im folgenden Screenshot dargestellt. Öffnen Sie **Visual Studio** auf Ihrem Computer. Wählen Sie im Menü **Extras** die Option **Extensions and update...** (Erweiterungen und Update...) und dann **Online** / **Visual Studio Marketplace**. Geben Sie **cosmosdb** ein, um nach den Komponenten zu suchen.

    Sie können den verbundenen Dienst für Azure Cosmos DB auch über den [Visual Studio Marketplace](https://go.microsoft.com/fwlink/?linkid=858709) installieren.

    ![Screenshot: „Connected Service download bits.png“](./media/connected-service/connected-service-downloadbits.png) 

## <a id="SetupVS"></a>Einrichten Ihrer Visual Studio-Projektmappe
1. Öffnen Sie auf Ihrem Computer **Visual Studio**.
2. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Visual C#** / **Konsolen-App (.NET Framework)** oder **WPF-App (.NET Framework)**, geben Sie Ihrem Projekt einen Namen, und klicken Sie anschließend auf **OK**.

    ![Screenshot des Fensters „Neues Projekt“](./media/connected-service/connected-service-new-project.png)
    
## <a name="add-connected-service-and-add-account"></a>Hinzufügen des verbundenen Diensts und Hinzufügen eines Kontos
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Hinzufügen** / **Verbundener Dienst**. Oder klicken Sie auf das Menü **Projekt**, und wählen Sie die Option **Verbundenen Dienst hinzufügen**.

    ![Screenshot des Fensters „Verbundenen Dienst hinzufügen“](./media/connected-service/connected-service-add-connectedservice-rightclick.png)
2. Klicken Sie auf der Seite für verbundene Dienste auf **Verbundene Dienste** / **Azure Cosmos DB**, um die Seite **Azure Cosmos DB** zu öffnen.

    ![Screenshot: Fenster „Azure Cosmos DB“](./media/connected-service/connected-service-choose-azure-cosmosdb.png)
3. Klicken Sie auf den Pfeil nach unten, um sich zum ersten Mal anzumelden oder ein Konto hinzuzufügen. Nach dem Anmelden werden alle Azure Cosmos DB-Konten im leeren Bereich angezeigt. Wählen Sie ein Azure Cosmos DB-Konto aus, das Ihrem Projekt hinzugefügt werden soll.

    ![Screenshot: Anmeldung und aufgeführtes DB-Konto](./media/connected-service/connected-service-add-db-account.png)
4. Nachdem Sie ein Azure Cosmos DB-Konto hinzugefügt haben, wird dem Projekt ein Ordner für den verbundenen Dienst des Azure Cosmos DB-Kontos hinzugefügt. Sie können mehr als ein Azure Cosmos DB-Konto hinzufügen, indem Sie die Schritte 1 bis 3 wiederholen.

    ![Screenshot: Fenster mit dem Ordner für den verbundenen Dienst](./media/connected-service/connected-service-add-connectedservice-folder.png)

5. Nachdem Sie einen verbundenen Dienst für Azure Cosmos DB hinzugefügt haben, können Sie Ihr Projekt ändern,um den Zugriff auf Azure Cosmos DB zu ermöglichen. Sie haben folgende Möglichkeiten:

    * Einige NuGet-Pakete, die für den Azure Cosmos DB-Client erforderlich sind, sind installiert. Sie können sie über die Paketkonfigurationsdatei anzeigen. 

        ![Neue Azure Cosmos DB-Paketkonfiguration](./media/connected-service/connected-service-packages-config.png)   
    
    * Der Azure Cosmos DB-Verbindungs-URI und der dazugehörige Schlüssel werden der Projektkonfigurationsdatei (hier: „App.config“) hinzugefügt. 

        ![Neue Azure Cosmos DB-App-Konfiguration](./media/connected-service/connected-service-app-config.png) 

## <a name="open-azure-cosmos-db-explorer"></a>Öffnen von Azure Cosmos DB Explorer
1. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Open Cosmos DB Explorer...** (Cosmos DB Explorer öffnen...).

    ![Screenshot: Versuchtes Öffnen des Fensters für den Daten-Explorer](./media/connected-service/connected-service-right-click-open-data-exporer.png)
2. Klicken Sie auf der Seite **Choose a Cosmos DB Account** (Cosmos DB-Konto auswählen) auf die Dropdownliste, um ein Azure Cosmos DB-Konto auszuwählen.

    ![Screenshot: Fenster mit dem hinzugefügten verbundenen Dienst für das Konto](./media/connected-service/connected-service-open-explorer.png)
3. Klicken Sie auf **Öffnen**. Das Daten-Explorer-Fenster wird angezeigt.

## <a id="Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool"></a>Erstellen eines Azure Cosmos DB-Kontos im Tool für verbundene Dienste
1. Klicken Sie auf der Seite für verbundene Dienste unten links auf **Create a New Cosmos DB Account** (Neues Cosmos DB-Konto erstellen), um die Seite **Cosmos DB-Konto erstellen** zu öffnen.

    ![Screenshot: Geöffnetes Fenster zum Erstellen eines Azure Cosmos DB-Kontos (Einstiegspunkt)](./media/connected-service/connected-service-click-new-db-account.png)
2. Geben Sie auf der Seite **Cosmos DB-Konto erstellen** die Konfiguration an, die Sie für dieses Azure Cosmos DB-Konto verwenden möchten.

    Füllen Sie die Felder auf der Seite **Cosmos DB-Konto erstellen** aus, indem Sie die Informationen im folgenden Screenshot als Referenz verwenden. 
 
    ![Seite für neue Azure Cosmos DB](./media/connected-service/connected-service-create-new-account.png)        
3. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.

## <a name="use-data-explorer"></a>Verwenden des Daten-Explorers

Nach dem Öffnen des Daten-Explorer können Sie wie folgt vorgehen:
* Erstellen und Löschen von Datenbanken
* Erstellen und Löschen von Sammlungen
* Erstellen, Löschen und Filtern von Dokumenten
* Erstellen und Löschen von gespeicherten Prozeduren
* Erstellen und Löschen von Triggern und benutzerdefinierten Funktionen zum Ausführen von Schritten für serverseitige Geschäftslogik 

![Seite für neue Azure Cosmos DB](./media/connected-service/connected-service-dataexplorerui.png)

## <a name="demo"></a>Demo

Sehen Sie sich das folgende Video an. Es enthält eine Anleitung zum Verwenden des verbundenen Diensts von Azure Cosmos DB in Visual Studio: [Use Azure Cosmos DB Connected Service in Visual Studio](https://go.microsoft.com/fwlink/?linkid=858711) (Verwenden des verbundenen Diensts von Azure Cosmos DB in Visual Studio).

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos DB-Kontos
> * Hinzufügen des verbundenen Diensts und Hinzufügen eines Kontos
> * Öffnen von Azure Cosmos DB Explorer
> * Verwenden des Daten-Explorers

Nachdem Sie die verbundenen Dienste nun für Ihr Azure Cosmos DB-Konto eingerichtet haben, können Sie mit einem Tutorial fortfahren, um mit der Entwicklung für Ihre Lösung zu beginnen:

* [Entwickeln mit der DocumentDB-API in .NET](tutorial-develop-documentdb-dotnet.md)
* [Azure Cosmos DB: DocumentDB-API – Tutorial zu den ersten Schritten](documentdb-get-started.md)
* Möchten Sie Azure Cosmos DB nutzen, um Skalierungs- und Leistungstests durchzuführen? Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).

