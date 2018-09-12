---
title: Problem beim Erstellen einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Beheben von Problemen beim Erstellen von Anwendungsproxyanwendungen im Azure AD-Verwaltungsportal
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 2344d35827cf541f0230f74917be3ae0ea39e074
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44354761"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problem beim Erstellen einer Anwendungsproxyanwendung 

Nachfolgend sind einige der häufigen Probleme aufgeführt, die beim Erstellen einer neuen Anwendungsproxyanwendung auftreten können.

## <a name="recommended-documents"></a>Empfohlene Dokumente 

Informationen zum Erstellen einer Anwendungsproxyanwendung über das Verwaltungsportal finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md).

Wenn Sie die Schritte in dieser Dokumentation befolgen und beim Erstellen der Anwendung einen Fehler erhalten, finden Sie weitere Informationen und Vorschläge zur Behebung der Anwendung in den Fehlerdetails. Die meisten Fehlermeldungen enthalten eine empfohlene Problemlösung. 

## <a name="specific-things-to-check"></a>Überprüfen bestimmter Punkte

Stellen Sie Folgendes sicher, um häufige Fehler zu vermeiden:

-   Sie sind ein Administrator mit der Berechtigung zum Erstellen einer Anwendungsproxyanwendung.

-   Die interne URL ist eindeutig.

-   Die externe URL ist eindeutig.

-   Die URLs beginnen mit „http“ oder „https“ und enden mit einem „/“.

-   Die URL muss ein Domänenname, keine IP-Adresse sein.

Die Fehlermeldung sollte beim Erstellen der Anwendung in der oberen rechten Ecke angezeigt werden. Sie können auch das Benachrichtigungssymbol auswählen, um die Fehlermeldungen anzuzeigen.

   ![Benachrichtigungsaufforderung](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren des Anwendungsproxys über das Azure-Portal](application-proxy-enable.md)
