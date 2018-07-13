---
title: 'Benutzerdefinierte Bing-Suche: Suchen nach einer benutzerdefinierten Ansicht | Microsoft-Dokumentation'
description: Suchen nach einer benutzerdefinierten Ansicht im Web
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 75f6c8d299c7eed901dda0631fca74b040f72e30
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374698"
---
# <a name="call-your-custom-search"></a>Aufrufen der benutzerdefinierten Suche
Sie benötigen einen Cognitive Services-Abonnementschlüssel, bevor Sie erstmals über die API für die benutzerdefinierte Suche Suchergebnisse für Ihre Instanz aufrufen können. Wie Sie einen Schlüssel für die API für die benutzerdefinierte Suche erhalten, erfahren Sie unter [Cognitive Services-Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).

> [!NOTE]
> Bestehende Kunden der benutzerdefinierten Bing-Suche, die am oder vor dem 15. Oktober 2017 einen Schlüssel für eine Vorschauversion bereitgestellt haben, können ihre Schlüssel bis zum 30. November 2017 nutzen bzw. bis sie die maximal zulässige Anzahl von Abfragen ausgeschöpft haben. Danach müssen sie zur allgemein verfügbaren Version in Azure migrieren.

## <a name="try-it-out"></a>Ausprobieren
Nachdem Sie Ihre benutzerdefinierte Suche konfiguriert haben, können Sie die Konfiguration über das Portal für die benutzerdefinierte Suche testen. Melden Sie sich in der [benutzerdefinierten Suche](https://customsearch.ai) an, klicken Sie auf eine benutzerdefinierte Suchinstanz und dann auf die Registerkarte **Produktion**. Die Registerkarte **Endpunkte** wird angezeigt. Ihr Abonnement bestimmt, welche Endpunkte Sie testen können. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/). Um einen Endpunkt zu testen, wählen Sie diesen in der Dropdownliste aus, und legen Sie die zugehörigen Konfigurationsoptionen fest. 

Folgende Optionen sind verfügbar:

- **Abfrage**: der gesuchte Suchbegriff. Nur für Endpunkte für das Web, Bilder und die Vorschlagssuche verfügbar.
- **Benutzerdefinierte Konfigurations-ID**: die Konfigurations-ID der ausgewählten benutzerdefinierten Suchinstanz. Dieses Feld ist schreibgeschützt.
- **Markt**: der Markt, aus dem die Ergebnisse stammen. Nur für Endpunkte für das Web, Bilder und gehostete Benutzeroberflächen verfügbar.
- **Abonnementschlüssel**: der Abonnementschlüssel zum Testen. Sie können einen Schlüssel in der Dropdownliste auswählen, oder einen Schlüssel manuell eingeben.
- **Sichere Suche**: ein Filter, der Webseiten nach jugendgefährdenden Inhalten durchsucht. Nur für Endpunkte für das Web, Bilder und gehostete Benutzeroberflächen verfügbar.
- **Anzahl**: die Anzahl der Suchergebnisse, die bei der Antwort zurückgegeben werden. Nur für Web- und Bildendpunkte verfügbar.
- **Offset**: die Anzahl der Suchergebnisse, die bei der Antwort zurückgegeben werden. Nur für Web- und Bildendpunkte verfügbar.

Nachdem Sie alle erforderlichen Optionen für das Web, Bilder oder die Vorschlagssuche angegeben haben, klicken Sie auf **Aufrufen**, um die JSON-Antwort im rechten Bereich anzuzeigen. 

Wenn Sie den Endpunkt für die gehostete Benutzeroberfläche auswählen, können Sie die Suche im rechten Bereich testen.

## <a name="next-steps"></a>Nächste Schritte
- [Aufrufen der benutzerdefinierten Ansicht mit C#](./call-endpoint-csharp.md)
- [Aufrufen der benutzerdefinierten Ansicht mit Java](./call-endpoint-java.md)
- [Aufrufen der benutzerdefinierten Ansicht mit NodeJs](./call-endpoint-nodejs.md)
- [Aufrufen der benutzerdefinierten Ansicht mit Python](./call-endpoint-python.md)