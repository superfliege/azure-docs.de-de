<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Vorbereiten für updates
Sie müssen die folgenden Schritte ausführen, bevor Sie überprüfen, und wenden Sie das Update:

1. Erstellen Sie eine cloudmomentaufnahme des ausgewählten Geräts.
2. Stellen Sie sicher, dass die festen IP-Adressen des Controllers routingfähig sind und mit dem Internet herstellen können. Diese festen IP-Adressen werden verwendet werden, um Updates für Ihr Gerät. Sie können diese testen, indem Sie das folgende Cmdlet auf jedem Controller ausführen, von der Windows PowerShell-Schnittstelle des Geräts:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Beispielausgabe für Test-Connection, wenn die feste IP-Adressen eine Verbindung mit dem Internet herstellen können**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Nachdem Sie diese manuellen Vorabüberprüfungen erfolgreich abgeschlossen haben, können Sie fortfahren, zu überprüfen und die Updates zu installieren.

