---
title: Authentifizierungsmethoden für Azure Security Center for IoT (Vorschauversion) | Microsoft-Dokumentation
description: Lernen Sie die verschiedenen Authentifizierungsmethoden kennen, die für den Azure Security Center for IoT-Dienst verfügbar sind.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 23bc4d0df1c8124ec225ac31239c7acb3f1ab546
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541494"
---
# <a name="security-agent-authentication-methods"></a>Authentifizierungsmethoden des Sicherheits-Agents 

> [!IMPORTANT]
> Azure Security Center for IoT befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview).
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel werden die verschiedenen Authentifizierungsmethoden erläutert, die Sie mit dem AzureIoTSecurity-Agent für die Authentifizierung beim IoT Hub verwenden können.

Für jedes Gerät, das im IoT Hub in Azure Security Center for IoT integriert ist, ist ein Sicherheitsmodul erforderlich. Für die Authentifizierung des Geräts kann Azure Security Center for IoT eine der beiden Methoden verwenden. Wählen Sie die Methode aus, die sich am besten für Ihre vorhandene IoT-Lösung eignet. 

> [!div class="checklist"]
> * Sicherheitsmoduloption
> * Geräteoption

## <a name="authentication-methods"></a>Authentifizierungsmethoden

Für den AzureIoTSecurity-Agent gibt es die beiden folgenden Authentifizierungsmethoden:

 - Authentifizierungsmodus **Modul**<br>
   Das Modul wird unabhängig von dem Gerätezwilling authentifiziert.
   Die Informationen, die für diese Art der Authentifizierung erforderlich sind, werden durch die Datei „Authentication.config“ für C# bzw. die Datei „LocalConfiguration.json“ für C definiert.
        
 - Authentifizierungsmodus **Gerät**<br>
    Bei dieser Methode wird der Sicherheits-Agent zunächst für das Gerät authentifiziert. Nach der ersten Authentifizierung führt der Azure Security Center for IoT-Agent den Aufruf **Rest** am IoT Hub durch und verwendet dabei die Rest-API mit den Authentifizierungsdaten des Geräts. Der Azure Security Center for IoT-Agent fordert dann die Authentifizierungsmethode und -daten des Sicherheitsmoduls vom IoT Hub an. Im letzten Schritt nimmt der Azure Security Center for IoT-Agent eine Authentifizierung beim Azure Security Center for IoT-Modul vor.    

Informationen zur Konfiguration finden Sie unter [Security agent installation parameters (Installationsparameter für den Sicherheits-Agent)](#security-agent-installation-parameters).
                                
## <a name="authentication-methods-known-limitations"></a>Bekannte Einschränkungen von Authentifizierungsmethoden

- Der Authentifizierungsmodus **Modul** unterstützt nur die Authentifizierung mit symmetrischen Schlüsseln.
- Der Authentifizierungsmodus **Gerät** unterstützt kein von der Zertifizierungsstelle signiertes Zertifikat.  

## <a name="security-agent-installation-parameters"></a>Installationsparameter für den Sicherheits-Agent

Beim [Bereitstellen eines Sicherheits-Agents](select-deploy-agent.md) müssen Authentifizierungsdetails als Argumente angegeben werden.
Diese Argumente werden in der folgenden Tabelle gezeigt.


|Parameter|BESCHREIBUNG|Optionen|
|---------|---------------|---------------|
|**Identität**|Authentifizierungsmodus| **Modul** oder **Gerät**|
|**type**|Authentifizierungsart|**SymmetricKey** oder **SelfSignedCertificate**|
|**filePath**|Vollständiger Pfad der Datei, die das Zertifikat oder den symmetrischen Schlüssel enthält| |
|**gatewayHostname**|Vollqualifizierter Domänenname (FQDN) des IoT Hubs|Beispiel: ContosoIotHub.azure-devices.net|
|**deviceId**|Geräte-ID|Beispiel: MyDevice1|
|**certificateLocationKind**|Speicherort des Zertifikats|**LocalFile** oder **Store**|


Wenn Sie das Installationsskript des Sicherheits-Agents verwenden, wird die folgende Konfiguration automatisch ausgeführt.

Um die Authentifizierung des Sicherheits-Agents manuell zu bearbeiten, bearbeiten Sie die Konfigurationsdatei. 

## <a name="change-authentication-method-after-deployment"></a>Ändern der Authentifizierungsmethode nach der Bereitstellung

Wenn Sie einen Sicherheits-Agent mit einem Installationsskript bereitstellen, wird automatisch eine Konfigurationsdatei erstellt.

Um Authentifizierungsmethoden nach der Bereitstellung zu ändern, muss die Konfigurationsdatei manuell bearbeitet werden.


### <a name="c-based-security-agent"></a>C#-basierter Sicherheits-Agent

Bearbeiten Sie _Authentication.config_ mit den folgenden Parametern:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C-basierter Sicherheits-Agent

Bearbeiten Sie _LocalConfiguration.json_ mit den folgenden Parametern:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Weitere Informationen
- [Security agents overview (Sicherheits-Agents (Übersicht))](security-agent-architecture.md)
- [Deploy security agent (Bereitstellen eines Sicherheits-Agents)](select-deploy-agent.md)
- [Access raw security data (Zugreifen auf Sicherheitsrohdaten)](how-to-security-data-access.md)
