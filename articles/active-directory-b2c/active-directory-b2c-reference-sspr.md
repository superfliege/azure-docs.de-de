---
title: 'Azure Active Directory B2C: Self-Service-Kennwortzurücksetzung | Microsoft Docs'
description: In diesem Thema wird das Einrichten der Self-Service-Kennwortrücksetzung für Ihre Kunden in Azure Active Directory B2C demonstriert.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: f38473989f90bfe6d35bffb17a02a892ad08cf5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Einrichten der Self-Service-Kennwortrücksetzung für Ihre Kunden
Das Feature für die Self-Service-Kennwortrücksetzung ermöglicht Ihren Kunden (die für lokale Konten registriert sind) das eigenständige Zurücksetzen ihrer Kennwörter. Dadurch wird die Belastung für Ihre Supportmitarbeiter erheblich reduziert, insbesondere, wenn Ihre Anwendung Millionen von Kunden besitzt, die sie regelmäßig verwenden. Derzeit unterstützen wir nur die Verwendung einer verifizierten E-Mail-Adresse als Wiederherstellungsmethode. Wir werden zukünftig zusätzliche Wiederherstellungsmethoden (verifizierte Telefonnummer, Sicherheitsfragen usw.) hinzufügen.

> [!NOTE]
> Dieser Artikel gilt für die Self-Service-Kennwortzurücksetzung im Kontext einer Anmeldungsrichtlinie. Informationen zu vollständig anpassbaren Richtlinien zur Kennwortrücksetzung, die in Ihrer App aufgerufen werden, finden Sie in [diesem Artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Standardmäßig ist für Ihr Verzeichnis die Self-Service-Kennwortzurücksetzung nicht aktiviert. Verwenden Sie die folgenden Schritte zum Aktivieren der Funktion:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie Ihr Verzeichnis erstellt haben.
2. Öffnen Sie Active Directory (in der Navigationsleiste auf der linken Seite).
3. Wählen Sie **Eigenschaften** aus.
4. Scrollen Sie nach unten zum Abschnitt **Self-Service-Kennwortzurücksetzung aktiviert**, und schalten Sie sie auf **Alle** um. 
5. Klicken Sie oben auf der Seite auf **Speichern**. Sie haben es geschafft!

Zum Testen verwenden Sie das Feature „Jetzt ausführen“ für eine beliebige Anmelderichtlinie mit lokalen Konten als Identitätsanbieter. Klicken Sie auf der lokalen Kontoanmeldeseite (auf der Sie eine E-Mail-Adresse und ein Kennwort oder einen Benutzernamen und ein Kennwort eingeben) auf **Sie können nicht auf Ihr Konto zugreifen?** , um die Benutzererfahrung zu überprüfen.

> [!NOTE]
> Die Seiten der Self-Service-Kennwortrücksetzung können mithilfe des [Features für Unternehmensbranding](../active-directory/customize-branding.md)angepasst werden.
> 
> 

