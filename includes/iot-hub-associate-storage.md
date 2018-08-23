---
title: Includedatei
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/20/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 892dd050829dce242035e2b875ea43ed13910d4a
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246073"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Zuweisen eines Azure Storage-Kontos zu IoT Hub

Da von der simulierten Geräte-App eine Datei in ein Blob hochgeladen wird, müssen Sie über ein [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md)-Konto verfügen, das IoT Hub zugeordnet ist. Wenn Sie Azure Storage-Konto einem IoT Hub zuordnen, generiert der IoT Hub einen SAS-URI. Ein Gerät kann diesen SAS-URI für das sichere Hochladen einer Datei zu einem Blobcontainer verwenden. Der IoT Hub-Dienst und die Geräte-SDKs koordinieren den Prozess, der die SAS-URI generiert und für ein Gerät verfügbar macht, sodass er beim Hochladen einer Datei verwendet werden kann.

Befolgen Sie die Anweisungen unter [Konfigurieren von Dateiuploads über das Azure-Portal](../articles/iot-hub/iot-hub-configure-file-upload.md), um Ihrem IoT Hub ein Azure Storage-Konto zuzuordnen. Stellen Sie sicher, dass Ihrem IoT Hub ein Blobcontainer zugeordnet ist und die Dateibenachrichtigungen aktiviert sind.

![Aktivieren von Dateibenachrichtigungen im Portal](media/iot-hub-associate-storage/enable-file-notifications.png)