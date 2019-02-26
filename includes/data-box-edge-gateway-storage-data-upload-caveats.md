---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 8bc4b78d262cf5f30076e90b40b92c4f3237924a
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56333888"
---
Die folgenden Einschränkungen gelten für Daten, die in Azure verschoben werden.

- Es wird empfohlen, dass nicht mehr als ein Gerät in den gleichen Container schreibt.
- Wenn Sie bereits über ein Azure-Objekt (z.B. Blob oder Datei) in der Cloud verfügen, das den gleichen Namen hat wie das Objekt, das kopiert wird, überschreibt das Gerät die Datei in der Cloud.
- Eine unter Freigabeordnern erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht in die Blobcontainer hochgeladen.
- Bei großen Dateien wird die Verwendung von Robocopy empfohlen, da der Kopiervorgang bei periodischen Fehlern wiederholt wird.
- Wenn die dem Azure-Speichercontainer zugeordnete Freigabe Blobs hochlädt, die nicht mit dem Typ der zum Zeitpunkt der Erstellung für die Freigabe definierten Blobs übereinstimmen, werden solche Blobs nicht aktualisiert. Sie erstellen z.B. eine Freigabe für Blockblobs auf dem Gerät. Ordnen Sie die Freigabe einem vorhandenen Cloudcontainer mit Seitenblobs zu. Aktualisieren Sie diese Freigabe zum Herunterladen der Dateien. Ändern Sie einige der aktualisierten Dateien, die in der Cloud bereits als Seitenblobs gespeichert sind. Es treten Uploadfehler auf.
