---
title: Herstellen einer Verbindung zwischen GitHub und Azure Logic Apps | Microsoft-Dokumentation
description: Automatisieren von Workflows für GitHub mit Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/02/2018
ms.author: estfan; ladocs
ms.openlocfilehash: 830c3e6b5462c5d2d917388c0d0924e444342f93
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
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