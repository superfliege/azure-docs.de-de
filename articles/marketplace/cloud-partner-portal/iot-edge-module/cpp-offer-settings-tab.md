---
title: Angebotseinstellungen für ein Azure IoT Edge-Modul | Microsoft-Dokumentation
description: Konfigurieren der Angebotseinstellungen für ein IoT Edge-Modul.
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
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 2a2ec41d7e2f21dc1550cd3c56009204842e1641
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49432633"
---
# <a name="iot-edge-module-offer-settings-tab"></a>Registerkarte „Angebotseinstellungen“ für ein IoT Edge-Modul

Die Seite **IoT Edge-Module > Neues Angebot** wird mit dem Fokus auf der Registerkarte **Angebotseinstellungen** geöffnet. 

![Seite „Neues Angebot“ für IoT Edge-Module](./media/iot-edge-module-offer-settings-tab.png)

## <a name="offer-identity-settings"></a>Einstellungen unter „Angebotsidentität“

Unter **Angebotsidentität** müssen Sie Informationen in den in der nachfolgenden Tabelle beschriebenen Feldern angeben. Mit einem Sternchen (*) gekennzeichnete Felder sind Pflichtfelder. 

|  **Feld**       |     **Beschreibung**                                                          |
|  ---------       |     ---------------                                                          |
| **Angebots-ID**       | Ein (innerhalb eines Herausgeberprofils) eindeutiger Bezeichner für das Angebot. Dieser Bezeichner wird in Produkt-URLs und Insights-Berichten angezeigt. Er hat eine maximale Länge von 50 Zeichen und kann alphanumerische Kleinbuchstaben und Bindestriche (-) enthalten. (Der Bezeichner darf nicht mit einem Bindestrich enden.) **Hinweis:** Nachdem ein Angebot live geschaltet wurde, kann dieses Feld nicht mehr geändert werden. <br> Wenn also beispielsweise Contoso ein Angebot mit der Angebots-ID **sample-iot-edge-module** veröffentlicht, wird ihm die Azure Marketplace-URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-iot-edge-module?tab=Overview` zugewiesen. |
| **Herausgeber**     | Der eindeutige Bezeichner Ihrer Organisation im Azure Marketplace. Alle Ihre Angebote sollten mit Ihrer Herausgeber-ID verknüpft sein. Dieser Wert kann nicht mehr geändert werden, nachdem das Angebot gespeichert wurde. |
| **Name**          | Der Anzeigenamen für Ihr Angebot. Dieser Name wird im Azure Marketplace und im Cloud-Partnerportal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Es empfiehlt sich, einen Markennamen mit hohem Wiedererkennungswert für Ihr Produkt zu verwenden. Verwenden Sie nicht den Namen Ihrer Organisation, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf anderen Websites und in anderen Publikationen vermarkten, achten Sie darauf, überall exakt den gleichen Namen zu verwenden. |
|  |  |

Wählen Sie **Speichern** aus, um die Angebotseinstellungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie auf der Registerkarte [SKUs](./cpp-skus-tab.md) die SKUs für Ihr Angebot.
