---
title: Konfigurieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS) in Azure API for FHIR konfigurieren.
author: matjazl
ms.author: matjazl
ms.date: 3/11/2019
ms.topic: reference
ms.service: healthcare-apis
ms.openlocfilehash: 4976ce5b8f0216e1a0978c7ad2225bf94e6af3cb
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58408381"
---
# <a name="configure-cross-origin-resource-sharing"></a>Konfigurieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen

Azure API for FHIR unterstützt die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](https://wikipedia.org/wiki/Cross-Origin_Resource_Sharing), mit der Sie Einstellungen konfigurieren können, damit Anwendungen aus einer Domäne (Ursprung) auf Ressourcen aus einer anderen Domäne (domänenübergreifende Anforderung) zugreifen können.

CORS wird hauptsächlich in Single-Page-Apps verwendet, die RESTful-API-Aufrufe für eine andere Domäne aufrufen muss.

Navigieren Sie zum Konfigurieren der CORS-Einstellung in Azure API for FHIR zur CORS-Einstellung. 

Hier können Sie die folgenden Einstellungen vornehmen:

**Ursprünge (Access-Control-Allow-Origin):** Liste der Domänen, die Anforderungen zwischen verschiedenen Ursprüngen an Azure API for FHIR stellen dürfen. Jede Domäne (Ursprung) muss in einer separaten Zeile eingegeben werden. Sie können „*“ eingeben, um Anrufe aus beliebigen Domänen zuzulassen (nicht empfohlen, da dies ein Sicherheitsrisiko darstellt).

**Headers (Access-Control-Allow-Headers):** Sie können eine Liste der Header angeben, die die Ursprungsanforderung enthalten soll. Sie können hier „*“ festlegen, um alle Header zuzulassen. 

**Methoden (Access-Control-Allow-Methods):** Sie können zulässige Methoden (PUT, GET, POST,...) auswählen, die im API-Aufruf erlaubt sind. Sie können auch **Alle auswählen** für alle Methoden verwenden.

**Max. Alter (Access-Control-Max-Age):** Wert in Sekunden, um die Ergebnisse der Preflight-Anforderung für **Access-Control-Allow-Methods** und **Access-Control-Allow-Headers** zwischenzuspeichern. 

**Anmeldeinformationen zulassen (Access-Control-Allow-Credentials):** CORS-Anforderungen enthalten normalerweise keine Cookies, um [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)-Angriffe (siteübergreifende Anforderungsfälschung) zu verhindern. Wenn diese Option aktiviert ist, kann die Anforderung mit Anmeldeinformationen (z. B. Cookies) gestellt werden. Sie können diese Einstellung nicht festlegen, wenn Sie die Ursprünge auf „*“ festlegen.

![](media/cors/cors.png)

Sie können keine unterschiedlichen Einstellungen für verschiedene Domänenursprünge festlegen. Alle Einstellungen („Headers“, „Methoden“, „Max. Alter“ und „Anmeldeinformationen zulassen“) gelten für alle in der Einstellung „Ursprünge“ angegebenen Ursprünge.
