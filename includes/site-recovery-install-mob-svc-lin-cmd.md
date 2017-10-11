1. Kopieren Sie den Installer auf einem lokalen Ordner (z. B./tmp), auf dem Server, den Sie schützen möchten. Führen Sie in einem abschließenden die folgenden Befehle ein:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Führen Sie zum Installieren des Mobility Service, den folgenden Befehl ein:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Nach Abschluss der Installation muss der Mobility Service auf dem Konfigurationsserver registriert werden. Führen Sie den folgenden Befehl zum Mobility Service beim Konfigurationsserver registrieren.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobility Service-Installers Befehlszeilen

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-r |Mandatory|Gibt an, ob der Mobility Service (MS) installiert werden soll, oder MasterTarget(MT) installiert werden soll|MS </br> MT|
|-d: |Optional|Speicherort, in dem Mobility Service installiert wird|/usr/local/ASR|
|-v|Mandatory|Gibt die Plattform, auf der der Mobility Service installiert erste ist </br> </br>- **VMware** : Verwenden Sie diesen Wert bei der Installation von Mobility Service auf einem virtuellen Computer unter *VMware vSphere ESXi-Hosts*, *Hyper-V-Hosts* und *Phsyical-Servern* </br> - **Azure** : Verwenden Sie diesen Wert aus, wenn Sie eine Azure-IaaS-VM Agent installiert werden| VMware </br> Azure|
|-q|Optional|Gibt an, dass das Installationsprogramm im unbeaufsichtigten Modus ausführen| N/V|


#### <a name="mobility-service-configuration-command-line"></a>Mobility Service-Konfiguration verwenden

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-i |Mandatory|IP-Adresse des Konfigurationsserver|Eine beliebige gültige IP-Adresse|
|-P |Mandatory|Vollständigen Pfad der Datei, in dem die Verbindung Passphrase gespeichert ist|Alle gültigen Ordner|
