---
title: 'Erstellen eines Power BI-App-Angebots: Azure Marketplace | Microsoft-Dokumentation'
description: Informationen zum Erstellen eines Power BI-App-Angebots im Marketplace von Microsoft AppSource.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
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
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: 6a4f7daa337618278c3652fad3053c20557a9e28
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414687"
---
# <a name="create-a-power-bi-app-offer"></a>Erstellen eines Power BI-App-Angebots

In diesem Artikel werden die Schritte zum Erstellen eines neuen Power BI-App-Angebots für [AppSource](https://appsource.microsoft.com) beschrieben. Jedes Angebot wird in AppSource als eigene Entität angezeigt. Beim Erstellen eines neuen Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) müssen Sie für Ihr Angebot vier Gruppen von Ressourcen angeben.

Die Ressourcengruppen werden in der folgenden Tabelle beschrieben:

|   Ressourcengruppe      | BESCHREIBUNG                                                                         |
| ----------------   | ----------------                                                                    |
| Angebotseinstellungen     | Die primären Bezeichner und der Name für das Angebot.                                      |
| Technische Informationen     | Die Installationsprogramm-URL, über die Sie die App im Power BI-Arbeitsbereich des Clients installieren. Weitere Informationen zum Generieren dieser URL finden Sie in der [Dokumentation zu Power BI-Apps](https://go.microsoft.com/fwlink/?linkid=2028636). |
| Storefrontdetails | Enthält Ressourcen für Marketing, rechtliche Hinweise und die Leadverwaltung. Zu den Marketingressourcen zählen eine Angebotsbeschreibung und Logos. Zu den rechtlichen Ressourcen zählen eine Datenschutzrichtlinie, Nutzungsbedingungen und andere rechtliche Dokumente. Mit der Leadverwaltungsrichtlinie können Sie festlegen, wie Leads aus dem AppSource-Endbenutzerportal behandelt werden sollen. |
| Kontakte           | Enthält Supportkontakt- und Richtlinieninformationen.                                     |

## <a name="new-offer-form"></a>Formular „Neues Angebot“

Nachdem Sie sich beim Cloud-Partnerportal angemeldet haben, wählen Sie im linken Bereich die Option **Neues Angebot** aus. Wählen Sie dann **Power BI-Apps** aus, um das Formular „Neues Angebot“ anzuzeigen und mit dem Definieren von Ressourcen für ein neues App-Angebot zu beginnen.

![Menüelement „Power BI-Angebot“](./media/new-offer-menu.png)

> [!NOTE] 
> Wenn die Option **Power BI-Apps** nicht angezeigt oder nicht aktiviert ist, verfügt Ihr Konto nicht über die Berechtigung zum Erstellen dieses Angebotstyps. Überprüfen Sie, ob Sie alle [Voraussetzungen](./cpp-prerequisites.md) für diesen Angebotstyp erfüllen (einschließlich der Registrierung für ein Entwicklerkonto).


## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln werden die Registerkarten auf der Seite **Neues Angebot** für einen Power BI-App-Angebotstyp erläutert. In jedem dieser Artikel werden die Ressourcengruppen und unterstützenden Dienste für Ihr neues Power BI-App-Angebot beschrieben.

-  [Registerkarte „Angebotseinstellungen“](./cpp-offer-settings-tab.md)
-  [Registerkarte „Technische Informationen“](./cpp-technical-info-tab.md)
-  [Registerkarte „Storefront Details“ (Storefrontdetails)](./cpp-storefront-details-tab.md)
-  [Registerkarte „Kontakte“](./cpp-contacts-tab.md)
