---
title: Herstellen einer Verbindung mit Dropbox – Azure Logic Apps | Microsoft-Dokumentation
description: Hochladen und Verwalten von Dateien mit Dropbox-REST-APIs und Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/15/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 77203788a6329ed4c5b58419fbcf48a48da91b30
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295046"
---
# <a name="get-started-with-the-dropbox-connector"></a>Erste Schritte mit dem Dropbox-Connector
Stellen Sie eine Verbindung mit Dropbox her, um Dateien zu verwalten. Sie können verschiedene Aktionen ausführen und beispielsweise Dateien hochladen, aktualisieren, abrufen und in Dropbox löschen.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. [Erstellen Sie daher erst einmal eine Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md), wie hier beschrieben.

## <a name="connect-to-dropbox"></a>Herstellen einer Verbindung mit Dropbox
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit Dropbox herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen an, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Falle des Dropbox-Beispiels benötigen Sie also die Anmeldeinformationen für Ihr Dropbox-Konto, um die Verbindung mit Dropbox zu erstellen. [Weitere Informationen zu Verbindungen]()

### <a name="create-a-connection-to-dropbox"></a>Erstellen einer Verbindung mit Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Verwenden eines Dropbox-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

In diesem Beispiel verwenden wir den Trigger **When a file is created** (Wenn eine Datei erstellt wird). Wenn dieser Trigger aktiviert wird, rufen wir die Dropbox-Aktion **Get file content using path** (Dateiinhalt anhand des Pfads abrufen) auf. 

1. Geben Sie im Suchfeld des Logic Apps-Designers die Zeichenfolge *dropbox* ein, und wählen Sie anschließend den Trigger **Dropbox – When a file is created** (Dropbox – wenn eine Datei erstellt wird) aus.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Wählen Sie den Ordner aus, in dem Sie die Dateierstellung nachverfolgen möchten. Wählen Sie „...“ (rot markiert) aus, und navigieren Sie zum gewünschten Ordner für die Triggereingabe.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Verwenden einer Dropbox-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Führen Sie nach dem Hinzufügen des Triggers die folgenden Schritte aus, um eine Aktion hinzuzufügen, die den Inhalt der neuen Datei abruft.

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn eine neue Datei erstellt wird.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Wählen Sie **Aktion hinzufügen**aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Geben Sie *dropbox* ein, um nach Dropbox-spezifischen Aktionen zu suchen.  
4. Wählen Sie **Dropbox - Get file content using path** (Dropbox – Dateiinhalt anhand des Pfads abrufen) als Aktion aus, die ausgeführt werden soll, wenn im ausgewählten Dropbox-Ordner eine neue Datei erstellt wird. Die Aktionskontrollblock wird geöffnet. Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr Dropbox-Konto zu autorisieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Wählen Sie „...“ (rechts neben dem Steuerelement **Dateipfad**) aus, und navigieren Sie zum gewünschten Dateipfad. Alternativ können Sie das Token **Dateipfad** verwenden, um die Erstellung Ihrer Logik-App zu beschleunigen.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Speichern Sie Ihre Arbeit, und erstellen Sie in Dropbox eine neue Datei, um den Workflow zu aktivieren.  

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/dropbox/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.