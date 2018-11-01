---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 65477f62af80511a73307204c2a6f4b5e0f409d6
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164249"
---
1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „C:\Temp“) auf dem Server, den Sie schützen möchten. Führen Sie die folgenden Befehle an einer Eingabeaufforderung als Administrator aus:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Führen Sie den folgenden Befehl aus, um den Mobilitätsdienst zu installieren:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Nun muss der Agent beim Konfigurationsserver registriert werden.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parameter|Typ|BESCHREIBUNG|Mögliche Werte|
|-|-|-|-|
|/Role|Erforderlich|Gibt an, ob Mobility Service (MS) oder MasterTarget (MT) installiert werden soll.|MS </br> MT|
|/InstallLocation|Optional|Installationsort des Mobilitätsdiensts|Beliebiger Ordner auf dem Computer|
|/Platform|Erforderlich|Gibt die Plattform an, auf der Mobility Service installiert wird. </br> </br>- **VMware**: Verwenden Sie diesen Wert für die Installation von Mobility Service auf einem virtuellen Computer, der auf *VMware vSphere ESXi-Hosts*, *Hyper-V-Hosts* und *physischen Servern* ausgeführt wird. </br> - **Azure**: Verwenden Sie diesen Wert, wenn Sie einen Agent auf einem virtuellen Azure-IaaS-Computer installieren. | VMware </br> Azure|
|/Silent|Optional|Gibt an, dass das Installationsprogramm im unbeaufsichtigten Modus ausgeführt werden soll.| N/V|

>[!TIP]
> Die Setupprotokolle finden Sie unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log“.

#### <a name="mobility-service-registration-command-line-arguments"></a>Befehlszeilenargumente für die Mobility Service-Registrierung

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parameter|Typ|BESCHREIBUNG|Mögliche Werte|
  |-|-|-|-|
  |/CSEndPoint |Erforderlich|IP-Adresse des Konfigurationsservers| Beliebige gültige IP-Adresse|
  |/PassphraseFilePath|Erforderlich|Speicherort der Passphrase |Beliebiger UNC- oder lokaler Dateipfad|


>[!TIP]
> Die Agent-Konfigurationsprotokolle finden Sie unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log“.
