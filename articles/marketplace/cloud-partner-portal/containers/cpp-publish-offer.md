---
title: Veröffentlichen eines Azure-Containerimageangebots | Microsoft-Dokumentation
description: Informationen zum Veröffentlichen eines Azure-Containerangebots.
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
ms.openlocfilehash: 7533d1a133c9c474bc39f0f64c5f1a8183ab30f8
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980216"
---
# <a name="publish-container-offer"></a>Veröffentlichen eines Containerangebots

 Nachdem Sie über **Neues Angebot** ein neues Angebot erstellt haben, können Sie es veröffentlichen. Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsprozess zu starten.

Das folgende Diagramm zeigt die wichtigsten Schritte des Veröffentlichungsprozesses, die ausgeführt werden müssen, um ein Angebot live zu schalten.

![Schritte für die Veröffentlichung eines Containerangebots](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Ausführliche Beschreibung der Veröffentlichungsschritte

In der folgenden Tabelle werden die einzelnen Veröffentlichungsschritte beschrieben. Außerdem wir eine Schätzung angegeben, wie viel Zeit für jeden Schritt erforderlich sein wird.


|  **Veröffentlichungsschritt**           | **Time**    | **Beschreibung**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Überprüfen der Voraussetzungen         | 15 Min.   | Die Angebotsinformationen und -einstellungen werden überprüft.                        |
| Zertifizierung                  | 1 Woche | Das Angebot wird vom Azure-Zertifizierungsteam analysiert. Das Angebot wird auf Viren, Schadsoftware, Sicherheitskonformität und Sicherheitsprobleme untersucht. Das Angebot wird überprüft, um festzustellen, ob es die Eignungskriterien erfüllt. Weitere Informationen finden Sie unter [Voraussetzungen](./cpp-prerequisites.md) und [Vorbereiten Ihrer technischen Ressourcen](./cpp-create-technical-assets.md). Sollte ein Problem gefunden werden, erhalten Sie entsprechendes Feedback. |
| Verpackung | 1 Stunde  | Die technischen Ressourcen des Angebots werden für die Verwendung durch die Kunden gepackt, und die Leadsysteme werden konfiguriert und eingerichtet. |
|  Abzeichnung des Verlegers             |  -        | Letzte Überprüfung und Bestätigung durch den Herausgeber, bevor das Angebot live geschaltet wird. Sie können Ihr Angebot in den ausgewählten Abonnements (in den Schritten für die Angebotsinformationen) bereitstellen, um sicherzugehen, dass es alle Ihre Anforderungen erfüllt.  Klicken Sie auf **Live schalten**, damit der nächste Schritt für Ihr Angebot ausgeführt werden kann. |
| Verpackung                 | 1 Stunde | Das fertige Angebot wird in den Produktionssystemen und Regionen des Marketplace repliziert. | 
| Live                           | 4 Tage |Das Angebot wird veröffentlicht, in den erforderlichen Regionen repliziert und für die Öffentlichkeit verfügbar gemacht. |

Es kann bis zu zehn Werktage dauern, bis der Veröffentlichungsprozess abgeschlossen ist und das Angebot veröffentlicht wird. Nach Abschluss des Veröffentlichungsprozesses wird Ihr Containerangebot im [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) gelistet.

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren eines vorhandenen Containerangebots im Azure Marketplace](./cpp-update-existing-offer.md)
