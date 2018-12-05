---
title: Abonnements in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Konzept von Abonnements in Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 45a65c7a94f3e6917b2882f27c9ad7d764ef97d7
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621714"
---
# <a name="subscriptions-in-azure-api-management"></a>Abonnements in Azure API Management

Abonnements sind ein wichtiges Konzept in Azure API Management (APIM). Am häufigsten erhalten API-Nutzer Zugriff auf APIs, die über eine APIM-Instanz veröffentlicht wurden. Dieser Artikel bietet eine Übersicht über das Konzept.

## <a name="what-is-subscriptions"></a>Was sind Abonnements?

Beim Veröffentlichen von APIs über APIM ist die einfachste und gängigste Methode zum Schutz des Zugriffs auf diese APIs die Verwendung von Abonnementschlüsseln. Anders ausgedrückt: Entwickler, die die veröffentlichten APIs nutzen möchten, müssen einen gültigen Abonnementschlüssel in HTTP-Anforderungen einschließen, wenn Aufrufe an diese APIs gesendet werden. Andernfalls werden die Aufrufe sofort durch das APIM-Gateway abgelehnt und nicht an die Back-End-Dienste weitergeleitet.

Um einen Abonnementschlüssel für den API-Zugriff zu erhalten, ist ein Abonnement erforderlich. Ein Abonnement ist im Wesentlichen ein benannter Container für ein Paar von Abonnementschlüsseln. Abonnements können von Entwicklern abgerufen werden, die die veröffentlichten APIs nutzen möchten, mit oder ohne Genehmigung durch die API-Herausgeber. API-Herausgeber können Abonnements im Namen von API-Nutzern auch direkt erstellen.

> [!TIP]
> APIM bietet außerdem Unterstützung für weitere Mechanismen zum Schutz des API-Zugriffs, darunter [OAuth2.0](api-management-howto-protect-backend-with-aad.md), [Clientzertifikate](api-management-howto-mutual-certificates-for-clients.md) und [IP-Whitelists](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs).

## <a name="scope-of-subscriptions"></a>Bereich von Abonnements

Abonnements können verschiedenen Bereichen zugeordnet werden: einem Produkt, allen APIs oder einer bestimmten API.

### <a name="subscriptions-for-a-product"></a>Abonnements für ein Produkt

Traditionell wurden Abonnements in APIM als Bereich immer ein einzelnes [API-Produkt](api-management-terminology.md) zugeordnet. Entwickler konnten die Liste der Produkte im Entwicklerportal einsehen und Abonnementanforderungen für die Produkte übermitteln, die sie verwenden wollten. Nachdem eine Abonnementanforderung genehmigt wurde (entweder automatisch oder durch den API-Herausgeber), können die Entwickler die Schlüssel darin für den Zugriff auf alle APIs im Produkt verwenden.

![Produktabonnements](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> In bestimmten Szenarien möchten API-Herausgeber vielleicht ein API-Produkt für die Öffentlichkeit veröffentlichen, ohne dass hierzu Abonnements erforderlich sind. Sie können hierzu im Azure-Portal auf der Seite **Einstellungen** die Option **Abonnement erforderlich** deaktivieren. Als Ergebnis kann auf alle APIs für das Produkt ohne einen API-Schlüssel zugegriffen werden.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Abonnements für alle APIs oder eine einzelne API

> [!NOTE]
> Aktuell ist dieses Feature nur im API Management-Verbrauchstarif verfügbar.

Bei Einführung des [Verbrauchstarifs](https://aka.ms/apimconsumptionblog) für APIM wurde einige Änderungen vorgenommen, um die Schlüsselverwaltung zu optimieren. Zunächst wurden zwei weitere Abonnementbereiche hinzugefügt: alle APIs und eine einzelne API. Der Bereich von Abonnements ist nicht länger auf ein API-Produkt beschränkt. Es ist jetzt möglich, Schlüssel zur Zugriffserteilung für eine API (oder alle APIs innerhalb einer APIM-Instanz) zu erstellen, ohne dass zunächst ein Produkt erstellt und diesem die APIs hinzugefügt werden müssen. Darüber hinaus umfasst jede APIM-Instanz jetzt ein unveränderbares Abonnement für alle APIs für ein einfacheres Testen und Debuggen von APIs innerhalb der Testkonsole.

Außerdem lässt APIM jetzt „eigenständige“ Abonnements zu. Es ist nicht mehr erforderlich, dass Abonnements einem Entwicklerkonto zugeordnet sind. Dies ist insbesondere in Szenarien nützlich, in denen ein Abonnement von mehreren Entwicklern oder Teams gemeinsam genutzt wird.

Und schließlich können API-Herausgeber jetzt direkt im Azure-Portal [Abonnements erstellen](api-management-howto-create-subscriptions.md):

![Flexible Abonnements](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu API Management:

+ Informationen zu weiteren [Konzepten](api-management-terminology.md) in API Management
+ [Tutorials](import-and-publish.md) zum weiteren Kennenlernen von API Management
+ [FAQ-Seite](api-management-faq.md) für häufige Fragen