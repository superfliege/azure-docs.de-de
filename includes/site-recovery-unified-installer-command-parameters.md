|Parametername| Typ | Beschreibung| Mögliche Werte|
|-|-|-|-|
| / ServerMode|Mandatory|Gibt an, ob die Konfiguration und die Prozess-Servern installiert werden soll, oder der Process-server|CS<br>PS|
|/ InstallLocation|Mandatory|Der Ordner, in dem die Komponenten installiert sind| Einen beliebigen Ordner auf dem computer|
|/ MySQLCredsFilePath|Mandatory|Der Dateipfad, in dem die MySQL Server-Anmeldeinformationen gespeichert werden|Die Datei sollte das unten angegebene Format sein.|
|/ VaultCredsFilePath|Mandatory|Der Pfad der Datei mit den Tresor-Anmeldeinformationen|Gültiger Dateipfad|
|/ EnvType|Mandatory|Typ des fern, die Sie schützen möchten |VMware<br>NonVMware|
|/ PSIP|Mandatory|IP-Adresse der Netzwerkkarte für die Übertragung von Daten verwendet werden soll| Eine beliebige gültige IP-Adresse|
|/ %CSIP|Mandatory|Die IP-Adresse der Netzwerkkarte auf dem Konfigurationsserver abgehört wird| Eine beliebige gültige IP-Adresse|
|/ PassphraseFilePath|Mandatory|Den vollständigen Pfad zum Speicherort der passphrasedatei|Gültiger Dateipfad|
|/ BypassProxy|Optional|Gibt an, dass es sich bei der Konfiguration in Azure ohne einen Proxy eine Verbindung herstellt|Um diesen Wert auf Venu abrufen|
|/ ProxySettingsFilePath|Optional|Proxy-Einstellungen (der standardmäßige Proxy erfordert Authentifizierung oder einen benutzerdefinierten Proxy)|Die Datei sollte in der unten angegebenen Format sein.|
|DataTransferSecurePort|Optional|Portnummer für die PSIP für Replikationsdaten verwendet werden soll| Gültige Portnummer (der Standardwert ist 9433)|
|/ SkipSpaceCheck|Optional|Skip überprüfen für Cachedatenträger| |
|/ AcceptThirdpartyEULA|Mandatory|Flag impliziert Annahme der LIZENZBEDINGUNGEN von Drittanbietern| |
|/ ShowThirdpartyEULA|Optional|Zeigt die Drittanbieter-Endbenutzer-Lizenzvertrag. Wenn Sie als Eingabe bereitgestellt werden alle anderen Parameter ignoriert.| |
