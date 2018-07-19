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
ms.openlocfilehash: 1d42d9a97244eeff501b9d02b0f143d6ef0c91b2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440629"
---
# <a name="using-b2clogincom"></a>Verwenden von b2clogin.com

>[!IMPORTANT]
>Dieses Feature befindet sich in der Phase der öffentlichen Vorschau. 
>

Jetzt haben Sie die Möglichkeit, den Azure Active Directory B2C-Dienst mit `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` zu verwenden.  Dies hat viele Vorteile:
* Sie teilen das Größenlimit für Cookieheader nicht mehr mit anderen Microsoft-Produkten.
* Sie können alle Verweise auf Microsoft in Ihrer URL entfernen (Sie können `<YourTenantName>.onmicrosoft.com` durch Ihre Mandanten-ID ersetzen). Beispiel: `https://<tenantname>.b2clogin.com/tfp/<tenantname>/<policyname>/v2.0/.well-known/openid-configuration`.

 Um alle Vorteile von b2clogin.com nutzen zu können, sollten Sie folgendes festlegen:

1. Stellen Sie sicher, dass Sie `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` für den **Endpunkt für sofortige Ausführung** verwenden.
2. Wenn Sie MSAL verwenden, müssen Sie `validateauthority=false` festlegen.  Der Grund hierfür ist, dass der Tokenaussteller `<YourTenantName>.b2clogin.com` wird.