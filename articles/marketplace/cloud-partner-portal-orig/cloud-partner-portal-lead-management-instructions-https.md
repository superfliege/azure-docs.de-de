---
title: HTTPS-Endpunkt | Microsoft-Dokumentation
description: Konfigurieren der Leadverwaltung für HTTPS.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: fd13a7281c7e8702fd199364261ebcd458db0555
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806023"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Konfigurieren der Leadverwaltung zur Verwendung eines HTTPS-Endpunkts

Sie können einen HTTPS-Endpunkt verwenden, um Azure Marketplace- und AppSource-Leads zu verwalten, die in ein CRM-System geschrieben werden können. In diesem Artikel ist beschreiben, wie die Leadverwaltung so konfiguriert wird, dass der Microsoft Flow-Automatisierungsdienst verwendet wird.


## <a name="create-a-flow-using-microsoft-flow"></a>Erstellen eines Flows mit Microsoft Flow

1.  Öffnen Sie die [Flow](https://flow.microsoft.com/)-Webseite. Wählen Sie **Anmelden** aus, oder wählen Sie **Kostenlos registrieren** aus, um ein kostenloses Flow-Konto zu erstellen.

2.  Melden Sie sich an, und wählen Sie **Meine Flows** in der Menüleiste aus.

    ![Meine Workflows](./media/cloud-partner-portal-lead-management-instructions-https/image001.png)

3.  Wählen Sie **Ohne Vorlage erstellen** aus.

    ![Ohne Vorlage erstellen](./media/cloud-partner-portal-lead-management-instructions-https/image003.png)


4.  Wählen Sie den Connector **Anforderung/Antwort** aus, und suchen Sie dann nach dem Anforderungstrigger. 

    ![Ohne Vorlage erstellen](./media/cloud-partner-portal-lead-management-instructions-https/image005.png)

5. Wählen Sie den Trigger **Anforderung** aus.
    ![Anforderungstrigger](./media/cloud-partner-portal-lead-management-instructions-https/image007.png)


6.  Kopieren Sie das **JSON-Beispiel**, das sich am Ende dieses Artikels befindet, in das **JSON-Schema für Anforderungstext**.

7.  Fügen Sie einen neuen Schritt hinzu, und wählen Sie das gewünschte CRM-System mit der Aktion zum Erstellen eines neuen Eintrags aus. Im nächsten Bildschirmfoto ist **Dynamics 365 – Neuen Datensatz erstellen** als Beispiel gezeigt.

    ![Create a new record (Neuen Datensatz erstellen)](./media/cloud-partner-portal-lead-management-instructions-https/image009.png)

8.  Geben Sie die Verbindungseingaben für Ihren Connector ein, und wählen die Entität **Leads** aus.

    ![Leads auswählen](./media/cloud-partner-portal-lead-management-instructions-https/image011.png)

9.  Flow zeigt ein Formular zum Angeben von Leadinformationen an. Sie können Elemente aus der Eingabeanforderung zuordnen, indem Sie auswählen, dass dynamischer Inhalt hinzugefügt werden soll.

    ![Dynamischen Inhalt hinzufügen](./media/cloud-partner-portal-lead-management-instructions-https/image013.png)

10.  Ordnen Sie die gewünschten Felder zu, und wählen Sie dann **Speichern** aus, um den Flow zu speichern.

11. Es wird eine HTTP-POST-URL in der Anforderung erstellt. Kopieren Sie diese URL, und verwenden Sie sie als HTTPS-Endpunkt.

    ![HTTP-POST-URL](./media/cloud-partner-portal-lead-management-instructions-https/image015.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Konfigurieren Sie Ihr Angebot so, dass Leads an den HTTPS-Endpunkt gesendet werden

Wenn Sie die Informationen zur Leadverwaltung für Ihr Angebot konfigurieren, wählen Sie **HTTPS-Endpunkt** als „Leadziel“ aus, und fügen Sie die HTTP-POST-URL ein, die Sie im vorherigen Schritt kopiert haben.  

![Dynamischen Inhalt hinzufügen](./media/cloud-partner-portal-lead-management-instructions-https/image017.png)

Wenn Leads generiert werden, sendet Microsoft diese Leads an den Flow, der an das CRM-System weitergeleitet wird, das Sie konfiguriert haben.


## <a name="json-example"></a>JSON-Beispiel

``` json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```
