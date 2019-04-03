---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 216380cf7069468d13c4e533fc90b2596aa211c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125139"
---
Ein ordnungsgemäßes SSL-Zertifikat stellt sicher, dass Sie verschlüsselte Informationen an den richtigen Server senden. Neben der Verschlüsselung ermöglicht das Zertifikat auch die Authentifizierung. Sie können über die PowerShell-Schnittstelle des Geräts Ihr eigenes vertrauenswürdiges SSL-Zertifikat hochladen.

1. [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](#connect-to-the-powershell-interface).
2. Verwenden Sie das Cmdlet `Set-HcsCertificate`, um das Zertifikat hochzuladen. Geben Sie bei entsprechender Aufforderung die folgenden Parameter an:

   - `CertificateFilePath`: Pfad zur Freigabe, der die Zertifikatdatei im *PFX*-Format enthält.
   - `CertificatePassword`: Kennwort zum Schutz des Zertifikats.
   - `Credentials`: Benutzername und Kennwort für den Zugriff auf die Freigabe, die das Zertifikat enthält.

     Nachfolgend sehen Sie ein Beispiel für die Verwendung dieses Cmdlets:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credentials "Username/Password"
     ```

