---
title: Verwenden des Slack-Connectors in Ihren Azure-Logik-Apps | Microsoft-Dokumentation
description: Stellen Sie in Ihren Logik-Apps eine Verbindung mit Slack her.
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 73c512c70f1c135bd791d93cecc42bd6f4c06b3d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-slack-connector"></a>Erste Schritte mit dem Slack-Connector
Slack ist ein Tool für die Kommunikation in Teams, das alle Kommunikationen in Ihrem Team zentral zusammenführt und unmittelbar durchsuchbar und überall verfügbar macht. 

Erstellen Sie zunächst eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-slack"></a>Herstellen einer Verbindung mit Slack
Stellen Sie zum Verwenden des Slack-Connectors zunächst eine **Verbindung** her, und geben Sie anschließend die Details für diese Eigenschaften an: 

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben von Slack-Anmeldeinformationen |

Führen Sie folgende Schritte aus, um sich bei Slack anzumelden und die Konfiguration der **Slack-Verbindung** in Ihrer Logik-App abzuschließen:

1. Wählen Sie **Wiederholung**
2. Wählen Sie eine **Häufigkeit** aus, und geben Sie ein **Intervall** an.
3. Wählen Sie **Aktion hinzufügen** aus.  
   ![Slack konfigurieren][1]  
4. Geben Sie in das Suchfeld „Slack“ ein, und warten Sie, bis die Suche alle Einträge mit Slack im Namen zurückgibt.
5. Wählen Sie **Slack – Nachricht veröffentlichen**
6. Wählen Sie **Bei Slack anmelden**:  
   ![Slack konfigurieren][2]
7. Geben Sie Ihre Slack-Anmeldeinformationen ein, um die Anwendung zu autorisieren.    
   ![Slack konfigurieren][3]  
8. Sie werden auf die Anmeldeseite Ihrer Organisation umgeleitet. **Autorisieren** Sie Slack für die Interaktion mit Ihrer Logik-App:      
   ![Slack konfigurieren][5] 
9. Nach Abschluss der Autorisierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können. Dazu konfigurieren Sie den Abschnitt **Slack – Alle Nachrichten abrufen**. Fügen Sie bei Bedarf weitere Trigger und Aktionen hinzu.  
   ![Slack konfigurieren][6]
10. Klicken Sie im Menü (im oberen Bereich) auf **Speichern**, um Ihre Arbeit zu speichern.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/slack/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
