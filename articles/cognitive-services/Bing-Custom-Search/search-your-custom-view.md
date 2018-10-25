---
title: Suche nach einer benutzerdefinierten Ansicht – Benutzerdefinierte Bing-Suche
titlesuffix: Azure Cognitive Services
description: Beschreibt, wie Sie nach einer benutzerdefinierten Ansicht im Web suchen.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 15c5b3c58c4f3617111707ed82d031b67b6ad4c1
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465134"
---
# <a name="call-your-custom-search"></a>Aufrufen der benutzerdefinierten Suche

Sie benötigen einen Cognitive Services-Abonnementschlüssel, bevor Sie erstmals über die API für die benutzerdefinierte Suche Suchergebnisse für Ihre Instanz aufrufen können. Wie Sie einen Schlüssel für die API für die benutzerdefinierte Suche erhalten, erfahren Sie unter [Cognitive Services-Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).


## <a name="try-it-out"></a>Ausprobieren

Nachdem Sie Ihre benutzerdefinierte Suche konfiguriert haben, können Sie die Konfiguration über das Portal für die benutzerdefinierte Suche testen. 

1. Melden Sie sich bei der [benutzerdefinierten Suche](https://customsearch.ai) an.
2. Klicken Sie auf eine Instanz der benutzerdefinierten Suche in der Liste der Instanzen.
3. Klicken Sie auf die Registerkarte **Produktion**. 
4. Wählen Sie auf der Registerkarte **Endpunkte** einen Endpunkt aus (z.B. Web-API). Ihr Abonnement legt fest, welche Endpunkte angezeigt werden (Abonnementoptionen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)). 
5. Geben Sie die Parameterwerte an. 

    Im Folgenden werden die möglichen Parameter, die Sie festlegen können, angegeben (die tatsächliche Liste hängt vom ausgewählten Endpunkt ab). Zusätzliche Informationen zu diesen Parametern finden Sie in der Referenz zur [API für die benutzerdefinierte Suche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

    - **Abfrage**: der gesuchte Suchbegriff. Nur für Endpunkte für die Web-, Bilder-, Video- und Vorschlagssuche verfügbar.
    - **Benutzerdefinierte Konfigurations-ID**: die Konfigurations-ID der ausgewählten benutzerdefinierten Suchinstanz. Dieses Feld ist schreibgeschützt.
    - **Markt**: der Markt, aus dem die Ergebnisse stammen. Nur für Endpunkte für die Web-, Bilder- und Videosuche und die gehostete Benutzeroberfläche verfügbar.
    - **Abonnementschlüssel**: der Abonnementschlüssel zum Testen. Sie können einen Schlüssel in der Dropdownliste auswählen, oder einen Schlüssel manuell eingeben.  
      
    Wenn Sie auf **Zusätzliche Parameter** klicken, werden die folgenden Parameter angezeigt:  
      
    - **Sichere Suche**: ein Filter, der Webseiten nach jugendgefährdenden Inhalten durchsucht. Nur für Endpunkte für die Web-, Bilder- und Videosuche und die gehostete Benutzeroberfläche verfügbar.
    - **Sprache der Benutzeroberfläche:** die Sprache für Zeichenfolgen auf der Benutzeroberfläche. Wenn Sie beispielsweise Bilder und Videos in der gehosteten Benutzeroberfläche aktivieren, wird auf den Registerkarten **Bild** und **Video** die angegebene Sprache verwendet.
    - **Anzahl**: die Anzahl der Suchergebnisse, die bei der Antwort zurückgegeben werden. Nur für Endpunkte für die Web-, Bilder- und Videosuche verfügbar.
    - **Offset:** die Anzahl von Ergebnissen, die übersprungen werden sollen, bevor Ergebnisse zurückgegeben werden. Nur für Endpunkte für die Web-, Bilder- und Videosuche verfügbar.

6. Nachdem Sie alle erforderlichen Optionen angegeben haben, klicken Sie auf **Aufrufen**, um die JSON-Antwort im rechten Bereich anzuzeigen. 

Wenn Sie den Endpunkt für die gehostete Benutzeroberfläche auswählen, können Sie die Suche im unteren Bereich testen.

## <a name="next-steps"></a>Nächste Schritte

- [Aufrufen der benutzerdefinierten Ansicht mit C#](./call-endpoint-csharp.md)
- [Aufrufen der benutzerdefinierten Ansicht mit Java](./call-endpoint-java.md)
- [Aufrufen der benutzerdefinierten Ansicht mit NodeJs](./call-endpoint-nodejs.md)
- [Aufrufen der benutzerdefinierten Ansicht mit Python](./call-endpoint-python.md)

- [Aufrufen der benutzerdefinierten Ansicht mit dem C# SDK](./sdk-csharp-quick-start.md)