---
title: Herstellen einer Verbindung mit Google Drive – Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen und Verwalten von Dateien mit Google Drive-REST-APIs und Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 4994687afbdd23f0265138cd1d4eff53bb47c163
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295882"
---
# <a name="get-started-with-the-google-drive-connector"></a>Erste Schritte mit dem Google Drive-Connector
Verbinden Sie sich mit Google Drive, um Dateien zu erstellen, Zeilen abzurufen usw. Google Drive ermöglicht Folgendes: 

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus einer Suche abgerufen werden. 
* Verwenden von Aktionen, um Bilder, Nachrichten und mehr zu suchen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z. B. ein Video suchen und dann Twitter verwenden, um das Video in einem Twitter-Feed zu posten.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-the-connection-to-google-drive"></a>Herstellen der Verbindung mit Google Drive
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Google Drive erlauben.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Google Drive-Eigenschaften ein, z. B. Ordnerpfad oder Dateiname. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/googledrive/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.
