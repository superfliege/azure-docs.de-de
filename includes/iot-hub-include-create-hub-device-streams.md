---
title: Includedatei (Vorschau für Gerätestreams)
description: Includedatei (Vorschau für Gerätestreams)
author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: rezas
ms.custom: include file
ms.openlocfilehash: d1cfe3d998c08aef2b845315a16d881dea1cd1f6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124353"
---
In diesem Abschnitt wird beschrieben, wie Sie über das [Azure-Portal](https://portal.azure.com) einen IoT-Hub erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie +**Ressource erstellen**, und wählen Sie dann **Internet der Dinge**.

3. Klicken Sie in der Liste auf der rechten Seite auf **Iot Hub**. Daraufhin wird Ihnen der erste Bildschirm zum Erstellen eines IoT-Hubs angezeigt.

   ![Screenshot: Erstellen eines Hubs im Azure-Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-1.png)

   Füllen Sie die Felder aus:

   **Abonnement**: Wählen Sie das Abonnement aus, das Sie für Ihren IoT-Hub verwenden möchten.

   **Ressourcengruppe**: Sie können eine neue Ressourcengruppe erstellen oder eine bereits vorhandene Ressourcengruppe verwenden. Um eine neue Ressourcengruppe zu erstellen, klicken Sie auf **Neu erstellen**, und geben Sie den Namen ein, den Sie verwenden möchten. Um eine vorhandene Ressourcengruppe zu verwenden, klicken Sie auf **Vorhandene verwenden**, und wählen Sie die Ressourcengruppe in der Dropdownliste aus. Weitere Informationen finden Sie unter [Verwalten von Azure Resource Manager-Ressourcengruppen](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   **Region**: Die Region, in der sich Ihr Hub befinden soll. Wählen Sie eine Region aus, die die IoT Hub-Gerätestreamsvorschau unterstützt – entweder „USA, Mitte“ oder „USA, Mitte (EUAP)“.

   **IoT Hub-Name**: Geben Sie den Namen für Ihren IoT-Hub ein. Dieser Name muss global eindeutig sein. Wenn der eingegebene Name verfügbar ist, wird ein grünes Häkchen angezeigt.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Klicken Sie auf **Weiter: Größe und Skalierung**, um die Erstellung Ihres IoT-Hubs fortzusetzen.

   ![Screenshot mit der Einstellung für Größe und Skalierung für einen neuen IoT-Hub über das Azure-Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-2-free.png)

   Auf diesem Bildschirm können Sie die Standardeinstellungen übernehmen und im unteren Bereich einfach auf **Überprüfen + erstellen** klicken.

   **Tarif und Skalierung**: Wählen Sie einen der Standard-Tarife (S1, S2, S3) oder den Free-Tarif (F1) aus. Berücksichtigen Sie bei dieser Entscheidung ggf. auch die Anzahl Ihrer Geräte sowie die zu erwartenden streamingfremden Workloads für Ihren Hub (etwa Telemetrienachrichten). Der kostenlose Tarif ist beispielsweise für Tests und Evaluierungen vorgesehen. Damit kann für 500 Geräte eine Verbindung mit dem IoT Hub hergestellt werden, und bis zu 8.000 Nachrichten pro Tag sind möglich. Jedes Azure-Abonnement kann einen IoT Hub im kostenlosen Tarif erstellen. 

   **IoT Hub-Einheiten**: Diese Option hängt von der streamingfremden Workload ab, die Sie für Ihren Hub erwarten. Wählen Sie vorerst „1“ aus.

   Einzelheiten zu den anderen Tarifoptionen finden Sie unter [Skalieren einer IoT Hub-Lösung](../articles/iot-hub/iot-hub-scaling.md).

5. Klicken Sie auf **Überprüfen + erstellen**, um Ihre Auswahl zu überprüfen. Die Anzeige entspricht in etwa dem folgenden Bildschirm.

   ![Screenshot zur Überprüfung von Informationen bei der Erstellung des neuen IoT-Hubs](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-3-free.png)

6. Klicken Sie auf **Erstellen**, um Ihren neuen IoT-Hub zu erstellen. Das Erstellen des Hubs dauert einige Minuten.
