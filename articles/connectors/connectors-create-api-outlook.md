---
title: Outlook.com-Connector für Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen Sie Logik-Apps mit Azure App Service. Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 1338b7ce8f72a57cb65f9389e630fd7ebd0417b1
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295583"
---
# <a name="get-started-with-the-outlookcom-connector"></a>Erste Schritte mit dem Outlook.com-Connector
Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-outlookcom"></a>Herstellen einer Verbindung mit Outlook.com
Um Logik-Apps mit Outlook.com zu erstellen, müssen Sie zuerst eine **Verbindung** herstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für Outlook.com |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

> [!INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)]
>

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/outlook/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.