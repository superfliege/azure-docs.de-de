---
title: Behandlung von Fehlern am Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen | Microsoft-Dokumentation
description: In diesem Artikel werden eine Liste von Fehlermeldungen zum Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen und die Schritte aufgeführt, um diese zu beheben.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: eafbe25a5a0fa9182030304e9142a6013c9fb29b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42146268"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Behandlung von Fehlern am Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen 


Bei der Arbeit mit dem Power BI-Inhaltspaket für die Vorschau von Azure Active Directory können eventuell folgende Fehler auftreten: 

- [Fehler beim Aktualisieren](troubleshoot-content-pack.md#refresh-failed) 
- [Fehler beim Aktualisieren der Anmeldeinformationen für die Datenquelle](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Der Import von Daten dauert zu lange.](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
In diesem Artikel finden Sie Informationen zu den möglichen Ursachen und zur Behebung dieser Fehler.
 
## <a name="refresh-failed"></a>Fehler beim Aktualisieren 
 
**So wird dieser Fehler eingeblendet**: Per E-Mail von Power BI oder als Fehlerstatus im Aktualisierungsverlauf. 


| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Aktualisierungsfehler können auftreten, wenn die Anmeldeinformationen der Benutzer, die eine Verbindung mit dem Inhaltspaket herstellen, zurückgesetzt, aber nicht in den Verbindungseinstellungen des Inhaltspakets aktualisiert wurden. | Suchen Sie in Power BI das Dataset, das dem Dashboard für Azure Active Directory-Aktivitätsprotokolle entspricht. Wählen Sie die geplante Aktualisierung aus, und geben Sie dann Ihre Azure AD-Anmeldeinformationen ein. |
| Bei einer Aktualisierung können aufgrund von Datenproblemen im zugrunde liegenden Inhaltspaket Fehler auftreten. | Öffnen Sie ein Supportticket. Weitere Informationen finden Sie unter [Erhalten von Support für Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Fehler beim Aktualisieren der Anmeldeinformationen für die Datenquelle 
 
**So wird dieser Fehler eingeblendet**: In Power BI beim Herstellen einer Verbindung mit dem Inhaltspaket des Azure Active Directory-Aktivitätsprotokolls (Vorschau). 

| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Der Benutzer, der eine Verbindung herstellt, ist kein globaler Administrator, kein Benutzer mit Leseberechtigung für Sicherheitsfunktionen bzw. kein Sicherheitsadministrator. | Verwenden Sie für den Zugriff auf die Inhaltspakete ein Konto, dem entweder die Rolle eines globalen Administrators, eines Benutzers mit Leseberechtigung für Sicherheitsfunktionen oder eines Sicherheitsadministrators zugewiesen ist. |
| Ihr Mandant ist kein Premium-Mandant oder weist keinen Benutzer mit einer Premium-Lizenzdatei auf. | Öffnen Sie ein Supportticket. Weitere Informationen finden Sie unter [Erhalten von Support für Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Der Import von Daten dauert zu lange. 
 
**So wird dieser Fehler eingeblendet**: Wenn Sie in Power BI eine Verbindung mit Ihrem Inhaltspaket hergestellt haben, wird der Datenimportvorgang gestartet, und Ihr Dashboard wird für das Azure Active Directory-Aktivitätsprotokoll vorbereitet. Folgende Meldung wird angezeigt: „*Daten werden importiert...*“.  

| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Je nach Größe Ihres Mandanten kann dieser Schritt bis zu 30 Minuten dauern. | Haben Sie ein wenig Geduld. Wenn die Meldung innerhalb einer Stunde nicht ausgeblendet und Ihr Dashboard nicht angezeigt wird, öffnen Sie ein Supportticket. Weitere Informationen finden Sie unter [Erhalten von Support für Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Nächste Schritte

Klicken Sie [hier](https://powerbi.microsoft.com/blog/azure-active-directory-meets-power-bi/), um das Power BI-Inhaltspaket für die Vorschau von Azure Active Directory zu installieren.


