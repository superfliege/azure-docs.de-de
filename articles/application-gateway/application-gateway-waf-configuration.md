---
title: 'Web Application Firewall: Anforderungsgrößenlimits und Ausschlusslisten in Azure Application Gateway – Azure-Portal | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zur Konfiguration von Anforderungsgrößenlimits und Ausschlusslisten für die Web Application Firewall (WAF) in Application Gateway mit dem Azure-Portal.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: victorh
ms.openlocfilehash: 995e003422d5a94fe57174dc9733c870e4e003aa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965503"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists-public-preview"></a>WAF-Anforderungsgrößenlimits und Ausschlusslisten (Öffentliche Vorschau)

Die Web Application Firewall (WAF) von Azure Application Gateway bietet Schutz für Webanwendungen. Dieser Artikel beschreibt die Konfiguration von WAF-Anforderungsgrößenlimits und -Ausschlusslisten.

> [!IMPORTANT]
> Die Konfiguration von WAF-Anforderungsgrößenlimits und -Ausschlusslisten befindet sich derzeit in der öffentlichen Vorschau. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="waf-request-size-limits"></a>WAF-Anforderungsgrößenlimits
Die Web Application Firewall ermöglicht Benutzern das Konfigurieren von Größenlimits mit Ober- und Untergrenzen. Die folgenden zwei Konfigurationen für Größenlimits sind verfügbar:
- Der Wert im Feld für die maximale Größe des Anforderungstexts wird in KB angegeben und bestimmt das Anforderungsgrößenlimit insgesamt mit Ausnahme von Dateiuploads. In diesem Feld können Werte von mindestens 1 KB bis maximal 128 KB angegeben werden. Der Standardwert für die Größe des Anforderungstexts beträgt 128 KB.
- Der Wert im Feld für das Dateiuploadlimit wird in MB angegeben und bestimmt die maximal zulässige Größe für Dateiuploads. In diesem Feld können Werte von mindestens 1 MB bis maximal 500 MB angegeben werden. Der Standardwert für die Größe von Dateiuploads beträgt 100 MB.

Die WAF bietet außerdem einen konfigurierbaren Knopf zum Aktivieren bzw. Deaktivieren der Anforderungstextüberprüfung. Standardmäßig ist die Überprüfung des Anforderungstexts aktiviert. Ist die Überprüfung des Anforderungstexts deaktiviert, wertet die WAF die Inhalte des HTTP-Nachrichtentexts nicht aus. In diesem Fall erzwingt die WAF weiterhin WAF-Regeln für Header, Cookies und den URI. Ist die Überprüfung des Anforderungstexts deaktiviert, ist das Feld für die maximale Größe des Anforderungstexts nicht verfügbar, und es kann kein Wert festgelegt werden. Wenn Sie die Überprüfung des Anforderungstexts deaktivieren, können Nachrichten, die größer als 128 KB sind, an die WAF gesendet werden. Der Nachrichtentext wird jedoch nicht auf Sicherheitsrisiken überprüft.

## <a name="waf-exclusion-lists"></a>WAF-Ausschlusslisten

Mit WAF-Ausschlusslisten können Benutzer bestimmte Anforderungsattribute in einer WAF-Auswertung weglassen. Ein gängiges Beispiel sind von Active Directory eingefügte Token, die für Authentifizierungs- oder Kennwortfelder verwendet werden. Diese Attribute enthalten häufig Sonderzeichen, die ein falsch positives Ergebnis von den WAF-Regeln auslösen können. Wenn ein Attribut zur WAF-Ausschlussliste hinzugefügt wird, wird es von konfigurierten und aktiven WAF-Regeln nicht berücksichtigt. Ausschlusslisten gelten global.
Zu WAF-Ausschlusslisten können Anforderungsheader und -cookies oder Abfragezeichenfolgenargumente für Anforderungen hinzugefügt werden. Sie können eine exakte Übereinstimmung für Anforderungsheader, Anforderungscookies oder Abfragezeichenfolgenattribute oder optional partielle Übereinstimmungen angeben. Für Übereinstimmungskriterien werden die folgenden Operatoren unterstützt: 
- **Equals** (gleich): Dieser Operator wird für exakte Übereinstimmungen verwendet. Verwenden Sie beispielsweise zum Auswählen des Headers **bearerToken** den Operator „equals“ mit dem Selektor **bearerToken**. 
- **Starts with** (beginnt mit): Dieser Operator gleicht alle Felder ab, die mit dem angegebenen Selektorwert beginnen. 
- **Ends with** (endet mit): Dieser Operator gleicht alle Anforderungsfelder ab, die auf den angegebenen Selektorwert enden. 
- **Contains** (enthält): Dieser Operator gleicht alle Anforderungsfelder ab, die den angegebenen Selektorwert enthalten.

In allen Fällen muss beim Abgleich die Groß-/Kleinschreibung beachtet werden, und reguläre Ausdrücke sind nicht als Selektor zulässig.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren Ihrer WAF-Einstellungen können Sie sich darüber informieren, wie Sie WAF-Protokolle anzeigen. Weitere Informationen finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md#diagnostic-logging).