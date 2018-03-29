---
title: Informationen zum Verwenden des Twitter-Connectors in Logik-Apps | Microsoft Docs
description: Übersicht über den Twitter-Connector mit REST-API-Parametern
services: ''
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: eb953ee7701d407b9b75a0699f53b9b64828a0e5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-twitter-connector"></a>Erste Schritte mit dem Twitter-Connector
Der Twitter-Connector ermöglicht Folgendes:

* Tweets posten und abrufen
* Auf Timelines, Freunde und Follower zugreifen
* Andere Aktionen und Trigger ausführen, die weiter unten in diesem Artikel beschrieben sind

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.  

## <a name="connect-to-twitter"></a>Verbinden mit Twitter
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-twitter"></a>Erstellen einer Verbindung mit Twitter
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Verwenden eines Twitter-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

In diesem Beispiel verwenden Sie den Trigger **Wenn ein neuer Tweet gepostet wird**, um nach #Seattle zu suchen. Wenn #Seattle gefunden wird, aktualisieren Sie eine Datei in Dropbox mit dem Text aus dem Tweet. In einem Unternehmensbeispiel könnten Sie nach dem Namen Ihres Unternehmens suchen und eine SQL-Datenbank mit dem Text aus dem Tweet aktualisieren.

1. Geben Sie *Twitter* im Suchfeld im Designer für Logik-Apps ein, und wählen Sie dann den Trigger **Twitter – Wenn ein neuer Tweet gepostet wird** aus.   
   ![Twitter-Trigger – Abbildung 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Geben Sie *#Seattle* in das Steuerelement **Suchtext** ein.  
   ![Twitter-Trigger – Abbildung 2](./media/connectors-create-api-twitter/trigger-2.png) 

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet. 

> [!NOTE]
> Eine Logik-App muss mindestens einen Trigger und eine Aktion enthalten. Verwenden Sie die Schritte im nächsten Abschnitt, um eine Aktion hinzufügen.

## <a name="add-a-condition"></a>Bedingung hinzufügen
Wir sind nur an Tweets von Benutzern mit mehr als 50 Followern interessiert. Daher wird der Logik-App zuerst eine Bedingung hinzugefügt, mit der die Anzahl von Followern bestätigt wird.  

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn #Seattle in einem neuen Tweet gefunden wird.  
   ![Twitter-Aktion – Abbildung 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Wählen Sie den Link **Bedingung hinzufügen** aus.  
   ![Twitter-Bedingung – Abbildung 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Daraufhin wird das Steuerelement **Bedingung** geöffnet, in dem Sie Bedingungen wie z.B. *ist gleich*, *ist kleiner als*, *ist größer als* und *enthält* auswählen können.  
   ![Twitter-Bedingung – Abbildung 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Wählen Sie das Steuerelement **Wert auswählen** aus. Bei diesem Steuerelement können Sie eine oder mehrere Eigenschaften aus vorherigen Aktionen oder Triggern auswählen. Die Bedingung dieses Eigenschaftswerts wird als „true“ oder „false“ ausgewertet.
   ![Twitter-Bedingung – Abbildung 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Wählen Sie **...** aus, um die Liste der Eigenschaften zu erweitern, damit Sie alle verfügbaren Eigenschaften sehen können.        
   ![Twitter-Bedingung – Abbildung 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Wählen Sie die Eigenschaft **Anzahl von Followern** aus.    
   ![Twitter-Bedingung – Abbildung 5](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Beachten Sie, dass die Eigenschaft für die Anzahl von Followern jetzt in das Steuerelement für den Wert aufgenommen wurde.    
   ![Twitter-Bedingung – Abbildung 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Wählen Sie **ist größer als** aus der Liste der Operatoren aus.    
   ![Twitter-Bedingung – Abbildung 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. Geben Sie „50“ als Operanden für den Operator *ist größer als* ein.  
   Die Bedingung wird hinzugefügt. Speichern Sie Ihre Arbeit, indem Sie im Menü den Link **Speichern** verwenden.    
   ![Twitter-Bedingung – Abbildung 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Verwenden einer Twitter-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Da jetzt ein Trigger vorhanden ist, können Sie eine Aktion hinzufügen, mit der ein neuer Tweet mit dem Inhalt der über den Trigger gefundenen Tweets gepostet wird. Bei dieser exemplarischen Vorgehensweise werden nur die Tweets von Benutzern mit mehr als 50 Followern gepostet.  

Im nächsten Schritt fügen Sie eine Twitter-Aktion hinzu. Diese Aktion postet Tweets auf der Grundlage von Eigenschaften aller Tweets, die von Benutzern mit jeweils mehr als 50 Followern gepostet werden.  

1. Wählen Sie **Aktion hinzufügen**aus. Mit diesem Schritt wird das Steuerelement für die Suche geöffnet, in dem Sie nach anderen Aktionen und Triggern suchen können.  
   ![Twitter-Bedingung – Abbildung 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Geben Sie *Twitter* in das Suchfeld ein, und wählen Sie die Aktion **Twitter - Tweet posten** aus. Mit diesem Schritt wird das Steuerelement **Tweet posten** geöffnet, in das Sie alle Details für den zu postenden Tweet eingeben.      
   ![Twitter-Aktion – Abbildung 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Wählen Sie das Steuerelement **Tweettext** aus. Alle Ausgaben früherer Aktionen und Trigger in der Logik-App werden jetzt angezeigt. Sie können beliebige Ausgaben auswählen und als Teil des Tweettexts des neuen Tweets verwenden.     
   ![Twitter-Aktion – Abbildung 2](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Wählen Sie **Benutzername** aus.   
5. Geben Sie im Steuerelement für den Tweettext direkt nach dem Benutzernamen *sagt:* ein.
6. Wählen Sie *Tweettext* aus.       
   ![Twitter-Aktion – Abbildung 3](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. Speichern Sie Ihre Arbeit, und senden Sie einen Tweet mit dem Hashtag #Seattle, um den Workflow zu aktivieren.


## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/twitterconnector/) an. 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)