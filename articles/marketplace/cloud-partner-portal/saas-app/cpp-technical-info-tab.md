---
title: Technische Konfiguration für ein Azure-SaaS-Anwendungsangebot | Azure Marketplace
description: Konfigurieren von technischen Informationen für SaaS-Anwendungsangebot im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 46dcf4aeb7ddb67028eb23dde9236f2b7709f86d
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941728"
---
# <a name="saas-application-technical-info-tab"></a>Registerkarte „Technische Informationen“ für SaaS-Anwendung

Die Registerkarte „Technische Informationen“ stellt das Formular für die technische Konfiguration bereit. Verwenden Sie dieses Formular, um den Typ der SaaS-Anwendung (App) auszuwählen, die Sie erstellen, und zu konfigurieren, wie Ihre App den Kunden bereitgestellt wird.

![Formular für technische Konfiguration](./media/saas-techinfo-techconfig.png)


## <a name="technical-configuration-form"></a>Formular „Technische Konfiguration“

Dieses Formular hat 2 Felder: „Produkt“ und „Handlungsaufforderung“.


### <a name="product-field"></a>Feld „Produkt“

Sie können eine SaaS-App für beide der folgenden digitalen Ladenzeilen (Storefronts) angeben:
- Für einen Geschäftskunden durch Auswählen der Option **Auflistung**.
- Für die ein IT-Administrator durch Auswählen von **Über Microsoft verkaufen**.
Damit Sie entscheiden können, welche Art von SaaS-App Sie erstellen, lesen Sie [Verstehen der Storefrontauswahl](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type#understand-storefront-selection).


#### <a name="sell-through-microsoft"></a>Über Microsoft verkaufen
Um diese Benutzeroberfläche zu erstellen, müssen Sie die folgenden Teile konfigurieren:

- Verbinden Sie die Website Ihres SaaS-Diensts mit den SaaS-APIs von Microsoft. Im Artikel [SaaS – Verkaufen über Azure: APIs](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-subscription-apis) ist erläutert, wie Sie diese Verbindung erstellen.
- Aktivieren Sie „Über Azure verkaufen“ im Cloud-Partnerportal im Formular „Technische Konfiguration“, und geben Sie die erforderlichen Informationen ein. Weitere Informationen über dieses Abrechnungsmodell und wie es implementiert wird finden Sie unter [SaaS – Verkaufen über Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-offer-subscriptions).

  ![Formular „Über Microsoft verkaufen“](./media/saas-techinfo-sellthrough-ms.png)

In der folgenden Tabelle sind die erforderlichen Felder für **Über Microsoft verkaufen** beschrieben.  Erforderliche Felder sind durch ein Sternchen (*) gekennzeichnet.

|  **Feldname**   |  **Beschreibung**  |
|  ---------------  |  ---------------  |
|  **Abonnement-IDs für die Vorschauversion\***   |  Diese Azure-Abonnement-IDs werden verwendet, um Ihr Angebot in der Vorschauversion zu testen, bevor es öffentlich verfügbar ist.  |
|  **Vorschau der AAD-/MSA-Konten\***   |  Die Azure AD-/MSA-Konten, durch Kommas getrennt, die auf die Vorschauversion zugreifen können. |
|  **Anweisungen für den Einstieg** |  Anweisungen für Ihre Kunden zum Herstellen einer Verbindung mit Ihrer SaaS-App. Einfache HTML-Tags zulässig ist, z. B.: &lt;p&gt;, &lt;h1&gt;, &lt;li&gt; usw.    |
|  **URL der Angebotsseite\***           |  Die URL Ihrer Website, auf die Sie Ihre Kunden nach dem Erwerb im Azure-Portal leiten. Diese URL wird auch der Endpunkt, der die Verbindungs-APIs für Geschäfte mit Microsoft empfängt.   |
| **Verbindungswebhook\***            |  Für alle asynchronen Ereignisse, die Microsoft im Auftrag des Kunden an Sie senden muss (Beispiel: das Azure-Abonnement ist ungültig geworden), müssen Sie einen Webhook für die Verbindung angeben. Wenn Sie noch nicht über ein Webhooksystem verfügen, ist die einfachste Konfiguration eine Logik-App am HTTP-Endpunkt, die auf alle veröffentlichten Ereignisse lauscht und sie entsprechend behandelt. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint">Aufrufen, Auslösen oder Schachteln von Workflows mit HTTP-Endpunkten in Logik-Apps</a>.    |
|  **Azure AD-Mandanten-ID\*** und **-App-ID\***      |   Es ist im Azure-Portal erforderlich, dass Sie eine Active Directory-App erstellen, damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Erstellen Sie für diese Felder eine AD-App, und fügen Sie die entsprechende Mandanten-ID und App-ID ein. Ihre App-ID ist Ihrer Herausgeber-ID zugeordnet. Stellen Sie daher sicher, dass Sie die gleiche App-ID wie in allen anderen Angeboten verwenden.   |
|   |   |

Schließlich gibt es, wenn Sie **Über Microsoft verkaufen** auswählen, eine weitere „Neues Angebot“-Registerkarte namens **Pläne**. 

Auf der [Registerkarte „Pläne“](./cpp-plans-tab.md) werden die von Ihrer SaaS-App unterstützten Tarife und die zugehörigen Preise aufgeführt. Bisher sind ausschließlich monatliche Preise mit der Möglichkeit des 1- bis 3-monatigen kostenlosen Zugriffs möglich. Diese Tarife und Preise sollten genau mit den Tarifen und Preisen auf der Website Ihrer SaaS-App übereinstimmen.

>[!NOTE] 
>Pläne sind nur erforderlich, wenn Sie „Über Microsoft verkaufen“ ausgewählt haben.

### <a name="call-to-action-field"></a>Feld „Handlungsaufforderung“

Im Feld „Handlungsaufforderung“ können Sie die Meldung auswählen, die auf der Schaltfläche für einen Kauf Ihres Angebots angezeigt wird. Die folgenden Optionen sind verfügbar:

- Free: Wenn Sie diese Option auswählen, werden Sie aufgefordert, eine Testversion-URL einzugeben, über die Kunden Zugriff auf Ihre SaaS-App erhalten. Beispiel: https://contoso.com/trial
- Kostenlose Testversion: Wenn Sie diese Option auswählen, werden Sie aufgefordert, eine Testversion-URL einzugeben, über die Kunden Zugriff auf Ihre SaaS-App erhalten. Beispiel: https://contoso.com/trial
- Kontakt mit mir aufnehmen

Weitere Informationen zu den Optionen von „Handlungsaufforderung“ finden Sie unter „Auswählen einer Veröffentlichungsoption“.

>[!Note]
>Die Nutzung des CSP-Partnerkanals (Cloud Solution Provider) ist jetzt verfügbar.  Unter [Cloud Solution Providers](../../cloud-solution-providers.md) finden Sie weitere Informationen zum Vermarkten Ihres Angebots über die Microsoft CSP-Partnerkanäle.

## <a name="next-steps"></a>Nächste Schritte

- [Registerkarte „Pläne“ (optional)](./cpp-plans-tab.md)
- [Registerkarte „Channel Info“ (Kanalinformationen)](./cpp-channel-info-tab.md)
