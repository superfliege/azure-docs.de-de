---
title: Entfernen einer bei der Microsoft Identity Platform registrierten Anwendung | Azure
description: Es wird beschrieben, wie Sie eine Anwendung entfernen, die bei der Microsoft Identity Platform registriert ist.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 61da59fd570b9acc58dce89907eb5a1726a28de9
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104398"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform-preview"></a>Schnellstart: Entfernen einer bei Microsoft Identity Platform registrierten Anwendung (Vorschauversion)

Unternehmensentwickler und SaaS-Anbieter (Software-as-a-Service), die Anwendungen bei der Microsoft Identity Platform registriert haben, müssen die Registrierung einer Anwendung unter Umständen entfernen.

In diesem Schnellstart erfahren Sie, wie die folgenden Aufgaben ausgeführt werden:

* [Entfernen einer Anwendung, die von Ihnen oder Ihrer Organisation erstellt wurde](#remove-an-application-authored-by-your-organization)
* [Entfernen einer Anwendung, die von einer anderen Organisation erstellt wurde](#remove-an-application-authoried-by-another-organization)

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Sie verfügen über einen Mandanten, unter dem Anwendungen registriert wurden. Weitere Informationen zum Hinzufügen und Registrieren von Apps finden Sie unter [Register an application with the Microsoft identity platform](quickstart-register-app.md) (Registrieren einer Anwendung bei der Microsoft Identity Platform).
* Aktivieren Sie die Vorschauoberfläche für App-Registrierungen im Azure-Portal. Die Schritte in dieser Schnellstartanleitung gelten für die neue Benutzeroberfläche und funktionieren nur, wenn Sie sich für die Nutzung der Vorschauoberfläche entschieden haben.

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Entfernen einer Anwendung, die von Ihnen oder Ihrer Organisation erstellt wurde

Anwendungen, die von Ihnen oder Ihrer Organisation registriert wurden, werden in Ihrem Mandanten durch ein Anwendungsobjekt und ein Dienstprinzipalobjekt dargestellt. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md).

### <a name="to-remove-an-application"></a>So entfernen Sie eine Anwendung

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, indem Sie entweder ein Geschäfts-, Schul- oder Unikonto oder ein persönliches Microsoft-Konto verwenden.
2. Wenn Ihr Konto Zugriff auf mehr als einen Mandanten ermöglicht, wählen Sie oben rechts Ihr Konto aus und legen Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
3. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und dann **App-Registrierungen (Vorschau)**. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die Seite **Übersicht** angezeigt.
4. Wählen Sie auf der Seite **Übersicht** die Option **Löschen**.
5. Wählen Sie **Ja**, um zu bestätigen, dass Sie die App löschen möchten.

  > [!NOTE]
  > Zum Löschen einer Anwendung müssen Sie als Besitzer der Anwendung angegeben sein oder über Administratorberechtigungen verfügen.

## <a name="remove-an-application-authored-by-another-organization"></a>Entfernen einer Anwendung, die von einer anderen Organisation erstellt wurde

Wenn Sie **App-Registrierungen** im Kontext eines Mandanten anzeigen, stammt ein Teil der Anwendungen, die auf der Registerkarte **Alle Apps** angegeben sind, von einem anderen Mandanten. Sie wurden während des Zustimmungsprozesses unter Ihrem Mandanten registriert. Genauer gesagt: Sie werden unter Ihrem Mandanten nur von einem Dienstprinzipalobjekt ohne entsprechendes Anwendungsobjekt repräsentiert. Weitere Informationen zu den Unterschieden zwischen Anwendungs- und Dienstprinzipalobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure AD](active-directory-application-objects.md).

Der Administrator des Unternehmens muss seinen Dienstprinzipal entfernen, um für eine Anwendung den Zugriff auf Ihr Verzeichnis zu entfernen (nachdem die Zustimmung erteilt wurde). Der Administrator muss über globalen Administratorzugriff verfügen und kann die Anwendung über das Azure-Portal entfernen oder die [Azure AD-PowerShell-Cmdlets](https://go.microsoft.com/fwlink/?LinkId=294151) zum Entfernen des Zugriffs verwenden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die anderen Schnellstartanleitungen zur App-Verwaltung an:

* [Register an application with the Microsoft identity platform](quickstart-register-app.md) (Registrieren einer Anwendung bei der Microsoft Identity Platform)
* [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md)
* [Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs](quickstart-configure-app-expose-web-apis.md)
* [Modify the accounts supported by an application](quickstart-modify-supported-accounts.md) (Ändern der von einer Anwendung unterstützten Konten)
