Azure wird die Version von Python mit für ihre virtuelle Umgebung mit der folgenden Priorität bestimmen:

1. in runtime.txt im Stammordner angegebene Version
2. Version von Python-Einstellung in der Web-app-Konfiguration angegeben (die **Einstellungen** > **Anwendungseinstellungen** Blatt für Ihre Web-app im Azure-Portal)
3. Python 2.7 ist die Standardeinstellung, wenn keine der oben genannten Kriterien angegeben werden

Gültige Werte für den Inhalt der 

    \runtime.txt

sind:

* Python 2.7
* Python-3.4

Wenn die micro-Version (dritte Ziffer) angegeben wird, wird ignoriert.

