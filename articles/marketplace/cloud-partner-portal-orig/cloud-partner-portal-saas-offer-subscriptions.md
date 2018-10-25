---
title: SaaS – Verkaufen über Azure | Microsoft-Dokumentation
description: Beschreibt das Abrechnungsmodell für Abonnements für SaaS-Anwendungen und seine Implementierung.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: f193138fbc5e779c3a6671757320d8918e08db46
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806446"
---
<a name="saas---sell-through-azure"></a>SaaS – Verkaufen über Azure
=========================

Dieser Artikel beschreibt das Abonnementabrechnungsmodell für SaaS-Anwendungen und seine Implementierung im Cloud-Partnerportal.


<a name="overview"></a>Übersicht
--------

Sie können Ihre SaaS-Angebote im Azure Marketplace veröffentlichen und verkaufen. Ihre Kunden können nun direkt über die Azure-Abrechnung bezahlen und erhalten dann nur eine Rechnung von Azure für alle Ressourcen und Dienste, die sie aktiviert haben. Für die Veröffentlichung bieten SaaS-Abonnements die folgenden Vorteile:

-   **Einheitliche Oberfläche für die Veröffentlichung:** Wenn Sie bereits SaaS-Angebote oder andere Angebote im Azure Marketplace veröffentlichen, können Sie für die Veröffentlichung weiter das gleiche Veröffentlichungsportal verwenden.
-   **Neue Storefront für die Suche:** Alle veröffentlichten Angebote werden sowohl in der externen Storefront von Azure Marketplace als auch in der Azure Marketplace-Erweiterung im Azure-Portal angezeigt.
-   **Integrierte Abrechnung:** Sie können jetzt in allen Regionen, in denen Azure verfügbar ist, verkaufen und Azure die Abrechnung überlassen.
-   **Integrierte Leadgenerierung:** Sie können nun automatisch Leads von Azure im CRM Ihrer Wahl erhalten, wenn ein Azure-Benutzer Ihren SaaS-Dienst über den Azure Marketplace abonniert.
-   **Co-Selling mit Microsoft-Verkäufern:** Qualifizieren Sie sich für bidirektionales Lead Sharing, priorisierte Kataloglistings und die Möglichkeit, gemeinsame Geschäfte mit Microsoft-Verkäufern zu tätigen und abzuschließen.

<a name="billing-models"></a>Abrechnungsmodelle
--------------

Das einzige derzeit unterstützte Abrechnungsmodell ist eine pauschale monatliche Gebühr pro Abonnement Ihres SaaS-Diensts.

**Hinweis:** Zusätzliche Abrechnungsmodelle werden möglicherweise zu einem späteren Zeitpunkt verfügbar gemacht.

Jedes SaaS-Angebot kann einen oder mehrere Tarife (z.B. Basic oder Premium) umfassen. Jedem Tarif sind einige grundlegende Metadaten und ein Preis zugeordnet.

Sie haben vollständige Kontrolle über die Definition eines Plans. Dies gilt z.B. für den Umfang eines Basic-Tarifs. Der Tarif wird von Ihnen definiert, und Sie können beschreibenden Text im Rahmen der Veröffentlichung Ihres Plans angeben.

Der Preis, der dem Tarif zugeordnet ist, ist eine pauschale monatliche Gebühr, die Azure-Benutzer für das Verwenden Ihres Diensts bezahlen.

### <a name="what-is-not-supported-today"></a>Was wird bisher nicht unterstützt?

-   Abrechnung basierend auf benutzerdefinierten Einheiten, z.B. ein Festpreis basierend auf der Anzahl von Anforderungen.
-   Abrechnung basierend auf Arbeitsplatzzuweisungen, z.B. der Erwerb von Lizenzen durch Azure Benutzer basierend auf der Anzahl von Benutzern.

<a name="end-to-end-flow"></a>Der vollständige Ablauf
---------------

Zunächst wird der Ablauf für ein SaaS-Abonnementangebot aus Sicht der Endbenutzer beschrieben.

