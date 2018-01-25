---
title: Wunderlist-Connector in Azure Logic Apps | Microsoft-Dokumentation
description: Stellen Sie eine Verbindung mit Wunderlist her, und verwenden Sie diese Verbindung zum Erstellen Ihres Workflows in Logic Apps.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3657955ca4280fecd3a0fb1ea64b90e0a5c5c765
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-wunderlist-connector"></a>Erste Schritte mit dem Wunderlist-Connector
Wunderlist ist gleichzeitig eine Aufgabenliste und ein Aufgabenmanager und erleichtert es Menschen, ihren Alltag zu meistern.  Ob Sie eine Einkaufsliste mit Ihrem Partner teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu notieren, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die Synchronisierung zwischen Telefon, Tablet und Computer – die Aufgaben sind auf allen Geräten sofort zur Hand.

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