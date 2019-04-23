---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 23587e617c62cfe4aa24256330c5f7673dd1c674
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684420"
---
Ihr Gerät ist einem Speicherkonto zugeordnet, das als Ziel für Ihre Daten in Azure verwendet wird. Der Zugriff auf das Speicherkonto wird über das Abonnement und zwei Zugriffsschlüssel mit 512 Bit gesteuert, die dem Speicherkonto zugeordnet sind.

Einer der Schlüssel dient zur Authentifizierung, wenn das Data Box Edge-Gerät auf das Speicherkonto zugreift. Der andere Schlüssel dient als Reserve und gestattet Ihnen, die Schlüssel regelmäßig zu rotieren.

Aus Sicherheitsgründen ist in vielen Datencentern eine Schlüsselrotation erforderlich. Es wird empfohlen, diese bewährten Methoden für die Schlüsselrotation zu befolgen:

- Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, dass Ihr Kontoschlüssel immer gut geschützt ist. Geben Sie das Kennwort nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie es nicht als Klartext an einem Ort, auf den andere Benutzer Zugriff haben.
- [Generieren Sie Ihren Kontoschlüssel über das Azure-Portal neu](../articles/storage/common/storage-account-manage.md#regenerate-access-keys), wenn Sie der Meinung sind, dass er nicht mehr sicher ist.
- Der Azure-Administrator sollte den primären oder sekundären Schlüssel regelmäßig über den Abschnitt „Storage“ im Azure-Portal neu generieren, um direkt auf das Speicherkonto zuzugreifen.