**Hinweis:** Bei dieser Ablaufbeschreibung wird davon ausgegangen, dass Sie Ihr SaaS-Angebot im Azure Marketplace unter dem Namen „Contoso demo SaaS“ veröffentlicht haben.

![Ablauf für SaaS-Abonnementbenutzer](media/saas-offer-subscription/saas-subscription-user-flow.png)

Ein Azure-Benutzer kann auf folgende Weise mit dem SaaS-Dienst interagieren:

-   Suchen Ihres SaaS-Diensts im Azure Marketplace
-   Abonnieren Ihres SaaS-Diensts in Azure
-   Navigieren zum SaaS-Dienst im Azure-Portal
-   Erstellen eines Kontos beim SaaS-Dienst und Verwalten des Kontos (Ändern/Löschen des Tarifs) in SaaS
-   Kündigen des Abonnements für den SaaS-Dienst im Azure-Portal

<a name="discover-your-saas-service-in-azure-marketplace"></a>Suchen Ihres SaaS-Diensts im Azure Marketplace
-----------------------------------------------

Wenn Benutzer den Azure Marketplace im Azure-Portal starten, wird eine Kategorie namens „Software-as-a-Service (SaaS)“ angezeigt.

![Suchen des SaaS-Diensts über das Kategoriemenü](media/saas-offer-subscription/saas-category-menu.png)

Benutzer können auch nach dem SaaS-Dienst suchen.

![Suchen des SaaS-Diensts über die Suchfunktion](media/saas-offer-subscription/saas-cpp-search.png)

Benutzer können Details zu Ihrem Dienst anzeigen und dann auf **Erstellen** klicken.

![Erstellen eines SaaS-Abonnements](media/saas-offer-subscription/saas-create-subscription.png)

### <a name="subscribe-to-saas-service-in-azure"></a>Abonnieren des SaaS-Diensts in Azure

Benutzer können den SaaS-Dienst dann im Azure-Portal abonnieren.

![Abonnieren eines SaaS-Diensts](media/saas-offer-subscription/saas-subscribe-signup.png)

Wenn ein Benutzer Ihren SaaS-Dienst abonniert, gibt er die folgende Informationen an:

-   Name: der Name, unter dem Benutzer diese SaaS-Abonnementinstanz in Azure suchen und verwalten können
-   Abonnement: der Abonnementkontext, dem sie die Abrechnung für den SaaS-Dienst zuordnen möchten
-   Tarif: der SaaS-Diensttarif, den sie abonnieren möchten

Das Dokument mit den Nutzungsbedingungen wird den Benutzern als Teil der Angebotsveröffentlichung ebenfalls angezeigt, bevor sie den SaaS-Dienst abonnieren.

Der Benutzer kann jetzt den Dienst abonnieren, indem er auf **Abonnieren** klickt.
Azure sendet im Portal eine Benachrichtigung, nachdem das Abonnement abgeschlossen wurde.

### <a name="navigate-to-the-saas-service-from-azure-portal"></a>Navigieren zum SaaS-Dienst im Azure-Portal

Benutzer klicken dann auf **Zu Ressource wechseln**, um ihr SaaS-Abonnement anzuzeigen oder zu verwalten.

![Anzeigen oder Verwalten Ihrer SaaS-Instanz](media/saas-offer-subscription/saas-go-to-resource.png)

Die Benutzer werden benachrichtigt, dass sie zuerst das Konto im SaaS-Dienst konfigurieren müssen. Ihre Abrechnung beginnt, wenn der SaaS-Dienst Azure benachrichtigt, dass die Abrechnung starten soll. Dies erfolgt, wenn der Benutzer ein Konto auf der Website des SaaS-Diensts erstellt.

![Konfigurieren Ihrer SaaS-Instanz](media/saas-offer-subscription/saas-configure-account.png)

