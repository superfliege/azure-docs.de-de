---
title: Herstellen einer Verbindung mit GitHub – Azure Logic Apps | Microsoft-Dokumentation
description: Überwachen von GitHub-Ereignissen mit GitHub-REST-APIs und Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: ce567dc631c3a147b795eb2355a4961faa8881d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295811"
---
# <a name="connect-to-github"></a>Herstellen einer Verbindung mit GitHub

GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Darin finden Sie die gesamte verteilte Versionskontroll- und SCM-Funktionalität (Source Code Management, Quellcodeverwaltung) in Git sowie weitere Features.

Um mit der Verwendung des GitHub-Connectors loszulegen, [erstellen Sie zunächst eine Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Herstellen einer Verbindung mit GitHub

Um den GitHub-Connector in einer Logik-App verwenden zu können, müssen Sie zuerst eine *Verbindung* erstellen, und dann Details für diese Eigenschaften angeben: 

| Eigenschaft | Erforderlich | BESCHREIBUNG | 
| -------- | -------- | ----------- | 
| Tokenverschlüsselung | Ja | Geben Sie Ihre Anmeldeinformationen für GitHub an. |

Nachdem Sie die Verbindung erstellt haben, können Sie die Aktionen ausführen und auf die in diesem Artikel beschriebenen Trigger lauschen.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Lesen Sie bei den in Swagger definierten Triggern und Aktionen sowie Beschränkungen in den [Connectordetails](/connectors/github/) an.

## <a name="find-more-connectors"></a>Weitere Connectors

* Überprüfen Sie die [Liste der Connectors](apis-list.md).