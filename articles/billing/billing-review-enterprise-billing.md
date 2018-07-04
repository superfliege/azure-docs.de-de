---
title: Überprüfen von Abrechnungsdaten der Azure Enterprise-Registrierung mit REST-API | Microsoft Docs
description: Erfahren Sie, wie Sie Azure-REST-APIs zum Überprüfen von Abrechnungsinformationen der Enterprise-Registrierung verwenden.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: 046b2e31aaefa5916a42b3652f9e6a8fdceff367
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064397"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Überprüfen von Abrechnungsdaten der Enterprise-Registrierung mit REST-APIs

Mit Azure-Berichterstellungs-APIs können Sie Ihre Azure-Kosten überprüfen und verwalten.

Hier erfahren Sie, wie Sie die einer Enterprise-Kontoregistrierung zugeordnete aktuelle Rechnung abrufen.

Gehen Sie wie folgt vor, um die aktuelle Rechnung abzurufen:
``` http
GET https://consumption.azure.com/v2/enrollments/{enrollmentID}/usagedetails
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Erstellen der Anforderung  

Der Parameter `{enrollmentID}` ist erforderlich und sollte die Registrierungs-ID für das Enterprise-Konto (Enterprise Account, EA) enthalten.

Die folgenden Header sind erforderlich: 

|Anforderungsheader|BESCHREIBUNG|  
|--------------------|-----------------|  
|*Inhaltstyp*:|Erforderlich. Legen Sie diese Option auf `application/json` fest.|  
|*Autorisierung*:|Erforderlich. Legen Sie diese Option auf einen gültigen `Bearer`-[API-Schlüssel](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) fest. |  

In diesem Beispiel wird ein synchroner Aufruf gezeigt, der Details für den aktuellen Abrechnungszyklus zurückgibt. Aus Leistungsgründen geben synchrone Aufrufe Informationen des letzten Monats zurück.  Sie können auch die [API asynchron](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) aufrufen, um Daten von 36 Monaten zurückzugeben.


## <a name="response"></a>response  

Bei einer erfolgreichen Antwort wird der Statuscode 200 (OK) zurückgegeben, der eine Liste der detaillierten Kosten für Ihr Konto enthält.

``` json
{
    "id": "${id}",
    "data": [
        {
            "cost": ${cost}, 
            "departmentId": ${departmentID},
            "subscriptionGuid" : ${subscriptionGuid} 
            "date": "${date}",
            "tags": "${tags}",
            "resourceGroup": "${resourceGroup}"
        } // ...
    ],
    "nextLink": "${nextLinkURL}"
}
```  

Jedes Element in **data** stellt eine Gebühr dar:

|Response-Eigenschaft|BESCHREIBUNG|
|----------------|----------|
|**cost** | Der in Rechnung gestellte Betrag in einer für den Standort des Rechenzentrums geeigneten Währung. |
|**subscriptionGuid** | Die global eindeutige ID des Abonnements. | 
|**departmentId** | Die ID der Abteilung, sofern vorhanden. |
|**date** | Das Datum, an dem die Gebühr in Rechnung gestellt wurde. |
|**Tags** | JSON-Zeichenfolge mit Tags, die dem Abonnement zugeordnet sind. |
|**Ressourcengruppe**|Der Name der Ressourcengruppe, die das Objekt enthält, für das die Kosten anfallen. |
|**nextLink**| Gibt bei entsprechender Festlegung eine URL für die nächste „Seite“ mit Details an. Leer, wenn es sich um die letzte Seite handelt. |  
||
  
Abteilungs-IDs, Ressourcengruppen, Tags und verwandte Felder werden vom EA-Administrator definiert.  

Dieses Beispiel ist abgekürzt. Eine vollständige Beschreibung der einzelnen Antwortfelder finden Sie unter [Abrufen von Nutzungsdetails](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail). 

Andere Statuscodes geben Fehlerbedingungen an. In diesen Fällen erläutert das Antwortobjekt, warum ein Fehler bei der Anforderung aufgetreten ist.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Nächste Schritte 
- Sehen Sie sich die [Übersicht über Unternehmensberichte](https://docs.microsoft.com/azure/billing/billing-enterprise-api) an.
- Erkunden Sie die [Enterprise-Abrechnungs-REST-API](https://docs.microsoft.com/rest/api/billing/).   
- [Erste Schritte mit der Azure-REST-API](https://docs.microsoft.com/rest/api/azure/)   
