1. Kopieren Sie den Installer auf einem lokalen Ordner (z. B. C:\Temp) auf dem Server, den Sie schützen möchten. Führen Sie die folgenden Befehle als Administrator an der Eingabeaufforderung ein:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Führen Sie zum Installieren des Mobility Service, den folgenden Befehl ein:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Nun muss der Agent und dem Konfigurationsserver registriert werden.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Mobility Service-Installer-Befehlszeilenargumente

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|/ Rolle ""|Mandatory|Gibt an, ob der Mobility Service (MS) installiert werden soll, oder MasterTarget(MT) installiert werden soll|MS </br> MT|
|/ InstallLocation|Optional|Speicherort, auf dem Mobility Service installiert ist|Einen beliebigen Ordner auf dem computer|
|/ Platform|Mandatory|Gibt die Plattform, auf der der Mobility Service installiert erste ist </br> </br>- **VMware** : Verwenden Sie diesen Wert bei der Installation von Mobility Service auf einem virtuellen Computer unter *VMware vSphere ESXi-Hosts*, *Hyper-V-Hosts* und *Phsyical-Servern* </br> - **Azure** : Verwenden Sie diesen Wert aus, wenn Sie eine Azure-IaaS-VM Agent installiert werden| VMware </br> Azure|
|/Silent|Optional|Gibt an, dass das Installationsprogramm im unbeaufsichtigten Modus ausführen| N/V|

>[!TIP]
> Die Setup-Protokollen finden Sie unter %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Mobility Service-Registrierung-Befehlszeilenargumente

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parameter|Typ|Beschreibung|Mögliche Werte|
  |-|-|-|-|
  |/ CSEndPoint |Mandatory|IP-Adresse des konfigurationsservers| Eine beliebige gültige IP-Adresse|
  |/ PassphraseFilePath|Mandatory|Speicherort der passphrase |Alle gültigen UNC-Namen oder lokalen Pfad|


>[!TIP]
> Die Protokolle AgentConfiguration finden Sie unter %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log
