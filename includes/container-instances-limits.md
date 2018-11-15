---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572164"
---
| Ressource | Standardlimit |
| --- | :--- |
| Containergruppen pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Anzahl von Containern pro Containergruppe | 60 |
| Anzahl von Volumes pro Containergruppe | 20 |
| Ports pro IP-Adresse | 5 |
| Containererstellungen pro Stunde |300<sup>1</sup> |
| Containererstellungen pro 5 Minuten | 100<sup>1</sup> |
| Containerlöschungen pro Stunde | 300<sup>1</sup> |
| Containerlöschungen pro 5 Minuten | 100<sup>1</sup> |
| Mehrere Container pro Containergruppe | Nur Linux<sup>2</sup> |
| Azure Files-Volumes | Nur Linux<sup>2</sup> |
| GitRepo-Volumes | Nur Linux<sup>2</sup> |
| Geheime Volumes | Nur Linux<sup>2</sup> |

<sup>1</sup> Erstellen Sie eine [Azure Supportanfrage][azure-support], um eine Erhöhung des Grenzwerts anzufordern.<br />
<sup>2</sup> Windows-Unterstützung für dieses Feature ist geplant.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
