---
title: Technische Informationen für ein Power BI-App-Angebot – Azure Marketplace | Microsoft-Dokumentation
description: Informationen zum Konfigurieren von Feldern des Typs „Technische Informationen“ für ein Power BI-App-Angebot für den Marketplace von Microsoft AppSource.
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
ms.openlocfilehash: d96ef2fd318d6164e1b7dfc5c4b72d6957af0f3e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744433"
---
# <a name="power-bi-apps-technical-info-tab"></a>Registerkarte „Technische Informationen“ für Power BI-Apps

Auf der Registerkarte **Technische Informationen** der Seite **Neues Angebot** geben Sie die URL des Power BI-Installationspakets und alle zusätzlichen Informationen an, die für die Überprüfung des neuen Angebots erforderlich sind.  Bei ihrer Erstveröffentlichung sind alle Power BI-Apps kostenlos und stehen in AppSource kostenlos zum Download bereit. Daher können Sie für diese Angebotsart keine Stock Keeping Units (SKUs) definieren.

![Registerkarte „Technische Informationen“](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Felder auf der Registerkarte „Technische Informationen“ 

Auf der Registerkarte **Technische Informationen** müssen Sie die folgenden Fehler ausfüllen.  Mit einem Sternchen (*) gekennzeichnete Felder sind Pflichtfelder.

|        Feld          |  Beschreibung                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installations-URL**     | Von Power BI generierte Adresse, wenn Sie die App veröffentlichen und in die Produktion höher stufen.  Weitere Informationen zum Generieren der URL finden Sie unter [Veröffentlichen von Dienst-Apps in Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Überprüfungsanweisungen**  |  Optionale Textanweisungen (max. 3.000 Zeichen) für das Microsoft-Überprüfungsteam zur Unterstützung beim Konfigurieren, Verbinden und Testen Ihrer App, einschließlich typischer Konfigurationseinstellungen, Testkonten oder Parameter, die zum Testen der Option „Daten verbinden“ verwendet werden können, usw. Diese Informationen werden nur dem Überprüfungsteam zur Verfügung gestellt und dienen nur zu Überprüfungszwecken.  |
| **Wird diese App als Power BI-Inhaltspaket erstellt?** | Derzeit ist dies ein intern verwendetes Feld. Ändern Sie den Standardwert `No` nicht in `Yes`, da dies die Veröffentlichung beeinträchtigen kann.  |  
|  |  |


## <a name="next-steps"></a>Nächste Schritte

Auf der nächsten Registerkarte ([Details der digitalen Ladenzeile](./cpp-storefront-details-tab.md)) geben Sie Marketing- und rechtliche Informationen zu Ihrer App an.