Wenn Benutzer auf **Konto konfigurieren** klicken, werden sie zum SaaS-Dienstendpunkt umgeleitet. Während dieser Umleitung wird ein Token als Abfrageparameter übergeben. Beispiel: 

![SaaS-Kontotoken](media/saas-offer-subscription/saas-account-token.png)

Beachten Sie den Tokenwert. Dieses Token ist kurzlebig und muss aufgelöst werden, um in Azure eine Abonnement-ID zu erhalten.

<a name="creating-a-saas-offer-subscription"></a>Erstellen eines SaaS-Angebotsabonnements
----------------------------------

Um diese Funktion zu erstellen, sind zwei Arbeitsschritte erforderlich:

-   Verbinden Sie die Website Ihres SaaS-Diensts über die SaaS-APIs von Microsoft. Im Dokument zum [SaaS-Verkauf über Azure – APIs](./cloud-partner-portal-saas-subscription-apis.md) wird erläutert, wie Sie diese Verbindung erstellen.  
-   Aktivieren Sie im Cloud-Partnerportal **Sell through Azure** (Über Azure verkaufen) im Abschnitt **Technische Informationen**, und geben Sie alle Konfigurationsdetails an.

![Technische Informationen für das neue SaaS-Angebot](media/cpp-creating-saas-offers/saas-new-offer-technical-info-2.png)

Im Folgenden finden Sie eine Erläuterung aller erforderlichen Felder im Abschnitt **Technische Informationen**:

|  **Angebotsfelder**                 |  **Beschreibung**                                   |
|  ----------------                 |  -------------------------------------             |
| Abonnement-IDs für die Vorschauversion          | Diese Azure-Abonnement-IDs werden verwendet, um Ihr Angebot in der Vorschauversion zu testen, bevor es öffentlich verfügbar ist. |
| Anweisungen für den Einstieg      | Anweisungen für Ihre Kunden zum Herstellen einer Verbindung mit Ihrer SaaS-App. Eine grundlegende HTML-Verwendung ist zulässig, z.B. von Tags wie `<p>`, `<h1>`, `<li>` usw.  |
| URL der Angebotsseite                  | Die URL Ihrer Website, auf die Sie Ihre Kunden nach dem Erwerb im Azure-Portal leiten. Diese URL wird auch der Endpunkt, der die Verbindungs-APIs für Geschäfte mit Microsoft empfängt.  |
| Verbindungswebhook                | Für alle asynchronen Ereignisse, die Microsoft im Auftrag des Kunden an Sie senden muss (Beispiel: das Azure-Abonnement ist ungültig geworden), müssen Sie einen Webhook für die Verbindung angeben. Wenn Sie noch nicht über ein Webhooksystem verfügen, ist die einfachste Konfiguration eine Logik-App am HTTP-Endpunkt, die auf alle veröffentlichten Ereignisse lauscht und sie entsprechend behandelt.  Weitere Informationen finden Sie unter [Aufrufen, Auslösen oder Schachteln von Workflows mit HTTP-Endpunkten in Logik-Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint). |
| Azure AD-Mandanten-ID und -App-ID     | Es ist im Azure-Portal erforderlich, dass Sie eine Active Directory-App erstellen, damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Erstellen Sie für diese Felder eine AD-App, und fügen Sie die entsprechende Mandanten-ID und App-ID ein. |
|  |  |


Wenn Sie abschließend **Sell through Azure** (Über Azure verkaufen) auswählen, wird der Abschnitt **Tarife** hinzugefügt. Tarife sind nur erforderlich, wenn der Verkauf über Azure ausgewählt wurde. In diesem Abschnitt werden die von Ihrer SaaS-App unterstützten Tarife und die zugehörigen Preise aufgeführt. Bisher sind ausschließlich monatliche Preise mit der Möglichkeit des 1- bis 3-monatigen kostenlosen Zugriffs möglich. Diese Tarife und Preise sollten genau mit den Tarifen und Preisen auf der Website Ihrer SaaS-App übereinstimmen.
