---
title: Verwenden von b2clogin.com | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung von b2clogin.com statt login.microsoftonline.com.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: d2737e995b91caa2dcc55027baa2b552e64af23e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33660420"
---
#<a name="using-b2clogincom"></a>Verwenden von b2clogin.com

>[!IMPORTANT]
>Dieses Feature befindet sich in der Phase der öffentlichen Vorschau. 
>

Jetzt haben Sie die Möglichkeit, den Azure Active Directory B2C-Dienst mit `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` zu verwenden.  Dies hat viele Vorteile:
* Sie teilen das Größenlimit für Cookieheader nicht mehr mit anderen Microsoft-Produkten.
* Sie können alle Verweise auf Microsoft in Ihrer URL entfernen (Sie können `<YourTenantName>.onmicrosoft.com` durch Ihre Mandanten-ID ersetzen).

 Um alle Vorteile von b2clogin.com nutzen zu können, sollten Sie folgendes festlegen:

1. Stellen Sie sicher, dass Sie `<YourTenantName>.b2clogin.com` statt `login.microsoftonline.com` für den **Endpunkt für sofortige Ausführung** verwenden.
2. Wenn Sie MSAL verwenden, müssen Sie `validateauthority=false` festlegen.  Der Grund hierfür ist, dass der Tokenaussteller `<YourTenantName>.b2clogin.com` wird.