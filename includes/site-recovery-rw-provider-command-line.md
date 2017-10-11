UnifiedSetup.exe [/ ServerMode < CS/PS >] [/ Installationslaufwerk <DriveLetter>] [/ MySQLCredsFilePath <MySQL credentials file path>] [/ VaultCredsFilePath <Vault credentials file path>] [/ EnvType < VMWare/NonVMWare >] [/ PSIP < IP-Adresse für die Datenübertragung verwendet werden soll] [/ %CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>]

Parameter:

* / ServerMode: obligatorisch. Gibt an, ob die Konfiguration und die Prozess-Servern installiert werden soll, oder der Process-Server. Eingabewerte: CS PS.
* InstallLocation: obligatorisch. Der Ordner, in dem die Komponenten installiert sind.
* / MySQLCredsFilePath. Obligatorisch ist. Der Dateipfad, in dem die MySQL Server-Anmeldeinformationen gespeichert sind. Die Datei muss im folgenden Format:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* / VaultCredsFilePath. Obligatorisch ist. Der Speicherort der Datei mit den Tresor-Anmeldeinformationen
* / EnvType. Obligatorisch ist. Der Typ der Installation. Werte: VMware, NonVMware
* / PSIP und /CSIP. Obligatorisch ist. Die IP-Adresse dem Prozessserver und Konfigurationsserver.
* / PassphraseFilePath. Obligatorisch ist. Der Speicherort der passphrasedatei.
* / BypassProxy. (Optional) Gibt an, dass es sich bei der Konfiguration in Azure ohne einen Proxy eine Verbindung herstellt.
* / ProxySettingsFilePath. (Optional) Proxy-Einstellungen (der standardmäßige Proxy erfordert Authentifizierung oder einen benutzerdefinierten Proxy). Die Datei muss im folgenden Format:
* [ProxySettings]
* $C = "Yes/No"
* Proxy-IP-= "IP-Adresse >"
* ProxyPort = "<Port>"
* ProxyUserName = "<User Name>"
* ProxyPassword = "<Password>"
* DataTransferSecurePort. (Optional) Die Nummer des Ports für die von Replikationsdaten.
* SkipSpaceCheck. (Optional) Speicherplatz für den Cache Überprüfung zu überspringen.
* AcceptThirdpartyEULA. Obligatorisch ist. Akzeptiert die LIZENZBEDINGUNGEN von Drittanbietern.
* ShowThirdpartyEULA. Obligatorisch ist. Zeigt die Drittanbieter-Endbenutzer-Lizenzvertrag. Wenn als Eingabe bereitgestellt wird, werden alle anderen Parameter ignoriert.
