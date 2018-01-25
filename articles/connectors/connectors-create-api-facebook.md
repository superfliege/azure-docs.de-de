---
title: "Hinzufügen des Facebook-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Facebook-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 27a9297c5d4cfb52f254ceb12e37423403e7f41b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-facebook-connector"></a>Erste Schritte mit dem Facebook-Connector
Verbinden Sie sich mit Facebook, um z. B. die Chronik zu ergänzen oder einen Seitenfeed abzurufen. Facebook ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Facebook abgerufen werden. 
* Verwenden eines Triggers, wenn ein neuer Beitrag empfangen wird.
* Verwenden von Aktionen, die z. B. die Chronik ergänzen oder einen Seitenfeed abrufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn ein neuer Beitrag in Ihrer Chronik vorhanden ist, können Sie diesen Beitrag per Push an Ihren Twitter-Feed übertragen. 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-facebook"></a>Herstellen einer Verbindung mit Facebook
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Facebook-Konto erlauben.

1. Melden Sie sich bei Ihrem Facebook-Konto an.
2. Wählen Sie **Autorisieren**aus, um zu erlauben, dass Ihre Logik-Apps sich mit Ihrem Facebook-Konto verbinden und dieses nutzen. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/facebook/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.