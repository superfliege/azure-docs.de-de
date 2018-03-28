---
title: DSGVO im Azure Active Directory-Anwendungsproxy | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die DSGVO im Azure Active Directory-Anwendungsproxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5e99fba2f226e1e9b401bd8ef5b3b85a5e8c6a7c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="gdpr-in-the-azure-active-directory-application-proxy"></a>DSGVO im Azure Active Directory-Anwendungsproxy  

Das Azure Active Directory-Anwendungsproxy (Azure AD) ist wie alle anderen Microsoft-Dienste und -Features mit der DSGVO konform. Weitere Informationen zur Konformität von Microsoft mit der DSGVO finden Sie unter [Licensing Terms and Documentation](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (Lizenzierungsbedingungen und Dokumentation).

Da dieses Feature Connectors für Ihre Computer umfasst, gibt es einige Ereignisse, die Sie zur Aufrechterhaltung der Konformität mit der DSGVO überwachen müssen. Das Anwendungsproxy erstellt die folgenden Protokolltypen, die EU II-Richtlinien umfassen können:

- Connector-Ereignisprotokolle
- Windows-Ereignisprotokolle

Es gibt zwei Möglichkeiten zur Einhaltung der DSGVO:

- Löschen und Exportieren von Anforderungen, sobald sie auftreten

- Deaktivieren der Protokollierung

Für:

- Informationen zum Konfigurieren der Datenaufbewahrung für die Windows-Ereignisprotokolle, siehe [Settings for event logs](https://technet.microsoft.com/library/cc952132.aspx) (Einstellungen für Ereignisprotokolle) 
- Allgemeine Informationen zum Windows-Ereignisprotokoll, siehe [Using Windows Event Log](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx) (Verwendung des Windows-Ereignisprotokolls)


Die Connectorereignisprotokolle finden Sie unter `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. In den folgenden Abschnitten werden die jeweiligen Schritte zu den Connectorereignisprotokollen beschrieben. Sie müssen diese Schritte auf allen Connectorcomputern ausführen.
 

## <a name="processing-requests"></a>Verarbeiten von Anforderungen

Es gibt drei verschiedene Typen von Anforderungen, für die Sie haftbar sind: 

- Löschen
- Sicht
- Export
 
Um Anzeige-/Exportanforderungen verarbeiten zu können, müssen Sie die einzelnen Protokolldateien für die Suche nach verwandten Einträgen durchlaufen. 

Da es sich bei den Protokollen um Textdateien handelt, können Sie z.B. mithilfe des Befehls `findstr` nach Einträgen im Zusammenhang mit Ihrem Benutzer suchen. Suchen Sie nach den folgenden Feldern, die möglicherweise in den Protokollen befindlich sind: 

- UserId
- Geben Sie den Typ des Benutzernamens an, der für Anwendungen mit eingeschränkter Kerberos-Delegierung konfiguriert ist:
    - Benutzerprinzipalname
    - Lokaler Benutzerprinzipalname
    - Benutzernamenteil des Benutzerprinzipalnamens
    - Benutzernamenteil des lokalen Benutzerprinzipalnamens
    - Name des lokalen SAM-Kontos 

 
Sie können diese Felder dann erfassen und für den Benutzer freigeben.
Um Löschanforderungen zu verarbeiten, müssen Sie die relevanten Protokolle löschen. Sie können den Connectordienst (Microsoft Azure AD-Anwendungsproxyconnector) neu starten, um eine neue Protokolldatei zu generieren. Mithilfe der neuen Protokolldatei können Sie die alten Protokolldateien löschen. Anschließend können Sie über den Prozess für die Anzeige bzw. den Export alle relevanten Protokolle suchen und diese Felder oder Dateien selektiv löschen. Sie können auch einfach stets sämtliche alte Protokolldateien löschen, wenn Sie sie nicht mehr benötigen.


## <a name="turn-off-connector-logs"></a>Deaktivieren von Connectorprotokollen

Um Connectorprotokolle zu deaktivieren, müssen Sie `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config` anpassen, indem Sie die markierte Zeile entfernen: 


![Konfiguration](./media/active-directory-application-proxy-gdpr/01.png)


## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über den Azure AD-Anwendungsproxy finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).

