---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 1ae6c6ee59b66e6c61714c0ece9f306f1a885096
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553504"
---
Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des Azure IoT Hub Device Provisioning-Diensts gelten.

| Ressource | Begrenzung |
| --- | --- |
| Maximale Anzahl von Gerätebereitstellungsdiensten pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 500.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 500.000 |
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
