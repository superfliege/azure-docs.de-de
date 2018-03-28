---
title: Wunderlist-Connector in Azure Logic Apps | Microsoft-Dokumentation
description: Stellen Sie eine Verbindung mit Wunderlist her, und verwenden Sie diese Verbindung zum Erstellen Ihres Workflows in Logic Apps.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 4d1ae30724faa59dcdeffd21be9c67d280d574f6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-wunderlist-connector"></a>Erste Schritte mit dem Wunderlist-Connector
Wunderlist ist eine Kombination aus Aufgabenliste und Aufgaben-Manager, die Benutzer bei der Erledigung von Aufgaben unterstützt.  Ganz gleich, ob Sie eine Einkaufsliste teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu erfassen, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die umgehende Synchronisierung zwischen Telefon, Tablet und Computer, sodass die Aufgaben auf allen Geräten zur Verfügung stehen.

Erstellen Sie zunächst eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-wunderlist"></a>Herstellen einer Verbindung mit Wunderlist
Um Logik-Apps mit Wunderlist zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für Wunderlist |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die Trigger verwenden.

> [!INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)]
> 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/wunderlist/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.