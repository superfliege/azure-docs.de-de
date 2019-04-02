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
ms.openlocfilehash: 49bbe5dcf17a9aa20cb47f477c59e7115d29dacc
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819598"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Registerkarte „Angebotseinstellungen“ für Power BI

Beim Öffnen der Seite **Neues Angebot** für Dienst-Apps wird zuerst die Registerkarte **Angebotseinstellungen** angezeigt. Auf dieser Registerkarte geben Sie die primären Bezeichner und den Namen für Ihr Angebot ein. Ein Sternchen (*) kennzeichnet ein erforderliches Feld.

![Registerkarte „Angebotseinstellungen“](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Felder von „Angebotseinstellungen“ 

Auf der Registerkarte **Angebotseinstellungen** müssen Sie Informationen in folgende Felder eingeben:

|  Feld        |  BESCHREIBUNG                                                               |
|---------------|----------------------------------------------------------------------------|
| **Angebots-ID**  | Ein (innerhalb eines Herausgeberprofils) eindeutiger Bezeichner für das Angebot. Dieser Bezeichner ist in Produkt-URLs, Azure Resource Manager-Vorlagen und Abrechnungsberichten sichtbar. Die maximale Länge beträgt 50 Zeichen. Er darf nur aus klein geschriebenen alphanumerischen Zeichen und Bindestrichen (-) bestehen. Er darf nicht mit einem Bindestrich enden. Dieser Bezeichner kann nicht mehr geändert werden, nachdem das Angebot live geschaltet wurde. Wenn Contoso ein Angebot mit der Angebots-ID `sample-SvcApp` veröffentlicht, wird dem Angebot die AppSource-URL `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp` zugewiesen.      |
| **Herausgeber** | Der eindeutige Bezeichner Ihrer Organisation in [AppSource](https://appsource.microsoft.com). Alle Ihre Angebote sollten mit Ihrer Herausgeber-ID verknüpft sein. Dieser Wert kann nicht mehr geändert werden, nachdem das Angebot gespeichert wurde.                         |
| **Name**      | Der Anzeigenamen für Ihr Angebot. Dieser Name wird in AppSource und im Cloud-Partnerportal angezeigt. Die maximale Länge beträgt 50 Zeichen. Verwenden Sie einen Markennamen mit Wiedererkennungswert für Ihr Produkt. Geben Sie hier nicht den Namen Ihrer Organisation ein, es sei denn, die App wird unter diesem Namen vermarktet. Verwenden Sie immer denselben Namen, wenn Sie dieses Angebot auf anderen Websites und in Veröffentlichungen bereitstellen.    <br/>Wenn Sie ein Angebot während des Vorschauzeitraums von Power BI-Apps freigeben, fügen Sie die Zeichenfolge `(Preview)` an den Namen Ihrer Anwendung an, z.B. `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Nächste Schritte

Auf der nächsten Registerkarte geben Sie [technische Informationen](./cpp-technical-info-tab.md) für Ihr Angebot an.
