Jedes Blob im Azure-Speicher muss in einem Container befinden. Der Container bildet einen Teil der Blob-Name. Beispielsweise `mycontainer` ist der Name des Containers in diesen Beispiel-Blob-URIs:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Ein Containername muss ein gültiger DNS-Name, den folgenden Benennungsregeln entspricht:

1. Containernamen müssen mit einem Buchstaben oder einer Ziffer beginnen und darf nur Buchstaben, Zahlen und Bindestriche (-) enthalten.
2. Jedem Bindestrich (-) muss unmittelbar vorangestellt und gefolgt von einem Buchstaben oder einer Ziffer; aufeinander folgende Bindestriche sind in Containernamen nicht zulässig.
3. Alle Buchstaben in einem Containernamen müssen Kleinbuchstaben sein.
4. Containernamen müssen zwischen 3 und 63 Zeichen lang sein.

> [!IMPORTANT]
> Beachten Sie, dass der Name eines Containers immer kleingeschrieben werden muss. Wenn Sie einen Großbuchstaben in einem Containernamen enthalten oder den Benennungsregeln für Container verletzen, wird möglicherweise ein Fehler 400 (Ungültige Anforderung) angezeigt. 
> 
> 

