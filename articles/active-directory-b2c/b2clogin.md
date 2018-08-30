---
title: Verwenden von b2clogin.com | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung von b2clogin.com statt login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6ad0a5d59b28bf48742c9e1be89b51d2301dd582
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189289"
---
# <a name="using-b2clogincom"></a>Verwenden von b2clogin.com

>[!IMPORTANT]
>Dieses Feature befindet sich in der Phase der öffentlichen Vorschau. 
>

Jetzt haben Sie die Möglichkeit, den Azure Active Directory B2C-Dienst mit `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` zu verwenden.  Dies hat viele Vorteile:
* Sie teilen das Größenlimit für Cookieheader nicht mehr mit anderen Microsoft-Produkten.
* Sie können alle Verweise auf Microsoft in Ihrer URL entfernen (Sie können `<YourTenantName>.onmicrosoft.com` durch Ihre Mandanten-ID ersetzen). Beispiel: `https://<tenantname>.b2clogin.com/tfp/<tenantID>/<policyname>/v2.0/.well-known/openid-configuration`.

 Um alle Vorteile von b2clogin.com nutzen zu können, sollten Sie folgendes festlegen:

1. Stellen Sie sicher, dass Sie `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` für den **Endpunkt für sofortige Ausführung** verwenden.
2. Wenn Sie MSAL verwenden, müssen Sie `validateauthority=false` festlegen.  Der Grund hierfür ist, dass der Tokenaussteller `<YourTenantName>.b2clogin.com` wird.
