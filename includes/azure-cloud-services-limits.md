---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: acf5a7e8d7feebaac3d82629c5b561a22112de0f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553592"
---
| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| [Web- oder Workerrollen pro Bereitstellung](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Instanz-Eingabeendpunkte](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) pro Bereitstellung |25 |25 |
| [Eingabeendpunkte](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) pro Bereitstellung |25 |25 |
| [Interne Endpunkte](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) pro Bereitstellung |25 |25 |
| [Zertifikate für gehostete Dienste](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) pro Bereitstellung |199 |199 |

<sup>1</sup>Jeder Azure-Clouddienst mit Web- oder Workerrollen kann zwei Bereitstellungen haben: je eine für Produktions- und Stagingumgebung. Dieser Grenzwert bezieht sich auf die Anzahl der unterschiedlichen Rollen, also Konfigurationen. Dieser Grenzwert bezieht sich nicht auf die Anzahl der Instanzen pro Rolle, d.h. die Skalierung.

