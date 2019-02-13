---
title: Erstellen eines Power BI-Anwendungsangebots – Azure Marketplace | Microsoft-Dokumentation
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
ms.openlocfilehash: e0e1311276b858f1ac16fe6f17ccad49dd9901c9
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665931"
---
# <a name="create-a-power-bi-application-offer"></a>Erstellen eines Power BI-Anwendungsangebots

In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie ein neues Power BI-App-Angebot für [AppSource](https://appsource.microsoft.com) erstellen. Jedes Angebot wird in AppSource als eigene Entität angezeigt.  Beim Erstellen eines neuen Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) müssen Sie für Ihr Angebot vier Gruppen von Ressourcen angeben.

|   Ressourcengruppe      | Beschreibung                                                                         |
| ----------------   | ----------------                                                                    |
| Angebotseinstellungen     | Primäre Bezeichner und Namen für das Angebot                                      |
| Technische Informationen     | Die Installations-URL zum Installieren der App im Power BI-Arbeitsbereich des Clients. Weitere Informationen zum Generieren dieser URL finden Sie in der [Dokumentation zu Power BI-Apps](https://go.microsoft.com/fwlink/?linkid=2028636).   |
| Details der digitalen Ladenzeile | Enthält Ressourcen für Marketing, rechtliche Aspekte und Leadverwaltung. Zu Marketingressourcen zählen eine Angebotsbeschreibung und Logos.  Zu den rechtlichen Ressourcen zählen eine Datenschutzrichtlinie, Nutzungsbedingungen und andere rechtliche Dokumente.  Mit der Leadverwaltungsrichtlinie können Sie angeben, wie mit Leads aus dem AppSource-Endbenutzerportal umgegangen werden soll. |
| Kontakte           | Enthält Kontakt- und Richtlinieninformationen.                                     |
|    |     |


## <a name="new-offer-form"></a>Formular „Neues Angebot“

Klicken Sie nach der Anmeldung beim Cloud-Partnerportal auf der linken Menüleiste auf **+ Neues Angebot**.  Klicken Sie im daraufhin angezeigten Menü auf **Power BI-Apps**, um das Formular **Neues Angebot** anzuzeigen und mit dem Definieren von Ressourcen für ein neues App-Angebot zu beginnen.

![Menüelement „Power BI-Angebot“](./media/new-offer-menu.png)

> [!WARNING] 
> Wenn die Option **Power BI-Apps** nicht angezeigt oder nicht aktiviert ist, hat Ihr Konto nicht die Berechtigung zum Erstellen dieses Angebotstyps. Überprüfen Sie, ob Sie alle [Voraussetzungen](./cpp-prerequisites.md) für diesen Angebotstyp erfüllen (einschließlich der Registrierung für ein Entwicklerkonto).


## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln werden die Registerkarten der Seite **Neues Angebot** (für den Angebotstyp „Power BI-App“) erläutert. In den Artikeln erfahren Sie, wie Sie auf den jeweiligen Registerkarte die Ressourcengruppen und unterstützenden Dienste für Ihr neues App-Angebot definieren.

-  [Registerkarte „Angebotseinstellungen“](./cpp-offer-settings-tab.md)
-  [Registerkarte „Technische Informationen“](./cpp-technical-info-tab.md)
-  [Registerkarte „Details der digitalen Ladenzeile“](./cpp-storefront-details-tab.md)
-  [Kontakte](./cpp-contacts-tab.md)
