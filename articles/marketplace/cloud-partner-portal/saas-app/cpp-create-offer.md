---
title: Erstellen eines Azure-SaaS-Anwendungsangebots | Microsoft-Dokumentation
description: Erstellen eines SaaS-Anwendungsangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: bcaf470fae34056fe932cf8b5fd3076899f895ae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880278"
---
# <a name="create-a-new-saas-application-offer"></a>Erstellen eines neuen SaaS-Anwendungsangebots

In diesem Artikel wird beschrieben, wie Sie einen Azure Marketplace-Eintrag für ein SaaS-Anwendungsangebot erstellen und veröffentlichen.

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
