---
title: Multi-Factor Authentication in Azure Active Directory B2C | Microsoft-Dokumentation
description: Aktivieren der Multi-Factor Authentication in kundenorientierten Anwendungen, die mit Azure Active Directory B2C geschützt werden
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6c44bebad6b54ab673fcbf3b2ef5e5d34c8c6882
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847727"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Aktivieren der mehrstufige Authentifizierung in Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C bietet eine direkte Integration in die [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md), damit Sie eine zweite Sicherheitsebene zu Registrierungs- und Anmeldeoberflächen in Anwendungen hinzufügen können. Sie können die mehrstufige Authentifizierung aktivieren, ohne eine einzige Codezeile schreiben zu müssen. Wenn Sie bereits Benutzerflows für Registrierung und Anmeldung erstellt haben, können Sie dennoch die mehrstufige Authentifizierung aktivieren.

Mithilfe dieses Features können Anwendungen z. B. folgende Szenarios bewältigen:

- Für den Zugriff auf eine Anwendung ist keine mehrstufige Authentifizierung erforderlich, jedoch für den Zugriff auf eine andere Anwendung. Beispielsweise kann sich der Kunde bei der Anwendung einer Kfz-Versicherung mit einem lokalen Konto oder dem Konto eines sozialen Netzwerks anmelden. Er muss jedoch seine Telefonnummer bestätigen, bevor er auf die Anwendung für die Hausversicherung zugreifen kann, die im selben Verzeichnis registriert ist.
- Die mehrstufige Authentifizierung ist allgemein für den Zugriff auf eine Anwendung nicht erforderlich, jedoch für vertrauliche Unterbereiche dieser Anwendung. Beispielsweise kann sich der Kunde bei einer Onlinebanking-Anwendung mit einem lokalen Konto oder dem Konto eines sozialen Netzwerks anmelden, um den Kontostand abzufragen. Um eine Überweisung zu tätigen, ist jedoch die Bestätigung der Telefonnummer erforderlich.

## <a name="set-multi-factor-authentication"></a>Festlegen der mehrstufigen Authentifizierung

Wenn Sie einen Benutzerflow erstellen, können Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) aktivieren.

![Festlegen der mehrstufige Authentifizierung](./media/active-directory-b2c-reference-mfa/add-policy.png)

Legen Sie **Mehrstufige Authentifizierung** auf **Aktiviert** fest.

Sie können die Benutzeroberflächenfunktion mit **Benutzerflow ausführen** überprüfen. Bestätigen Sie das folgende Szenario:

In Ihrem Mandanten wird ein Kundenkonto erstellt, bevor der Schritt für die mehrstufige Authentifizierung ausgeführt wird. Während dieses Schritts wird der Kunde aufgefordert, seine Telefonnummer anzugeben und zu bestätigen. Wenn die Überprüfung erfolgreich ist, wird die Telefonnummer zur späteren Verwendung an das Kundenkonto angefügt. Auch wenn der Kunde den Vorgang abbricht, kann er bei der nächsten Anmeldung aufgefordert werden, eine Telefonnummer erneut zu bestätigen (bei aktivierter mehrstufiger Authentifizierung).

## <a name="add-multi-factor-authentication"></a>Hinzufügen der mehrstufigen Authentifizierung

Es ist möglich, die mehrstufige Authentifizierung für einen Benutzerflow zu aktivieren, den Sie zuvor erstellt haben. 

So aktivieren Sie die mehrstufige Authentifizierung:

1. Öffnen Sie den Benutzerflow, und wählen Sie **Eigenschaften** aus. 
2. Klicken Sie neben **Mehrstufige Authentifizierung** auf **Aktiviert**.
3. Klicken Sie oben auf der Seite auf **Speichern**.


