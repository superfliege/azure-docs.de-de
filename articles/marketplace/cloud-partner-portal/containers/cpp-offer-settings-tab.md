---
title: Angebotseinstellungen für ein Azure-Containerimage | Microsoft-Dokumentation
description: Konfigurieren der Angebotseinstellungen für einen Azure-Container.
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
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 9a669ebad674f938d11c8e372e6f586ca3c67f03
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980252"
---
# <a name="container-offer-settings-tab"></a>Registerkarte „Containerangebotseinstellungen“

Die Seite **Container > Neues Angebot** wird mit dem Fokus auf der Registerkarte **Angebotseinstellungen** geöffnet. 

![Angebots-ID](./media/containers-offer-settings.png)

## <a name="offer-identity-settings"></a>Einstellungen unter „Angebotsidentität“

Unter **Angebotsidentität** müssen Sie Informationen in den in der nachfolgenden Tabelle beschriebenen Feldern angeben. Mit einem Sternchen (*) gekennzeichnete Felder sind Pflichtfelder. 

|  **Feld**       |     **Beschreibung**                                                          |
|  ---------       |     ---------------                                                          |
| **Angebots-ID**       | Ein (innerhalb eines Herausgeberprofils) eindeutiger Bezeichner für das Angebot. Dieser Bezeichner wird in Produkt-URLs und Insights-Berichten angezeigt. Er hat eine maximale Länge von 50 Zeichen und kann alphanumerische Kleinbuchstaben und Bindestriche (-) enthalten. (Der Bezeichner darf nicht mit einem Bindestrich enden.) **Hinweis:** Nachdem ein Angebot live geschaltet wurde, kann dieses Feld nicht mehr geändert werden. <br> Wenn also beispielsweise Contoso ein Angebot mit der Angebots-ID **sample-container** veröffentlicht, wird ihm die Azure Marketplace-URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-container?tab=Overview` zugewiesen. |
| **Herausgeber-ID**     | Der eindeutige Bezeichner Ihrer Organisation im Azure Marketplace. Alle Ihre Angebote sollten mit Ihrer Herausgeber-ID verknüpft sein. Dieser Wert kann nicht mehr geändert werden, nachdem das Angebot gespeichert wurde. |
| **Name**          | Der Anzeigenamen für Ihr Angebot. Dieser Name wird im Azure Marketplace und im Cloud-Partnerportal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Es empfiehlt sich, einen Markennamen mit hohem Wiedererkennungswert für Ihr Produkt zu verwenden. Verwenden Sie nicht den Namen Ihrer Organisation, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf anderen Websites und in anderen Publikationen vermarkten, achten Sie darauf, überall exakt den gleichen Namen zu verwenden. |
|  |  |

Wählen Sie **Speichern** aus, um die Angebotseinstellungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie auf der Registerkarte [SKUs](./cpp-skus-tab.md) die SKUs für Ihr Angebot.
