---
title: Entfernen einer Anwendung aus Azure Active Directory
description: Erfahren Sie, wie eine Anwendung aus Azure Active Directory (Azure AD) entfernt wird.
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
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0038dcc10aafa191121b2797f68d66a3e32b3fa7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207448"
---
# <a name="quickstart-remove-an-application-from-azure-active-directory"></a>Schnellstart: Entfernen einer Anwendung aus Azure Active Directory

Unternehmensentwickler und Software-as-a-Service-Anbieter (SaaS), die Anwendungen bei Azure Active Directory (Azure AD) registriert haben, müssen möglicherweise die Registrierung einer Anwendung aus ihrem Azure AD-Mandanten entfernen.

In diesem Schnellstart erfahren Sie, wie die folgenden Aufgaben ausgeführt werden:

* [Entfernen einer Anwendung, die von Ihrer Organisation erstellt wurde](#removing-an-application-authored-by-your-organization)
* [Entfernen einer mehrinstanzenfähigen Anwendung, die von einer anderen Organisation autorisiert ist](#removing-a-multi-tenant-application-authorized-by-another-organization)

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie einen Azure AD-Mandanten, bei dem Anwendungen registriert wurden.

* Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](quickstart-create-new-tenant.md).
* Weitere Informationen zum Hinzufügen und Registrieren von Apps bei Ihrem Mandanten finden Sie unter [Hinzufügen einer Anwendung zu Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="removing-an-application-authored-by-your-organization"></a>Entfernen einer Anwendung, die von Ihrer Organisation erstellt wurde

Anwendungen, die von Ihrer Organisation registriert wurden, werden unter dem Filter **Meine Apps** auf der Hauptseite **App-Registrierungen** Ihres Mandanten angezeigt. Dies sind Anwendungen, die Sie manuell über das Azure-Portal oder programmgesteuert über PowerShell oder die Microsoft Graph-API registriert haben. Genauer gesagt, werden diese Anwendungen sowohl durch ein Anwendungs- als auch durch ein Dienstprinzipalobjekt in Ihrem Mandanten dargestellt. Weitere Informationen zu diesen Objekten finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md).

### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>So entfernen Sie eine Einzelinstanzanwendung aus Ihrem Verzeichnis

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Ihr Konto Zugriff auf mehrere Mandanten ermöglicht, wählen Sie oben rechts Ihr Konto aus und legen Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den **Azure Active Directory**-Dienst und dann **App-Registrierungen** aus. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und wählen Sie diese aus.
    Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
1. Klicken Sie auf der Hauptseite der Anwendungsregistrierung auf **Löschen**.
1. Wählen Sie **Ja** aus, um zu bestätigen, dass Sie die Anwendung löschen möchten.

### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>So entfernen Sie eine mehrinstanzenfähige Anwendung aus ihrem Stammverzeichnis

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Ihr Konto Zugriff auf mehrere Mandanten ermöglicht, wählen Sie oben rechts Ihr Konto aus und legen Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den **Azure Active Directory**-Dienst und dann **App-Registrierungen** aus. Suchen Sie anschließend nach der Anwendung, die Sie konfigurieren möchten, und wählen Sie diese aus.
    Sie gelangen auf die Hauptseite der Anwendungsregistrierung, und die Seite **Einstellungen** für die Anwendung wird geöffnet.
1. Wählen Sie auf der Seite **Einstellungen** die Option **Eigenschaften** aus, und legen Sie **Mehrinstanzenfähig** auf **Nein** fest, um Ihre Anwendung zuerst auf die Verwendung eines einzelnen Mandanten festzulegen. Wählen Sie anschließend **Speichern** aus.
    Die Dienstprinzipalobjekte der Anwendung verbleiben auf den Mandanten aller Organisationen, die bereits ihre Zustimmung erteilt haben.
1. Wählen Sie auf der Hauptseite der Anwendungsregistrierung **Löschen** aus.
1. Wählen Sie **Ja** aus, um zu bestätigen, dass Sie die Anwendung löschen möchten.

## <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Entfernen einer mehrinstanzenfähigen Anwendung, die von einer anderen Organisation autorisiert ist

Bei einer Teilmenge der Anwendungen, die auf der Hauptseite **App-Registrierungen** Ihres Mandanten unter dem Filter **Alle Apps** (mit Ausnahme der Registrierungen vom Typ **Meine Apps**) angezeigt werden, handelt es sich um mehrinstanzenfähige Anwendungen.

In technischer Hinsicht stammen diese mehrinstanzenfähigen Anwendungen von einem anderen Mandanten und wurden während des Zustimmungsprozesses unter Ihrem Mandanten registriert. Genauer gesagt: Sie werden unter Ihrem Mandanten nur von einem Dienstprinzipalobjekt ohne entsprechendes Anwendungsobjekt repräsentiert. Weitere Informationen zu den Unterschieden zwischen Anwendungs- und Dienstprinzipalobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure AD](app-objects-and-service-principals.md).

Der Administrator des Unternehmens muss den Dienstprinzipal der Anwendung entfernen, um für eine mehrinstanzenfähige Anwendung den Zugriff auf Ihr Verzeichnis zu entfernen (nachdem die Zustimmung erteilt wurde). Der Administrator muss über globalen Administratorzugriff verfügen und kann die Entfernung durchführen – entweder über das Azure-Portal oder über die [Azure AD-PowerShell-Cmdlets](https://go.microsoft.com/fwlink/?LinkId=294151).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über diese anderen verwandten Schnellstarts für die App-Verwaltung für Apps, die den Azure AD v1.0-Endpunkt verwenden:

- [Hinzufügen einer Anwendung zu Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Aktualisieren einer Anwendung in Azure AD](quickstart-v1-update-azure-ad-app.md)
