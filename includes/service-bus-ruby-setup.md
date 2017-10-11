## <a name="create-a-ruby-application"></a>Erstellen einer Ruby-Anwendung
Anweisungen hierzu finden Sie unter [Erstellen einer Ruby-Anwendung in Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendung zur Verwendung von Service Bus
Um Service Bus verwenden, herunterladen Sie und Ruby-Azure-Pakets, das Satz Komfort Bibliotheken enthält, die mit der Speicher-REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden Sie RubyGems, um das Paket erhalten
1. Verwenden Sie z. B. eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminaldienste** (Mac), oder **Bash** (Unix).
2. Geben Sie "Kugel installieren Azure" im Befehlsfenster die Kugel und Abhängigkeiten zu installieren.

### <a name="import-the-package"></a>Importieren Sie das Paket
Mit Ihrem bevorzugten Text-Editor, fügen Sie Folgendes am Anfang der Ruby-Datei in der Sie Speicher verwenden möchten:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Einrichten einer Service Bus-Verbindung
Verwenden Sie den folgenden Code, um festzulegen, die Werte der Namespace, Name des Schlüssels, Schlüssel, Signaturgeber und Host:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Legen Sie den Namespacewert, der Wert, den Sie also nicht die gesamte URL zu erstellen. Verwenden Sie z. B. **"Yourexamplenamespace"**, nicht "yourexamplenamespace.servicebus.windows.net".
