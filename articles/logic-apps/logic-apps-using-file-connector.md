---
title: "Herstellen einer Verbindung mit lokalen Dateisystemen – Azure Logic Apps | Microsoft-Dokumentation"
description: "Informationen zum Herstellen einer Verbindung mit lokalen Dateisystemen in Ihrem Logik-App-Workflow über das lokale Datengateway und den Dateisystem-Connector"
keywords: Dateisysteme, lokal
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: LADocs; deli
ms.openlocfilehash: 7738b3346af49cb8aa811eb17003d1b72b1bbe46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Herstellen einer Verbindung mit lokalen Dateisystemen in Logik-Apps mit dem Dateisystem-Connector

Sie können zum Verwalten von Daten und für den sicheren Zugriff auf lokale Ressourcen für Ihre Logik-Apps das lokale Datengateway verwenden. In diesem Artikel veranschaulichen wir das Herstellen einer Verbindung mit einem lokalen Dateisystem mithilfe eines einfachen Szenarios: Kopieren einer Datei, die in Dropbox hochgeladen wurde, in eine Dateifreigabe, und anschließendes Senden einer E-Mail.

## <a name="prerequisites"></a>Voraussetzungen

* Laden Sie das neueste [lokale Datengateway](https://www.microsoft.com/download/details.aspx?id=53127) herunter.

* Installieren Sie das neueste lokale Datengateway, Version 1.15.6150.1 oder höher, und richten Sie es ein. Anweisungen finden Sie unter [Herstellen einer Verbindung mit Datenquellen in der lokalen Umgebung](http://aka.ms/logicapps-gateway). Sie müssen das Gateway auf einem lokalen Computer installieren, bevor Sie mit diesen Schritten fortfahren können.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/logic-apps-create-a-logic-app.md)

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Hinzufügen eines Triggers und von Aktionen für die Verbindung mit dem Dateisystem

1. Erstellen einer leeren Logik-App Fügen Sie als ersten Schritt diesen Trigger hinzu: **Dropbox – Wenn eine Datei erstellt wird** 

2. Wählen Sie unter dem Trigger **+ Nächster Schritt** > **Aktion hinzufügen** aus. 

3. Geben Sie im Suchfeld „Dateisystem“ als Filter ein. Wenn alle Aktionen für den Dateisystem-Connector angezeigt werden, wählen Sie die Aktion **Dateisystem – Datei erstellen** aus. 

   ![Suchen nach „Dateiconnector“](media/logic-apps-using-file-connector/search-file-connector.png)

4. Wenn noch keine Verbindung mit Ihrem Dateisystem besteht, werden Sie aufgefordert, eine Verbindung herzustellen. 

5. Wählen Sie **Verbinden über lokales Datengateway**. Wenn die Verbindungseigenschaften angezeigt werden, richten Sie die Verbindung entsprechend den Angaben in der Tabelle ein.

   ![Konfigurieren der Verbindung](media/logic-apps-using-file-connector/create-file.png)

   | Einstellung | Beschreibung |
   | ------- | ----------- |
   | **Stammordner** | Geben Sie den Stammordner Ihres Dateisystems an. Sie können einen lokalen Ordner auf dem Computer angeben, auf dem das lokale Datengateway installiert ist, oder eine Netzwerkfreigabe, auf die der Computer Zugriff hat. <p>**Tipp:** Der Stammordner ist der übergeordnete Hauptordner, der bei allen dateibezogenen Aktionen für relative Pfade verwendet wird. | 
   | **Authentifizierungstyp** | Der Typ der Authentifizierung, die von Ihrem Dateisystem verwendet wird | 
   | **Benutzername** | Geben Sie Ihren Benutzernamen {*Domäne*\\*Benutzername*} für das zuvor installierte Gateway an. | 
   | **Kennwort** | Geben Sie Ihr Kennwort für das zuvor installierte Gateway an. | 
   | **Gateway** | Wählen Sie das zuvor installierte Gateway aus. | 
   ||| 

6. Wählen Sie **Erstellen** aus, nachdem Sie alle Verbindungsdetails angegeben haben. 

   Die Verbindung wird in Logic Apps konfiguriert und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert. 
   Wenn die Verbindung ordnungsgemäß eingerichtet ist, werden Optionen für die zuvor ausgewählte Aktion angezeigt. 
   Der Dateisystem-Connector kann jetzt verwendet werden.

7. Richten Sie die Aktion **Datei erstellen** zum Kopieren von Dateien aus Dropbox in den Stammordner auf Ihrer lokalen Dateifreigabe ein.

   ![Erstellen einer Dateiaktion](media/logic-apps-using-file-connector/create-file-filled.png)

8. Fügen Sie im Anschluss an diese Aktion zum Kopieren der Datei eine Outlook-Aktion hinzu, über die eine E-Mail zum Informieren entsprechender Benutzer über die neue Datei gesendet wird. Geben Sie die Empfänger, den Titel und Text der E-Mail ein. 

   In der Liste **Dynamische Inhalte** können Sie Datenausgaben im Datei-Connector auswählen und so der E-Mail weitere Details hinzufügen.

   ![Aktion „E-Mail senden“](media/logic-apps-using-file-connector/send-email.png)

9. Speichern Sie Ihre Logik-App. Testen Sie die App durch Hochladen einer Datei in Dropbox. Die Datei sollte in die lokale Dateifreigabe kopiert werden, und Sie sollten eine E-Mail zu diesem Vorgang erhalten.

Herzlichen Glückwunsch, Sie verfügen nun über eine funktionsfähige Logik-App, über die eine Verbindung mit dem lokalen Dateisystem hergestellt werden kann. 

Sie können nun weitere Funktionen des Connectors ausprobieren, beispielsweise:

- Datei erstellen
- Dateien im Ordner aufführen
- Datei anfügen
- Datei löschen
- Dateiinhalte abrufen
- Dateiinhalt anhand des Pfads abrufen
- Dateimetadaten abrufen
- Dateimetadaten anhand des Pfads abrufen
- Dateien im Stammordner aufführen
- Datei aktualisieren

## <a name="view-the-swagger"></a>Anzeigen von Swagger

Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/fileconnector/). 

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Azure Logic Apps-Website für Benutzerfeedback](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung mit lokalen Daten](../logic-apps/logic-apps-gateway-connection.md) 
* [Überwachen von Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Enterprise Integration für B2B-Szenarien](../logic-apps/logic-apps-enterprise-integration-overview.md)
