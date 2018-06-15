---
title: Verwenden des SharePoint Server-Connectors in Ihren Logik-Apps | Microsoft-Dokumentation
description: Erste Schritte mit dem SharePoint Server-Connector in Ihren Logik-Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: c153b4987e37a5d97b95d4f1249de1ed92e851b3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295943"
---
# <a name="get-started-with-the-sharepoint-connector"></a>Erste Schritte mit dem SharePoint-Connector
Der SharePoint-Connector ermöglicht die Verwendung von Listen in SharePoint.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-sharepoint"></a>Herstellen einer Verbindung mit SharePoint
Erstellen Sie zum Verwenden des SharePoint-Connectors zunächst eine **Verbindung**, und geben Sie anschließend die Details für die folgenden Eigenschaften an: 

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Bereitstellen von SharePoint-Anmeldeinformationen |

Um eine Verbindung mit **SharePoint** herzustellen, geben Sie Ihre Identität (Benutzername und Kennwort, Smartcard-Anmeldeinformationen usw.) ein. Nach der Authentifizierung können Sie den SharePoint-Connector in Ihrer Logik-App verwenden. 

Führen Sie im Designer Ihrer Logik-App die folgenden Schritte aus, um sich anzumelden und die **Verbindung** für Ihre Logik-App zu erstellen:

1. Geben Sie in das Suchfeld „SharePoint“ ein, und warten Sie, bis die Suche alle Einträge mit „SharePoint“ im Namen zurückgibt:    
   ![SharePoint konfigurieren][1]  
2. Wählen Sie **SharePoint – Wenn eine Datei erstellt wird** aus.   
3. Wählen Sie **Bei SharePoint anmelden** aus:   
   ![SharePoint konfigurieren][2]    
4. Geben Sie Ihre SharePoint-Anmeldeinformationen ein, um sich bei SharePoint zu authentifizieren.   
   ![SharePoint konfigurieren][3]     
5. Nach Abschluss der Authentifizierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können, indem Sie das SharePoint-Dialogfeld **Wenn eine Datei erstellt wird** konfigurieren.          
   ![SharePoint konfigurieren][4]  
6. Sie können dann weitere Trigger und Aktionen hinzufügen, die Sie benötigen, um Ihre Logik-App abzuschließen.   
7. Klicken Sie im Menü (im oberen Bereich) auf **Speichern**, um Ihre Arbeit zu speichern.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/sharepoint/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
