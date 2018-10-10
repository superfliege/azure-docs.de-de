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
ms.openlocfilehash: 2aec8ba969fb639f2d785a429441f6ed4bbf7dfb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987678"
---
# <a name="configure-your-hosted-ui-experience"></a>Konfigurieren der gehosteten Benutzeroberfläche

Nachdem Sie Ihre Instanz für die benutzerdefinierte Suche konfiguriert haben, können Sie die API für die benutzerdefinierte Suche aufrufen, um die Suchergebnisse abzurufen und in Ihrer App anzuzeigen. Wenn es sich bei Ihrer App um eine Web-App handelt, können Sie alternativ auch die gehostete Benutzeroberfläche der benutzerdefinierten Suche verwenden.   

## <a name="configure-custom-hosted-ui"></a>Konfigurieren der benutzerdefinierten gehosteten Benutzeroberfläche

Gehen Sie folgendermaßen vor, um eine gehostete Benutzeroberfläche für Ihre Web-App zu konfigurieren:

1. Melden Sie sich beim [Portal](https://customsearch.ai) für die benutzerdefinierte Suche an.  
  
2. Klicken Sie auf eine benutzerdefinierte Suchinstanz. Informationen zum Erstellen einer Instanz finden Sie unter [Erstellen der ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).  

3. Klicken Sie auf die Registerkarte **Gehostete Benutzeroberfläche**.  
  
4. Wählen Sie ein Layout aus.
  
  - Suchleiste und Ergebnisse (Standard) &mdash; dieses Layout ist die traditionelle Suchseite mit Suchfeld und den Suchergebnissen.
  - Nur Ergebnisse &mdash; dieses Layout zeigt nur die Suchergebnisse. Bei diesem Layout wird kein Suchfeld angezeigt. Sie müssen die Suchabfrage angeben, indem Sie der Anforderungs-URL im JavaScript-Codeausschnitt oder dem Link zum HTML-Endpunkt den Abfrageparameter (&q=\<Abfragezeichenfolge>) hinzufügen.
  - Einblendung im Vordergrund &mdash; dieses Layout stellt ein Suchfeld bereit und zeigt die Suchergebnisse in einer eingeblendeten Überlagerung an.
      
5. Wählen Sie ein Farbdesign aus. Die möglichen Designs sind: 
  
  - Klassisch
  - Dark
  - Skyline Blue

  Klicken Sie auf die Designs, um herauszufinden, welches Design am besten zu Ihrer Web-App passt. Wenn Sie das Farbdesign zur besseren Integration in Ihre Web-App optimieren müssen, klicken Sie auf **Design anpassen**. Nicht alle Farbkonfigurationen sind auf alle Layoutdesigns anwendbar. Um eine Farbe zu ändern, geben Sie den RGB-Hexadezimalwert der Farbe (z.B. #366eb8) in das entsprechende Textfeld ein. Sie können auch auf die Farbschaltfläche und dann auf die Schattierung, die für Sie geeignet ist, klicken. 
  
  Überprüfen Sie nach dem Ändern einer Farbe, wie sich die Änderung auf das Vorschaubeispiel auf der rechten Seite auswirkt. Sie können jederzeit auf **Auf Standard zurücksetzen** klicken, um wieder die Standardfarben für das ausgewählte Design zu verwenden.

  > [!NOTE]
  > Beachten Sie bei Änderungen am Farbschema die Barrierefreiheit, wenn Sie die Farben auswählen.

5. Geben Sie unter **Zusätzliche Konfigurationen** entsprechende Werte für Ihre App an. Diese Einstellungen sind optional. Die Auswirkungen der angewendeten oder entfernten Einstellungen sehen Sie im Vorschaubereich auf der rechten Seite. Folgende Konfigurationsoptionen stehen zur Verfügung:  
  
  - Konfigurationen für die Websuche:
    - Webergebnisse aktiviert &mdash; legt fest, ob die Websuche aktiviert ist (am oberen Rand der Seite wird die Registerkarte „Web“ angezeigt).
    - Vorschlagssuche aktivieren &mdash; legt fest, ob die Vorschlagssuche aktiviert ist (die Zusatzkosten finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)).
    - Webergebnisse pro Seite &mdash; die Anzahl gleichzeitig angezeigter Websuchergebnisse (max. 50 Ergebnisse pro Seite).
    - Bildtitel &mdash; bestimmt, ob Bilder mit Suchergebnissen angezeigt werden.
  
    Die folgenden Konfigurationen werden angezeigt, wenn Sie auf **Erweiterte Konfigurationen anzeigen** klicken.  
  
    - Wörter hervorheben &mdash; legt fest, ob Suchbegriffe in den Ergebnissen fett formatiert werden. 
    - Linkziel &mdash; legt fest, ob die Webseite auf einer neuen Browserregisterkarte („blank“) oder auf derselben Registerkarte („self“) geöffnet wird, wenn der Benutzer auf ein Suchergebnis klickt. 

  - Konfigurationen für die Bildersuche:
    - Bildergebnisse aktiviert &mdash; legt fest, ob die Bildersuche aktiviert ist (am oberen Rand der Seite wird die Registerkarte „Bilder“ angezeigt).   
    - Bildergebnisse pro Seite &mdash; die Anzahl gleichzeitig angezeigter Bildsuchergebnisse (max. 150 Ergebnisse pro Seite).  
  
    Die folgende Konfiguration wird angezeigt, wenn Sie auf **Erweiterte Konfigurationen anzeigen** klicken.  
  
    - Filter aktivieren &mdash; fügt Filter hinzu, mit denen der Benutzer die von Bing zurückgegebenen Bilder filtern kann. Beispielsweise kann der Benutzer die Ergebnisse nur nach animierten GIF-Dateien filtern.

  - Konfigurationen für die Videosuche:
    - Videoergebnisse aktiviert &mdash; legt fest, ob die Videosuche aktiviert ist (am oberen Rand der Seite wird die Registerkarte „Videos“ angezeigt).  
    - Videoergebnisse pro Seite &mdash; die Anzahl gleichzeitig angezeigter Videosuchergebnisse (max. 150 Ergebnisse pro Seite).
  
    Die folgende Konfiguration wird angezeigt, wenn Sie auf **Erweiterte Konfigurationen anzeigen** klicken.  
  
    - Filter aktivieren &mdash; fügt Filter hinzu, mit denen der Benutzer die von Bing zurückgegebenen Videos filtern kann. Beispielsweise kann der Benutzer die Ergebnisse nach Videos mit einer bestimmten Auflösung oder nach Videos, die in den letzten 24 Stunden gefunden wurden, filtern.

  - Sonstige Konfigurationen:
    - Seitentitel &mdash; Text, der im Titelbereich der Seite mit den Suchergebnissen (nicht beim Layout mit eingeblendeten Überlagerungen) angezeigt wird.
    - Symbolleistendesign &mdash; legt die Hintergrundfarbe des Titelbereichs der Seite mit den Suchergebnissen fest.  
  
    Die folgenden Konfigurationen werden angezeigt, wenn Sie auf **Erweiterte Konfigurationen anzeigen** klicken.  
  
    - Textplatzhalter für das Suchfeld &mdash; Text, der vor Beginn der Eingabe im Suchfeld angezeigt wird.
    - Titellink-URL &mdash; das Ziel für den Titellink.
    - Logo-URL &mdash; Bild, das neben dem Titel angezeigt wird. 
    - Favicon-URL &mdash; Symbol, das auf der Titelleiste des Browsers angezeigt wird.  

    Die folgenden Konfigurationen gelten nur, wenn Sie die gehostete Benutzeroberfläche über den HTML-Endpunkt verwenden (gilt nicht bei Verwendung des JavaScript-Codeausschnitts).
    
    - Seitentitel
    - Symbolleistendesign
    - Titellink-URL
    - Logo-URL
    - Favicon-URL  
  
6. Geben Sie den Abonnementschlüssel für die Suche ein, oder wählen Sie einen in der Dropdownliste aus. Die Dropdownliste enthält Schlüssel aus den Abonnements Ihres Azure-Kontos. Weitere Informationen finden Sie unter [Create a Cognitive Services APIs account in the Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal).  

7. Wenn Sie die Vorschlagssuche aktiviert haben, geben Sie den Abonnementschlüssel für die Vorschlagssuche ein, oder wählen Sie einen in der Dropdownliste aus. Die Dropdownliste enthält Schlüssel aus den Abonnements Ihres Azure-Kontos. Für die benutzerdefinierte Vorschlagssuche muss ein bestimmter Abonnementtarif verwendet werden. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Änderungen, die Sie an der Konfiguration der benutzerdefinierten gehosteten Benutzeroberfläche vornehmen, werden im Bereich auf der rechten Seite angezeigt. Bei den angezeigten Ergebnissen handelt es sich nicht um echte Ergebnisse für Ihre Instanz.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Nutzen der benutzerdefinierten Benutzeroberfläche

Die gehostete Benutzeroberfläche kann auf zwei Arten genutzt werden: 

- Schließen Sie das Skript in Ihre Webseite ein.  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Sie können auch die folgende URL in einem Webbrowser verwenden.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Fügen Sie nach Bedarf der URL die folgenden Abfrageparameter hinzu. Weitere Informationen zu diesen Parametern finden Sie in der Referenz zur [API für die benutzerdefinierte Suche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).
  >
  > - q
  > - mkt
  > - safesearch
  > - setlang

  > [!IMPORTANT]
  > Es ist nicht möglich, auf der Seite Ihre Datenschutzrichtlinie oder andere Hinweise/Bedingungen anzuzeigen. Die Eignung für den gewünschten Zweck kann variieren.  

Weitere Informationen (einschließlich Ihrer benutzerdefinierten Konfigurations-ID) finden Sie auf der Registerkarte **Produktion** unter **Endpunkte**.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Dekorationsmarkierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)