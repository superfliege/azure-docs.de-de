Der Speicheremulator unterstützt ein einziges festgelegtes Konto und einen bekannten Authentifizierungsschlüssel, für die Authentifizierung mit gemeinsam verwendetem Schlüssel. Dieses Konto und die Schlüssel sind die einzigen gemeinsam verwendeten Schlüssel-Anmeldeinformationen für die Verwendung mit dem Speicheremulator zulässig. Sie sind:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Der vom Speicheremulator unterstützte Authentifizierungsschlüssel ist nur für das Testen der Funktionalität des clientauthentifizierungscodes vorgesehen. Es ist keine keinerlei Sicherheitszwecke. Sie können nicht Ihr Speicherkonto für die Produktion und der Schlüssel mit dem Speicheremulator verwenden. Sie sollten das entwicklungskonto nicht mit Produktionsdaten verwenden.
> 
> Der Speicheremulator unterstützt nur Verbindung über HTTP. HTTPS ist jedoch das empfohlene Protokoll für den Zugriff auf Ressourcen in einer produktionsumgebung Azure-Speicherkonto an.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Herstellen einer Verbindung mit dem Emulator-Konto mithilfe einer Verknüpfung
Die einfachste Möglichkeit zur Verbindung mit des Speicheremulators aus Ihrer Anwendung ist eine Verbindungszeichenfolge in der Konfigurationsdatei Ihrer Anwendung zu konfigurieren, die die Verknüpfung verweist auf `UseDevelopmentStorage=true`. Hier ist ein Beispiel einer Verbindungszeichenfolge auf den Speicheremulator in einer *"App.config"* Datei: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Herstellen einer Verbindung mit dem Emulator-Konto, mit dem bekannten Namen und Schlüssel
Um eine Verbindungszeichenfolge zu erstellen, die auf dem Emulator-Kontonamen und den Schlüssel verweist, müssen Sie die Endpunkte für jeden der Dienste angeben, die Sie über dem Emulator in der Verbindungszeichenfolge verwenden möchten. Dies ist erforderlich, damit die Verbindungszeichenfolge die Endpunkte Emulator verweisen, die unterscheiden, die für ein Speicherkonto für die Produktion sind. Beispielsweise wird der Wert der Verbindungszeichenfolge wie folgt aussehen:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Dieser Wert ist identisch mit der oben gezeigten Verknüpfung `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Geben Sie einen HTTP-proxy
Sie können auch angeben, dass einen HTTP-Proxy verwendet wird, wenn Sie den Dienst mit dem Speicheremulator testen. Dies kann hilfreich für das HTTP-Anforderungen und Antworten beobachten, während des Debuggens Vorgänge für die Speicherdienste sein. Fügen Sie zum Angeben eines Proxys der `DevelopmentStorageProxyUri` zur Verbindungszeichenfolge hinzufügen aus, und legen Sie dessen Wert auf den Proxy-URI. Hier ist z. B. eine Verbindungszeichenfolge, die auf den Speicheremulator verweist und einen HTTP-Proxy:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

