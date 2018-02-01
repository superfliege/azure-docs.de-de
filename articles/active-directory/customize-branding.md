---
title: "Anpassen der Anmeldeseite für Ihren Azure Active Directory-Mandanten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie der Anmeldeseite ein Unternehmensbranding hinzufügen."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/19/2018
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 03a6b82f769ed9a36c5d3ff9934de75d1536e1ae
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Schnellstart: Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in Azure AD
Um Verwirrung zu vermeiden, möchten viele Unternehmen über alle verwalteten Websites und Dienste hinweg für ein einheitliches Erscheinungsbild sorgen. Dies ist in Azure Active Directory (Azure AD) möglich, da Sie die Darstellung der Anmeldeseite mit Ihrem Firmenlogo und Ihren benutzerdefinierten Farbschemas anpassen können. Die Anmeldeseite wird angezeigt, wenn Sie sich bei einer webbasierten Anwendung wie Office 365 anmelden, die Azure AD als Identitätsanbieter verwendet. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

> [!NOTE]
> * Unternehmensbranding ist nur verfügbar, wenn Sie die Premium- oder Basic-Lizenz für Azure AD erworben haben oder über eine Office 365-Lizenz verfügen. Informationen dazu, ob bestimmte Features für Ihren Lizenztype unterstützt werden, finden Sie auf der Seite [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Die Azure AD-Editionen Premium und Basic stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure AD-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Azure-Dienst nicht unterstützt. Sollten Sie weitere Informationen benötigen, können Sie sich über das [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/) mit uns in Verbindung setzen.

## <a name="customizing-the-sign-in-page"></a>Anpassen der Anmeldeseite

<!--You can customize the following elements on the sign-in page: <attach image>-->

Anpassungen des Unternehmensbrandings werden auf der Anmeldeseite von Azure AD angezeigt, wenn Benutzer auf eine mandantenspezifische URL zugreifen, z.B. [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Beim Besuch von www.office.com werden Benutzern beispielsweise keine unternehmensspezifischen Anpassungen angezeigt, da der Benutzer noch keine Anmeldeinformationen eingegeben hat. Das Unternehmensbranding wird erst angezeigt, nachdem der Benutzer seine Benutzer-ID eingegeben oder auf eine Benutzerkachel geklickt hat.

> [!NOTE]
> * Ihr Domänenname muss im Abschnitt **Domänen** des Azure-Portals, in dem Sie das Branding konfiguriert haben, als „Aktiv“ angezeigt werden. Weitere Informationen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens](add-custom-domain.md).
> * Das Branding der Anmeldeseite wird nicht auf die Anmeldeseite für persönliche Microsoft-Konten übernommen. Wenn sich Ihre Mitarbeiter oder Gäste des Unternehmens mit einem persönlichen Microsoft-Konto anmelden, wird auf der Anmeldeseite das Branding Ihres Unternehmens nicht angezeigt.


### <a name="banner-logo"></a>Bannerlogo 

BESCHREIBUNG | Einschränkungen | Empfehlungen
------- | ------- | ----------
Das Bannerlogo wird auf der Anmelde- und den Zugriffsbereichsseiten angezeigt.<br>Auf der Anmeldeseite wird das Logo nach Eingabe des Benutzernamens angezeigt. | Transparente JPG- oder PNG-Datei<br>Max. Höhe: 36 px<br>Max. Breite: 245 px | Verwenden Sie hier das Logo Ihres Unternehmens.<br>Verwenden Sie ein transparentes Bild. Setzen Sie nicht voraus, dass der Hintergrund weiß ist.<br>Fügen Sie keinen Abstand um Ihr Logo herum in der Abbildung ein, da Ihr Logo anderenfalls unverhältnismäßig klein aussehen wird.

### <a name="username-hint"></a>Hinweis auf den Benutzernamen   
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Diese Option passt den Hinweistext im Feld für den Benutzernamen an. | Unicode-Text, bis zu 64 Zeichen<br>Nur-Text | Falls sich voraussichtlich Gastbenutzer von außerhalb Ihrer Organisation bei der App anmelden, sollten Sie diese Option nicht festlegen.
            
### <a name="sign-in-page-text"></a>Text der Anmeldeseite   
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Diese Option wird am unteren Rand des Anmeldeformulars angezeigt und kann verwendet werden, um zusätzliche Informationen wie etwa die Telefonnummer Ihres Helpdesks oder einen rechtlichen Hinweis bereitzustellen. | Unicode-Text, bis zu 256 Zeichen<br>Ausschließlich Nur-Text (keine Links oder HTML-Tags)    

### <a name="sign-in-page-image"></a>Bild der Anmeldeseite  
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Diese Option wird im Hintergrund der Anmeldeseite angezeigt, ist in der Mitte des sichtbaren Bereichs verankert und wird so skaliert und zugeschnitten, dass das gesamte Browserfenster ausgefüllt ist.    <br>Auf schmalen Bildschirmen, z.B. von Mobiltelefonen, wird dieses Bild nicht angezeigt.<br>Beim Laden der Seite wird eine schwarze Maske mit einer Deckkraft von 0,55 auf dieses Bild angewendet. | JPG oder PNG<br>Bildmaße: 1.920 x 1.080 px<br>Dateigröße: &lt; 300 KB | <br>Verwenden Sie Bilder, in denen die Bildschärfe sich nicht auf einen Bereich konzentriert. Das nicht transparente Anmeldeformular wird über dem Mittelpunkt dieses Bilds angezeigt und kann abhängig von der Größe des Browserfensters einen beliebigen Teil des Bilds einnehmen.<br>Verwenden Sie keine große Datei, um kurze Ladezeiten zu gewährleisten. 

### <a name="sign-in-page-background-color"></a>Hintergrundfarbe auf der Anmeldeseite
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Bei Verbindungen mit geringer Bandbreite wird diese Farbe anstelle des Hintergrundbildes verwendet. | RGB-Farbe im Hexadezimalformat (Beispiel: #FFFFFF) | Sie sollten die primäre Farbe des Bannerlogos oder die Farbe Ihrer Organisation verwenden.

### <a name="square-logo-image"></a>Bild für quadratisches Logo
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Dieses Bild wird während der Einrichtung für neue PCs unter Windows 10 Enterprise angezeigt. Es bietet Kontext für Mitarbeiter, die ihren neuen Arbeits-PC einrichten. Das Bild wird für Mandanten angezeigt, die ihre Arbeitsgeräte mithilfe von [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) einrichten, sowie auf Kennworteingabeseiten in anderen Windows 10-Umgebungen. | Transparentes PNG-Format (bevorzugt) oder JPG<br>Bildmaße: 240 x 240 px<br>Dateigröße: &lt; 10 KB | Verwenden Sie hier das Logo Ihres Unternehmens.<br> Verwenden Sie ein transparentes Bild.<br>Setzen Sie nicht voraus, dass der Hintergrund weiß ist.<br>Fügen Sie Ihrem Logo im Bild keinen Abstand hinzu. Andernfalls sieht Ihr Logo unverhältnismäßig klein aus.

### <a name="show-option-to-remain-signed-in"></a>Anzeigen der Option, angemeldet zu bleiben
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Die Azure AD-Anmeldung gibt Benutzern die Möglichkeit, angemeldet zu bleiben, wenn sie den Browser schließen und erneut öffnen. Diese Option wird durch diese Einstellung ausgeblendet.<br>Legen Sie diese Einstellung auf **Nein** fest, um die Option für Ihre Benutzer auszublenden. | &nbsp; | Das Ausblenden der Option hat keinen Einfluss auf die Lebensdauer der Sitzung.<br>Einige Features von SharePoint Online und Office 2010 setzen voraus, dass Benutzer wählen können, angemeldet zu bleiben. Wenn Sie diese Option auf **Nein** festlegen, werden Benutzern unter Umständen zusätzliche und unerwartete Anmeldeaufforderungen angezeigt.

> [!NOTE]
> Alle Elemente sind optional. Wenn Sie beispielsweise ein Bannerlogo ohne Hintergrundbild angeben, werden auf der Anmeldeseite Ihr Logo und das Hintergrundbild für die Zielwebsite (z.B. Office 365) angezeigt.

## <a name="add-company-branding-to-your-directory"></a>Hinzufügen eines Unternehmensbrandings zu Ihrem Verzeichnis

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators für den Mandanten an.
2. Klicken Sie auf **Benutzer und Gruppen** > **Unternehmensbranding** > **Bearbeiten**.
  
  ![Öffnen des benutzerdefinierten Brandings](./media/customize-branding/navigation-to-branding.png)
3. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
  
  ![Bearbeiten des benutzerdefinierten Brandings](./media/customize-branding/edit-branding.png)
5. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Hinzufügen eines sprachspezifischen Unternehmensbrandings zu Ihrem Verzeichnis

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators für das Verzeichnis an.
2. Klicken Sie auf **Benutzer und Gruppen** > **Unternehmensbranding** > **Neue Sprache**.
  
  ![Hinzufügen sprachspezifischer Brandingelemente](./media/customize-branding/add-language.png)
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
6. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie erfahren, wie ein Unternehmensbranding zu Ihrem Azure AD-Verzeichnis hinzugefügt wird. 

Über den folgenden Link können Sie mit dem Azure-Portal Ihr Unternehmensbranding in Azure AD konfigurieren.

> [!div class="nextstepaction"]
> [Konfigurieren des Unternehmensbrandings](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 