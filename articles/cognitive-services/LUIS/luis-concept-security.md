---
title: Grundlegendes zum Zugriff auf LUIS-Anwendungen – Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über den Zugriff für die LUIS-Erstellung.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: 13b769a0b5a940e0f3dd5f2e0cc3567d9879ee0d
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223778"
---
# <a name="authoring-and-endpoint-user-access"></a>Zugriff für die Erstellung und Benutzerzugriff auf Endpunkte
Erstellungszugriff ist für Besitzer und Projektmitarbeiter möglich. Bei einer privaten App ist der Zugriff auf den Endpunkt für Besitzer und Projektmitarbeiter möglich. Bei einer öffentlichen App ist der Zugriff auf den Endpunkt für jede Person möglich, die über ein eigenes LUIS-Konto und die öffentliche App-ID verfügt. 

## <a name="access-to-authoring"></a>Zugriff für die Erstellung
Der Zugriff auf die App von der [LUIS](luis-reference-regions.md#luis-website)-Website oder den [Erstellungs-APIs](https://aka.ms/luis-authoring-apis) wird vom Besitzer der App gesteuert. 

Der Besitzer und alle Projektmitarbeiter haben Zugriff für die Erstellung der App. 

|Der Zugriff für die Erstellung umfasst|Notizen|
|--|--|
|Hinzufügen oder Entfernen von Endpunktschlüsseln||
|Exportieren von Versionen||
|Exportieren von Endpunktprotokollen||
|Importieren von Versionen||
|Öffentlichmachen von Apps|Wenn eine App öffentlich ist, kann jede Person mit einem Erstellungs- oder Endpunktschlüssel die App abfragen.|
|Ändern von Modellen|
|Veröffentlichen|
|Überprüfen von Endpunktäußerungen für das [aktive Lernen](luis-how-to-review-endoint-utt.md)|
|Trainieren|

## <a name="access-to-endpoint"></a>Zugriff auf den Endpunkt
Der Zugriff auf den Endpunkt für das Abfragen von LUIS wird von der Einstellung **Öffentlich** der App auf der Seite **Einstellungen** gesteuert. Eine Abfrage am Endpunkt einer privaten App wird auf einen Erstellungsschlüssel mit verbleibenden Kontingenttreffern überprüft. Bei einer Abfrage am Endpunkt einer öffentlichen App muss auch ein Endpunktschlüssel (vom Abfragesteller) auf verbleibende Kontingenttreffer überprüft werden. 

Der Endpunktschlüssel wird entweder in der Abfragezeichenfolge der GET-Anforderung oder im Header der POST-Anforderung übergeben.

![Festlegen der App als öffentlich](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Sicherheit am Endpunkt privater Apps
Der Endpunkt einer privaten App ist nur für Folgendes verfügbar:

|Schlüssel und Benutzer|Erklärung|
|--|--|--|
|Erstellungsschlüssel des Besitzers| Bis zu 1.000 Endpunkttreffer|
|Erstellungsschlüssel von Projektmitarbeitern| Bis zu 1.000 Endpunkttreffer|
|Endpunktschlüssel, die auf der Seite **[Veröffentlichen](luis-how-to-publish-app.md)** hinzugefügt werden|Besitzer und Projektmitarbeiter können Endpunktschlüssel hinzufügen.|

Andere Erstellungs- oder Endpunktschlüssel haben **keinen** Zugriff.

### <a name="public-app-endpoint-access"></a>Zugriff auf Endpunkte öffentlicher Apps
Sie konfigurieren eine App auf der Seite **Einstellungen** der App als **öffentlich**. Wenn eine App als öffentlich konfiguriert ist, können _alle_ gültigen LUIS-Erstellungsschlüssel oder LUIS-Endpunktschlüssel Ihre App abfragen, solange der Schlüssel nicht das gesamte Endpunktkontingent verbraucht hat.

Ein Benutzer, der nicht Besitzer oder Projektmitarbeiter ist, hat Zugriff auf eine öffentliche App, wenn er die App-ID hat. LUIS verfügt nicht über einen öffentlichen _Markt_ oder eine andere Möglichkeit der Suche nach einer öffentlichen App.  

## <a name="microsoft-user-accounts"></a>Microsoft-Benutzerkonten
Ersteller und Projektmitarbeiter können LUIS auf der Seite „Veröffentlichen“ Schlüssel hinzufügen. Das Microsoft-Benutzerkonto, das den LUIS-Schlüssel im Azure-Portal erstellt, muss dem App-Besitzer oder einem App-Projektmitarbeiter zugeordnet sein. 

Unter [Azure Active Directory-Mandantenbenutzer](luis-how-to-account-settings.md#azure-active-directory-tenant-user) finden Sie weitere Informationen zu Active Directory-Benutzerkonten. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Schützen von Endpunkten 
Sie können steuern, wer Ihre LUIS-Endpunktschlüssel anzeigen kann, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und die Authentifizierung und Autorisierung überprüft wurden, übergeben Sie den Aufruf an LUIS. Auch wenn diese Strategie Man-in-the-Middle-Angriffe nicht verhindert, verbirgt sie Ihren Endpunkt vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (z.B. [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Sicherheit und Compliance
LUIS hat die Überprüfung nach ISO 27001:2013 und ISO 27018:2014 mit 0 (null) Abweichungen (Ergebnisse) im Prüfbericht erfolgreich absolviert. Außerdem erhielt LUIS auch die CSA STAR-Zertifizierung mit der höchstmöglichen Auszeichnung, dem Gold Award, bei der Bewertung der Robustheit. Azure ist der einzige wichtige Dienstanbieter einer öffentliche Cloud, der diese Zertifizierung erhalten hat. Weitere Informationen finden Sie in den Angaben zu LUIS im aktualisierten Gültigkeitshinweis im Hauptdokument mit der [Übersicht über die Kompatibilität](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) von Azure, auf das auf den ISO-Seiten im [Trust Center](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) verwiesen wird.  

## <a name="next-steps"></a>Nächste Schritte

Unter [Best Practices](luis-concept-best-practices.md) erfahren Sie, wie Sie Absichten und Entitäten für die besten Vorhersagen verwenden.