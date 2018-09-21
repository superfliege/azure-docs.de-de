---
title: Gewähren von Berechtigungen für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation
description: Gewähren von Berechtigungen für Ihre benutzerdefiniert entwickelte Anwendung mit dem Azure AD-Portal oder einem URL-Parameter
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 10cf04fca8379f41587b1ea680c5b52a26193b53
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723781"
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Gewähren von Berechtigungen für eine benutzerdefiniert entwickelte Anwendung

Wenn Sie im Vorfeld Berechtigungen für Ihre App gewähren möchten oder ein Fehler auftritt, den Sie keiner App zugeteilt haben, führen Sie die unten stehenden Schritte aus.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Ausführen der Administratorzustimmung für Ihre Anwendung

Dies entspricht dem Erteilen der Zustimmung zur Anwendung für alle Benutzer in Ihrer Organisation.

1. Navigieren Sie als **globaler Administrator** zum Blatt **App-Registrierungen**, und wählen Sie dann die App aus.

2. Wählen Sie **Erforderliche Berechtigungen** aus, und klicken Sie dann auf die Schaltfläche **Berechtigungen erteilen** am oberen Rand des Blatts.

Alternativ können Sie eine Anforderung an *login.microsoftonline.com* mit Ihren App-Konfigurationen erstellen und an *&prompt=admin\_consent* anfügen. Nach der Anmeldung als Administrator sollte die App-Zustimmung für alle Benutzer erteilt sein.

## <a name="how-to-force-user-consent-for-your-application"></a>Erzwingen der Benutzerzustimmung für Ihre Anwendung

* Fügen Sie *&prompt=consent* an Autorisierungsanforderungen an, die bei jeder Authentifizierung Zustimmung durch die Endbenutzer erfordern.

## <a name="next-steps"></a>Nächste Schritte

[Genehmigen und Integrieren von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Genehmigen und Zuweisen von Berechtigungen für konvergierte Azure AD V2.0-Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
