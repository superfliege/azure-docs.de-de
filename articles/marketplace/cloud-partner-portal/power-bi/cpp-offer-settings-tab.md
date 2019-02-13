---
title: Einstellungen für ein Power BI-App-Angebot – Azure Marketplace | Microsoft-Dokumentation
description: Informationen zum Konfigurieren von Angebotseinstellungen für ein Power BI-App-Angebot für den Marketplace von Microsoft AppSource.
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
ms.openlocfilehash: fef2455129d94913c5b51a08c04403834861bb48
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665811"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Registerkarte „Angebotseinstellungen“ für Power BI

Auf der Seite **Neues Angebot** für Dienstanwendungen wird als Erstes die Registerkarte **Angebotseinstellungen** angezeigt.  Hier geben Sie die primären Bezeichner und den Namen Ihres Angebots ein.  Mit einem an den Feldnamen angefügten Sternchen (*) wird angegeben, dass eine Eingabe erforderlich ist.

![Registerkarte „Angebotseinstellungen“](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Felder in „Angebotseinstellungen“ 

Auf der Registerkarte **Angebotseinstellungen** müssen folgende Felder ausgefüllt werden:

|  Feld        |  BESCHREIBUNG                                                               |
|---------------|----------------------------------------------------------------------------|
| **Angebots-ID**  | Ein (innerhalb eines Herausgeberprofils) eindeutiger Bezeichner für das Angebot. Dieser Bezeichner ist in Produkt-URLs, Resource Manager-Vorlagen und Abrechnungsberichten sichtbar. Er hat eine maximale Länge von 50 Zeichen und darf ausschließlich alphanumerische Kleinbuchstaben und Bindestriche (-) enthalten, aber nicht mit einem Bindestrich enden. Nachdem ein Angebot live geschaltet wurde, kann dieses Feld nicht mehr geändert werden. Wenn also beispielsweise Contoso ein Angebot mit der Angebots-ID `sample-SvcApp` veröffentlicht, wird ihm die AppSource-URL `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp` zugewiesen.      |
| **Herausgeber** | Der eindeutige Bezeichner Ihrer Organisation in [AppSource](https://appsource.microsoft.com). Alle Ihre Angebote sollten mit Ihrer Herausgeber-ID verknüpft sein. Nachdem das Angebot gespeichert wurde, kann dieser Wert nicht mehr geändert werden.                         |
| **Name**      | Der Anzeigenamen für Ihr Angebot. Dieser Name wird in AppSource und im Cloud-Partnerportal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Hier empfiehlt es sich, einen Markennamen mit gutem Wiedererkennungswert für Ihr Produkt zu verwenden. Geben Sie hier nicht den Namen Ihrer Organisation ein, es sei denn, die App wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf anderen Websites und in anderen Publikationen vermarkten, achten Sie darauf, überall den gleichen Namen zu verwenden.    <br/>Wenn Sie ein Angebot während des Vorschauzeitraums von Power BI-Apps freigeben, fügen Sie die Zeichenkette `(Preview)` an den Namen Ihrer Anwendung an, z.B. `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Nächste Schritte

Auf der nächsten Registerkarte geben Sie [technische Informationen](./cpp-technical-info-tab.md) für Ihr Angebot an.
