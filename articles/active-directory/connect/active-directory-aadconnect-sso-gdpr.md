---
title: 'Azure AD Connect: Nahtloses einmaliges Anmelden – Einhaltung der DSGVO | Microsoft-Dokumentation'
description: In diesem Artikel geht es um das nahtlose einmalige Anmelden für Azure Active Directory (Azure AD) und die Einhaltung der DSGVO.
services: active-directory
keywords: Was ist Azure AD Connect, DSGVO, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: billmath
ms.openlocfilehash: 0c7ed376accb1eed01106358491e925d3b8126c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="azure-ad-seamless-single-sign-on-gdpr-compliance"></a>Nahtloses einmaliges Anmelden für Azure AD: Einhaltung der DSGVO

## <a name="overview"></a>Übersicht

Im Mai 2018 tritt ein europäisches Datenschutzgesetz – die [Datenschutz-Grundverordnung (DSGVO)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) – in Kraft. Die DSGVO enthält neue Vorschriften für Unternehmen, Regierungsbehörden, gemeinnützige Organisationen und andere Organisationen, die Menschen in der Europäischen Union (EU) Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. Die DSGVO gilt unabhängig von Ihrem Standort. 

Ihnen stehen schon heute Microsoft-Produkte und -Dienste zur Verfügung, mit denen Sie die DSGVO erfüllen können. Weitere Informationen zur Microsoft-Datenschutzrichtlinie finden Sie im [Trust Center](https://www.microsoft.com/trustcenter).

Beim nahtlosen einmaligen Anmelden für Azure AD wird der folgende Protokolltyp erstellt, der EU II-Richtlinien umfassen kann:

- Azure AD Connect-Ablaufverfolgungsprotokoll-Dateien

Die Einhaltung der DSGVO für das nahtlose einmalige Anmelden kann auf zwei Arten erreicht werden:

1.  Extrahieren von Daten für eine Person und Entfernen der Daten dieser Person aus Installationen auf Anforderung
2.  Sicherstellen, dass keine Daten länger als 48 Stunden beibehalten werden

Wir raten Ihnen dringend, die zweite Option zu verwenden, da diese einfacher zu implementieren und zu verwalten ist. Hier sind die Anleitungen für die einzelnen Protokolltypen angegeben:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Löschen von Azure AD Connect-Ablaufverfolgungsprotokoll-Dateien

Überprüfen Sie den Inhalt des Ordners **%ProgramData%\AADConnect**, und löschen Sie den Inhalt des Ablaufverfolgungsprotokolls (**trace-\*.log**-Dateien) des Ordners innerhalb von 48 Stunden nach dem Installieren oder Aktualisieren von Azure AD Connect oder dem Ändern des nahtlosen einmaligen Anmeldens. Der Grund ist, dass mit dieser Aktion ggf. Daten erstellt werden, die unter die DSGVO fallen.

>[!IMPORTANT]
>Löschen Sie nicht die Datei **PersistedState.xml** in diesem Ordner, da diese Datei zum Verwalten des Zustands der vorherigen Installation von Azure AD Connect und nach einer Upgradeinstallation verwendet wird. Diese Datei enthält niemals Daten zu einer Person und sollte nie gelöscht werden.

Sie können diese Ablaufverfolgungsprotokoll-Dateien entweder mit Windows-Explorer überprüfen und löschen, oder Sie können das folgende PowerShell-Skript verwenden, um die erforderlichen Aktionen durchzuführen:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Speichern Sie das Skript in einer Datei mit der Erweiterung „.PS1“. Führen Sie dieses Skript je nach Bedarf aus.

Weitere Informationen zu verwandten DSGVO-Anforderungen für Azure AD Connect finden Sie in [diesem Artikel](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Hinweis zu Domänencontrollerprotokollen

Wenn die Überwachungsprotokollierung aktiviert ist, kann dieses Produkt Sicherheitsprotokolle für Ihre Domänencontroller generieren. Weitere Informationen zum Konfigurieren von Überwachungsrichtlinien finden Sie in [diesem Artikel](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Nächste Schritte

- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-sso.md) – Beheben von häufig auftretenden Problemen mit diesem Feature
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen
