---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 65c89730e7d3d492b91daa8aba50e5606ca700a1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804116"
---
Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des Azure IoT Hub Device Provisioning-Diensts gelten.

| Ressource | Begrenzung |
| --- | --- |
| Maximale Anzahl von Gerätebereitstellungsdiensten pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 1.000.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 1.000.000 |
| Maximale Anzahl von Registrierungsgruppen | 100 |
| Maximale Anzahl von Zertifizierungsstellen | 25 |

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl von Instanzen in Ihrem Abonnement erhöhen zu lassen.

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl der Registrierungen für Ihren Bereitstellungsdienst zu erhöhen.

Der Device Provisioning Service drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden.

| Drosselung | Wert pro Einheit |
| --- | --- |
| Vorgänge | 200/Minute/Dienst |
| Geräteregistrierungen | 200/Minute/Dienst |
| Abrufvorgang für Geräte | 5/10 Sekunden/Gerät |
