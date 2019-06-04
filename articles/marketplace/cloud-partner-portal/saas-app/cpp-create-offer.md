---
title: Erstellen eines Azure SaaS-Anwendungsangebots | Azure Marketplace
description: Erstellen eines SaaS-Anwendungsangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 646e9bf844477b3d8e1c4c42fb5956e015805433
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833498"
---
# <a name="create-a-new-saas-application-offer"></a>Erstellen eines neuen SaaS-Anwendungsangebots

In diesem Artikel wird beschrieben, wie Sie einen Azure Marketplace-Eintrag für ein SaaS-Anwendungsangebot erstellen und veröffentlichen.

> [!IMPORTANT] 
> Die SaaS-Angebotsfunktionalität wird zum [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory) migriert.  Alle neuen Herausgeber müssen das Partner Center für die Erstellung neuer SaaS-Angebote und die Verwaltung bestehender Angebote verwenden.  Aktuelle Herausgeber mit SaaS-Angeboten werden schrittweise vom Cloud-Partnerportal zum Partner Center migriert.  Das Cloud-Partnerportal zeigt Statusmeldungen an, um anzugeben, wann bestimmte bestehende Angebote migriert wurden.


## <a name="offer-process"></a>Angebotsprozess

In der nächsten Abbildung ist der Prozess für das Erstellen eines SaaS-Anwendungsangebots dargestellt.

![Prozess für das Erstellen eines SaaS-Anwendungsangebots](./media/saas-offer-process-overview.png)

## <a name="offer-components"></a>Angebotskomponenten

Ein SaaS-Anwendungsangebot besteht aus fünf Abschnitten, die in der folgenden Tabelle beschrieben sind:

|  **Ressourcengruppe**   |  **Beschreibung**  |
|  ---------------   |  ---------------  |
|    Angebotseinstellungen  |  Wird dazu verwendet, eine eindeutige Identität für die SaaS-Anwendung (App) zu konfigurieren.                 |
|  Technische Informationen    |  Wird dazu verwendet, den Typ der SaaS-Lösung zu konfigurieren und die Verbindungsdetails für Ihre Anwendung bereitzustellen.                |
|  Kanalinformationen      |   Geben Sie Kanalinformationen an, etwa GTM-Materialien und Kontakte.                |
|  Testversion        |   Optionaler Abschnitt zum Definieren eines Diensts, mit dem Kunden Ihr Angebot testen können, bevor sie es erwerben.                |
|  Storefrontdetails       | Enthält Ressourcen für Marketing, rechtliche Aspekte und Leadverwaltung sowie entsprechende Spezifikationen.  <ul><li> Zu Marketingressourcen zählen unter anderem der Name des Angebots, eine Beschreibung und Logos.</li> <li> Zu den rechtlichen Ressourcen zählen eine Datenschutzrichtlinie, Nutzungsbedingungen und andere rechtliche Dokumentationen.</li>  <li> Mit der Leadverwaltungsrichtlinie können Sie angeben, wie Leads aus dem Azure Marketplace-Endbenutzerportal verwendet werden sollen.</li> </ul> |
| Kontakte            | Enthält Kontakt- und Richtlinieninformationen. |

## <a name="new-offer-form"></a>Formular „Neues Angebot“

Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an, und wählen Sie dann in der linken Menüleiste die Option **+ Neues Angebot**. Wählen Sie im Menü „Neues Angebot“ die Option **SaaS-Apps** aus, um das Formular „Neues Angebot“ anzuzeigen und mit dem Definieren von Ressourcen für ein neues SaaS-Anwendungsangebot zu beginnen.

![Menü „Neues Angebot“ für SaaS-Apps](./media/azure-new-saas-offer.png)

## <a name="next-steps"></a>Nächste Schritte

Die Seite „Neues Angebot“ für den SaaS-Angebotstyp enthält eine Reihe von Registerkarten und Formularfeldern zum Erstellen eines neuen Angebots. In den folgenden Artikeln wird erläutert, wie Sie auf der jeweiligen Registerkarte die Ressourcengruppen und unterstützenden Dienste für Ihr neues Angebot definieren.

- [Registerkarte „Angebotseinstellungen“](./cpp-offer-settings-tab.md)
- [Registerkarte „Technische Informationen“](./cpp-technical-info-tab.md)
- [Registerkarte „Channel Info“ (Kanalinformationen)](./cpp-channel-info-tab.md)
- [Registerkarte „Testversion“](./cpp-testdrive-tab.md)
- [Registerkarte „Storefront Details“ (Storefrontdetails)](./cpp-storefront-tab.md)
- [Registerkarte „Kontakte“](./cpp-contacts-tab.md)
