---
title: 'Benutzerdefinierte Bing-Suche: Websitesuche | Microsoft-Dokumentation'
description: Beschreibt die Vorgehensweise zum Konfigurieren der gehosteten Benutzeroberfläche.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 593ea4d23f8ddcec8efc4be632afa2aab1a5210f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374731"
---
# <a name="configure-your-hosted-ui-experience"></a>Konfigurieren der gehosteten Benutzeroberfläche
Nachdem Sie Ihre Instanz für die benutzerdefinierte Suche konfiguriert haben, können Sie die API für die benutzerdefinierte Suche aufrufen, um die Suchergebnisse abzurufen und in Ihrer App anzuzeigen. Wenn es sich bei Ihrer App um eine Web-App handelt, können Sie alternativ auch die gehostete Benutzeroberfläche der benutzerdefinierten Suche verwenden.   

## <a name="configure-custom-hosted-ui"></a>Konfigurieren der benutzerdefinierten gehosteten Benutzeroberfläche
Gehen Sie wie folgt vor, um eine gehostete Benutzeroberfläche für Ihre Web-App zu konfigurieren.
1.  Melden Sie sich beim [Portal](https://customsearch.ai) für die benutzerdefinierte Suche an.
2.  Klicken Sie auf eine benutzerdefinierte Suchinstanz. Informationen zum Erstellen einer Instanz finden Sie unter [Erstellen der ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).
3.  Klicken Sie auf die Registerkarte **Gehostete Benutzeroberfläche**.
4.  Wählen Sie ein Layout aus.
    - „Search bar and results (default)“ (Suchleiste und Ergebnisse (Standard)): Zeigt ein Suchfeld und Suchergebnisse an.
    - „Results only“ (Nur Ergebnisse): Zeigt nur Ergebnisse an, aber kein Suchfeld.
    - „Pop-over“ (Einblendung im Vordergrund): Zeigt nur Ergebnisse als gleitende Überlagerung an, aber kein Suchfeld.
    
   > [!IMPORTANT]
   > Bei Verwendung des Layouts **Results only** (Nur Ergebnisse) muss der Abfrageparameter „customConfig“ eingeschlossen werden (siehe [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).

5.  Geben Sie unter **Zusätzliche Konfigurationen** entsprechende Werte für Ihre App an. Diese Einstellungen sind optional. Die Auswirkungen der angewendeten oder entfernten Einstellungen sehen Sie im Vorschaubereich auf der rechten Seite.  Folgende Konfigurationsoptionen stehen zur Verfügung:
    - Konfigurationen für die Websuche:
        - „Web results enabled“ (Webergebnisse aktiviert): Bestimmt, ob Ergebnisse der Websuche zurückgegeben werden.
        - „Enable autosuggest“ (Vorschlagssuche aktivieren): Bestimmt, ob die Vorschlagssuche aktiviert ist.
        - „Web results per page“ (Webergebnisse pro Seite): Die Anzahl gleichzeitig angezeigter Websuchergebnisse.
        - „Image caption“ (Bildtitel): Bestimmt, ob Bilder mit Suchergebnissen angezeigt werden.
        - „Highlight words“ (Wörter hervorheben): Bestimmt, ob Suchbegriffe in den Ergebnissen fett formatiert werden.
    - Konfigurationen für die Bildersuche:
        - „Image results enabled“ (Bildergebnisse aktiviert): Bestimmt, ob Ergebnisse der Bildersuche zurückgegeben werden.
    - Sonstige Konfigurationen:
        - „Page title“ (Seitentitel): Text, der im Seitentitelbereich angezeigt wird.
        - „Toolbar theme“ (Symbolleistendesign): Bestimmt die Hintergrundfarbe des Seitentitelbereichs.
        - „Search box text placeholder“ (Textplatzhalter für das Suchfeld): Text, der vor Beginn der Eingabe im Suchfeld angezeigt wird.
        - „Title link url“ (Titellink-URL): Das Ziel für den Titellink.
        - „Logo url“ (Logo-URL): Bild, das neben dem Titel angezeigt wird. 
        - „Favicon url“ (Favicon-URL): Symbol, das auf der Titelleiste des Browsers angezeigt wird.

   > [!IMPORTANT]
   > Es muss mindestens entweder die Bildersuche oder die Websuche aktiviert sein.

6.  Geben Sie den Abonnementschlüssel für die Suche ein, oder wählen Sie einen aus der Dropdownliste aus. Die Dropdownliste enthält Schlüssel aus den Azure-Abonnements Ihres Kontos. Weitere Informationen finden Sie unter [Create a Cognitive Services APIs account in the Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal).
7.  Wenn Sie die Vorschlagssuche aktiviert haben, geben Sie den Abonnementschlüssel für die Vorschlagssuche ein, oder wählen Sie einen aus der Dropdownliste aus. Die Dropdownliste enthält Schlüssel aus den Azure-Abonnements Ihres Kontos. Für die benutzerdefinierte Vorschlagssuche muss ein bestimmter Abonnementtarif verwendet werden. Weitere Informationen finden Sie auf den [Preisseiten](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Änderungen, die Sie an der Konfiguration der benutzerdefinierten gehosteten Benutzeroberfläche vornehmen, werden im Bereich auf der rechten Seite angezeigt. Bei den angezeigten Ergebnissen handelt es sich nicht um echte Ergebnisse für Ihre Instanz.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Nutzen der benutzerdefinierten Benutzeroberfläche
Die gehostete Benutzeroberfläche kann auf zwei Arten genutzt werden: 

- Schließen Sie das Skript in Ihre Webseite ein.
    ``` html
    <html>
        <body>
            <script type="text/javascript"
                id="bcs_js_snippet"            
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">            
            </script>
        </body>    
    </html>
    ```

- Verwenden Sie die bereitgestellte URL: `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > Es ist nicht möglich, auf der Seite Ihre Datenschutzrichtlinie oder andere Hinweise/Bedingungen anzuzeigen. Die Eignung für den gewünschten Zweck kann variieren.  

Weitere Informationen (einschließlich Ihrer benutzerdefinierten Konfigurations-ID) finden Sie auf der Registerkarte **Produktion** unter **Endpunkte**.

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden von Dekorationsmarkierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)