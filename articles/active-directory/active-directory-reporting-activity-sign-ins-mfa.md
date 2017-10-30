---
title: "Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal | Microsoft-Dokumentation"
description: "Enthält Referenzinformationen zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3b817c59658f4a5d102a3d65a17fade254e0257c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="reference-for-multi-factor-authentication-reporting-in-the-azure-portal"></a>Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal

Mit der [Azure Active Directory-Berichterstellungsfunktion (Azure AD)](active-directory-reporting-azure-portal.md) im [Azure-Portal](https://portal.azure.com) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die [Berichte zu den Anmeldeaktivitäten](active-directory-reporting-activity-sign-ins.md) enthalten Informationen zur Nutzung von verwalteten Anwendungen und Benutzeranmeldeaktivitäten, z.B. auch Informationen zur Nutzung von MFA (Multi-Factor Authentication, mehrstufige Authentifizierung). 

Die MFA-Daten liefern Ihnen Erkenntnisse zur Funktionsweise von MFA in Ihrer Organisation. Sie können dann beispielsweise folgende Fragen beantworten: 

- Wurde bei der Anmeldung MFA verwendet? 

- Wie hat der Benutzer den MFA-Vorgang durchgeführt? 

- Warum konnte der Benutzer den MFA-Vorgang nicht durchführen?  

Indem Sie alle Anmeldedaten aggregieren, können Sie in Ihrer Organisation ein besseres Verständnis des MFA-Ablaufs entwickeln. Mit den Daten können Sie folgende Fragen beantworten: 

- Wie viele Benutzer werden zur Durchführung des MFA-Vorgangs aufgefordert?  

- Wie viele Benutzer können den MFA-Vorgang nicht durchführen? 

- Welche MFA-Probleme treten für Benutzer häufig auf? 


Diese Daten sind über das Azure-Portal und die [Berichterstellungs-API](active-directory-reporting-api-getting-started-azure-portal.md) verfügbar. 


## <a name="data-structure"></a>Datenstruktur


Über die Berichte zu den Anmeldeaktivitäten für MFA erhalten Sie Zugriff auf die folgenden Informationen:

**MFA erforderlich:** Gibt an, ob MFA für die Anmeldung erforderlich ist. MFA kann aufgrund von MFA pro Benutzer, bedingtem Zugriff oder aus anderen Gründen obligatorisch sein. Mögliche Werte sind `Yes` oder `No`.

**MFA-Authentifizierungsmethode:** Die Authentifizierungsmethode, die vom Benutzer zum Durchführen des MFA-Vorgangs verwendet wurde. Mögliche Werte: 

- Textnachricht 

- Benachrichtigung über eine mobile App 

- Telefonanruf (Telefonnummer für Authentifizierung) 

- Prüfcode in der mobilen App 

- Telefonanruf (Geschäftliche Telefonnummer) 

- Telefonanruf (alternative Telefonnummer für Authentifizierung) 

**MFA-Authentifizierungsdetail:** Bereinigte Version der Telefonnummer, z.B. +X XXXXXXXX64. 

**MFA-Ergebnis:** Weitere Informationen dazu, ob der MFA-Vorgang erfolgreich durchgeführt wurde:

- Wenn der MFA-Vorgang erfolgreich war, enthält diese Spalte weitere Details dazu. 

- Wenn der MFA-Vorgang nicht erfolgreich war, ist in dieser Spalte der Grund angegeben. Mögliche Werte sind `Satisfied` oder `Denied`. 

Im folgenden Abschnitt sind die möglichen Zeichenfolgenwerte für das Feld mit dem MFA-Ergebnis aufgeführt.

## <a name="status-strings"></a>Statuszeichenfolgen

Dieser Abschnitt enthält die möglichen Werte für die Statuszeichenfolge zum MFA-Ergebnis.

### <a name="satisfied-status-strings"></a>Statuszeichenfolgen für „Erfolgreiche Durchführung“


- Azure Multi-Factor Authentication

    - completed in the cloud (in der Cloud durchgeführt) 

    - has expired due to the policies configured on tenant (ist aufgrund der auf dem Mandanten konfigurierten Richtlinien abgelaufen) 

    - registration prompted (zur Registrierung aufgefordert) 

    - satisfied by claim in the token (per Anspruch im Token erfüllt) 

    - satisfied by claim in the token (per Anspruch im Token erfüllt) 

    - satisfied by claim in the token (per Anspruch im Token erfüllt) 

    - satisfied by claim in the token (per Anspruch im Token erfüllt) 

    - satisfied by claim provided by external provider (per Anspruch vom externen Anbieter erfüllt) 

    - satisfied by strong authentication (per strenger Authentifizierung erfüllt) 

    - skipped as flow exercised was Windows broker logon flow (übersprungen, da es sich um einen Windows-Broker-Anmeldefluss gehandelt hat) 

    - skipped as flow exercised was Windows broker logon flow (übersprungen, da es sich um einen Windows-Broker-Anmeldefluss gehandelt hat) 

    - skipped due to app password (übersprungen aufgrund von App-Kennwort) 

    - skipped due to location (übersprungen aufgrund von Standort) 

    - skipped due to registered device (übersprungen aufgrund von registriertem Gerät) 
    
    - skipped due to remembered device (übersprungen aufgrund von gespeichertem Gerät) 

    - successfully completed (erfolgreich abgeschlossen) 

- Redirected to external provider for multi-factor authentication (umgeleitet an externen Anbieter zur mehrstufigen Authentifizierung) 

 
### <a name="denied-status-strings"></a>Statuszeichenfolgen für „Nicht erfolgreiche Durchführung“

Azure Multi-Factor Authentication denied; (Azure Multi-Factor Authentication verweigert;) 

- authentication in-progress (Authentifizierung in Bearbeitung) 

- duplicate authentication attempt (versuchte doppelte Authentifizierung) 

- entered incorrect code too many times (falschen Code zu häufig eingegeben) 

- invalid authentication (ungültige Authentifizierung) 

- invalid mobile app verification code (ungültiger Verifizierungscode der mobilen App) 

- misconfiguration (Fehlkonfiguration) 

- phone call went to voicemail (Voicemail bei Telefonanruf) 

- phone number has an invalid format (Telefonnummer hat ein ungültiges Format) 

- service error (Dienstfehler) 

- unable to reach the user’s phone (Telefon des Benutzers nicht erreichbar) 

- unable to send the mobile app notification to the device (Benachrichtigung über mobile App kann nicht an das Gerät gesendet werden) 

- unable to send the mobile app notification (Benachrichtigung über mobile App kann nicht gesendet werden) 

- user declined the authentication (Benutzer hat die Authentifizierung abgelehnt) 

- user did not respond to mobile app notification (Benutzer hat auf Benachrichtigung über mobile App nicht geantwortet) 

- user does not have any verification methods registered (für Benutzer sind keine Verifizierungsmethoden registriert) 

- user entered incorrect code (Benutzer hat falschen Code eingegeben) 

- user entered incorrect PIN (Benutzer hat falsche PIN eingegeben) 

- user hung up the phone call without succeeding the authentication (Benutzer hat Gespräch beendet, ohne die Authentifizierung durchzuführen) 

- user is blocked (Benutzer ist gesperrt) 

- user never entered the verification code (Benutzer hat den Verifizierungscode niemals eingegeben) 

- user not found (Benutzer wurde nicht gefunden) 
 
- verification code already used once (Verifizierungscode wurde bereits einmal verwendet) 



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Azure Active Directory-Berichterstellung](active-directory-reporting-azure-portal.md).




























