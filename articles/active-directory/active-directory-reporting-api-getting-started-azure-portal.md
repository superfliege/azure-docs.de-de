---
title: Erste Schritte mit der Azure AD-Berichterstellungs-API | Microsoft-Dokumentation
description: Vorgehensweise zum Einstieg in die Azure Active Directory-Berichterstellungs-API
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: df0b672a07575a0d26fff89c90008043d02818dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589099"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Erste Schritte mit der Berichterstellungs-API von Azure Active Directory

Azure Active Directory stellt eine Vielzahl von [Berichten](active-directory-reporting-azure-portal.md) zur Verfügung. Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – sehr nützlich sein. 

Mit der Azure AD-Berichterstellungs-APIs können Sie über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten erhalten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Dieser Artikel enthält eine Übersicht über den Zugriff auf die Berichtsdaten mit der zugehörigen API.

Wenn Probleme auftreten sollten, lesen Sie [Gewusst wie: Beziehen von Support für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Voraussetzungen

Um auf die Berichterstellungs-API zuzugreifen, müssen Sie folgende Schritte durchführen (auch wenn Sie mithilfe eines Skripts auf die API zugreifen möchten):

1. Zuweisen von Rollen (Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Sicherheitsadministrator, globaler Administrator)
2. Registrieren einer Anwendung
3. Erteilen von Berechtigungen
4. Erfassen von Konfigurationseinstellungen


 
Ausführliche Anweisungen finden Sie in den [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API](active-directory-reporting-api-prerequisites-azure-portal.md).


## <a name="recommendation"></a>Empfehlung 

Wenn Sie Berichtsdaten ohne Benutzereingriff abrufen möchten, sollten Sie eventuell die Azure AD-Berichterstellungs-API mit Zertifikaten verwenden.

Ausführliche Anweisungen finden Sie unter [Abrufen von Daten per Azure AD-Berichterstellungs-API mit Zertifikaten](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Erkunden

Verschaffen Sie sich einen ersten Eindruck über die Berichterstellungs-APIs:
   
   - [Verwenden der Beispiele für die Überwachungs-API](active-directory-reporting-api-audit-samples.md) 
 
   - [Verwenden der Beispiele für die Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Anpassen  

Erstellen Sie Ihre eigene Lösung: 
   
   - [Verwenden der Referenz zur Überwachungs-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Verwenden der Referenz zur Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



