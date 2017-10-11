Azure bestimmt, dass Ihre Anwendung Python verwendet **Wenn beide Bedingungen erfüllt sind**:

* Requirements.txt-Datei im Stammordner
* Jede py-Datei im Stammordner oder eine runtime.txt, der angibt, Python

Wenn dies der Fall ist, wird ein Python Bereitstellungsskript verwendet die standardmäßige Synchronisierung von Dateien sowie zusätzliche Python-Vorgänge wie z. B. ausführt:

* Zur automatischen Verwaltung der virtuellen Umgebung
* Installation von Paketen, die in der Verwendung von Pip requirements.txt aufgeführt
* Erstellen der entsprechenden Web.config-Datei anhand der ausgewählten Python-Version.
* Statische Dateien für Django Anwendungen sammeln

Sie können bestimmte Aspekte des Standard-Bereitstellungsschritte steuern, ohne dass das Skript anpassen.

Wenn Sie alle Python bestimmte Bereitstellungsschritte überspringen möchten, können Sie diese leere Datei erstellen:

    \.skipPythonDeployment

Mehr Kontrolle über die Bereitstellung können Sie das Standard-Bereitstellungsskript überschreiben, indem Sie die folgenden Dateien erstellen:

    \.deployment
    \deploy.cmd

Sie können die [Azure-Befehlszeilenschnittstelle] [ Azure command-line interface] zur Erstellung der Dateien.  Verwenden Sie diesen Befehl aus dem Projektordner aus:

    azure site deploymentscript --python

Wenn diese Dateien nicht vorhanden sind, wird Azure eine temporäre Bereitstellungsskript erstellt und ausgeführt.  Es ist identisch mit dem Kennwort, die Sie mit den oben aufgeführten Befehl zu erstellen.

[Azure command-line interface]: http://azure.microsoft.com/downloads/
