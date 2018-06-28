---
title: Gewähren von Berechtigungen für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation
description: Gewähren von Berechtigungen für Ihre benutzerdefiniert entwickelte Anwendung mit dem Azure AD-Portal oder einem URL-Parameter
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
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 3310a08047700a577c5c6cbada90e575fcd12089
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333703"
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
