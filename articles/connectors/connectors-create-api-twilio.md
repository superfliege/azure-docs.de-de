---
title: Hinzufügen des Twilio-Connectors zu Ihren Azure-Logik-Apps | Microsoft-Dokumentation
description: Übersicht über den Twilio-Connector mit REST-API-Parametern
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 2bb7961c850fc8a35f3e114d9497941b188cf51b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-twilio-connector"></a>Erste Schritte mit dem Twilio-Connector
Stellen Sie eine Verbindung mit Twilio her, um global SMS, MMS und IP-Nachrichten zu senden und zu empfangen. Mit Twilio können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Twilio abgerufen werden. 
* Verwenden von Aktionen zum Abrufen einer Nachricht, von Listennachrichten und mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn Sie eine neue Twilio-Nachricht erhalten, können Sie diese Nachricht B. annehmen und als Service Bus-Workflow verwenden. 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-twilio"></a>Herstellen einer Verbindung mit Twilio
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, geben Sie die folgenden Twilio-Werte ein:

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Konto-ID |Ja |Geben Sie Ihre Twilio-Konto-ID ein. |
| Access Token |Ja |Geben Sie Ihr Twilio-Zugriffstoken ein. |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Wenn Sie kein Zugriffstoken für Twilio besitzen, finden Sie unter [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity) (Benutzeridentität und Zugriffstoken) weitere Informationen.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/twilio/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.