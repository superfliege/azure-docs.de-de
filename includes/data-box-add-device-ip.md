---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550285"
---
1. Melden Sie sich am Data Box-Gerät an. Stellen Sie sicher, dass es nicht gesperrt ist.

    ![Data Box-Dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Navigieren Sie zu **Netzwerkschnittstellen festlegen**. Notieren Sie sich die IP-Adresse des Geräts für die Netzwerkschnittstelle, die zum Herstellen der Verbindung mit dem Client verwendet wird.

    ![Data Box-Dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Navigieren Sie zu **Verbindung herstellen und Daten kopieren**, und klicken Sie auf **REST (Vorschau)**.

    ![Data Box-Dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Kopieren Sie im Dialogfeld **Auf Speicherkonto zugreifen und Daten hochladen** den **Blob-Dienstendpunkt**.

    ![Data Box-Dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Starten Sie **Editor** als Administrator, und öffnen Sie die Datei **hosts** unter `C:\Windows\System32\Drivers\etc`.
6. Fügen Sie Ihrer **hosts**-Datei den folgenden Eintrag hinzu: `<device IP address> <Blob service endpoint>`.
7. Verwenden Sie die folgende Abbildung als Referenz. Speichern Sie die Datei **hosts**.

    ![Data Box-Dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
