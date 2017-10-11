Einige Pakete können nicht mithilfe von Pip, die bei der Ausführung unter Azure installiert.  Es kann einfach sein, dass das Paket nicht auf die Python-Paketverzeichnis verfügbar ist.  Es ist möglicherweise, dass ein Compiler erforderlich ist (ein Compiler ist nicht verfügbar, auf dem Computer, auf die Web-app in Azure App Service ausgeführt).

In diesem Abschnitt sehen wir uns Methoden zum Umgang mit diesem Problem wäre.

### <a name="request-wheels"></a>Anforderung Räder
Wenn die Installation des Pakets einen Compiler erfordert, sollten Sie versuchen, wenden Sie sich an den Besitzer des Pakets, um anzufordern, Räder für das Paket zur Verfügung gestellt werden.

Mit der aktuellen Verfügbarkeit des [Microsoft Visual C++-Compiler für Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], es ist jetzt einfacher zum Erstellen von Paketen, die über systemeigenen Code für Python 2.7 verfügen.

### <a name="build-wheels-requires-windows"></a>Erstellen von Räder (erfordert Windows)
Hinweis: Wenn Sie diese Option verwenden zu können, stellen Sie sicher, kompilieren Sie das Paket mit einer Python-Umgebung, die Version die Plattform/Architektur/entspricht, die auf die Web-app in Azure App Service verwendet wird (Windows/32-bit/2.7 oder 3.4).

Wenn das Paket installieren nicht, da sie einen Compiler erfordert, können Sie den Compiler auf dem lokalen Computer installieren und erstellen ein Rad für das Paket, das Sie dann in Ihrem Repository enthalten.

Mac/Linux-Benutzer: Wenn Sie den Zugriff auf einen Windows-Computer besitzen, finden Sie unter [erstellen Sie einen virtuellen Computers unter Windows] [ Create a Virtual Machine Running Windows] zum Erstellen eines virtuellen Computers in Azure.  Sie können es verwenden, erstellen die Räder, fügen sie in das Repository und verwerfen den virtuellen Computer bei Bedarf. 

Sie können für Python 2.7, installieren [Microsoft Visual C++-Compiler für Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].

Für die Python-3.4, installieren Sie [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].

Um Räder zu erstellen, benötigen Sie das Mausrad-Paket:

    env\scripts\pip install wheel

Verwenden Sie `pip wheel` um eine Abhängigkeit zu kompilieren:

    env\scripts\pip wheel azure==0.8.4

Dadurch wird eine .whl-Datei im Ordner "\wheelhouse" erstellt.  Die \wheelhouse-Ordner und dem Mausrad-Dateien an Ihr Repository hinzuzufügen.

Bearbeiten Sie Ihre requirements.txt hinzufügen der `--find-links` Option oben. Dies teilt Pip, um nach einer genauen Übereinstimmung im lokalen Ordner zu suchen, bevor die Python-Paketverzeichnis möchten.

    --find-links wheelhouse
    azure==0.8.4

Wenn Sie alle Ihre Abhängigkeiten beinhalten, im Ordner "\wheelhouse" und nicht die Python-Paketverzeichnis überhaupt verwenden möchten, können Sie erzwingen, dass Pip, um das Paket durch Hinzufügen von ignorieren `--no-index` an den Anfang der requirements.txt.

    --no-index

### <a name="customize-installation"></a>Anpassen der installation
Sie können anpassen, dass das Bereitstellungsskript zum Installieren eines Pakets in der virtuellen Umgebung mit einem alternativen Installationsprogramm wie leicht\_installieren.  Finden Sie unter deploy.cmd für ein Beispiel, das auskommentiert ist.  Stellen Sie sicher, dass solche Pakete in requirements.txt, um zu verhindern, dass Pip installiert werden nicht aufgelistet werden.

Fügen Sie an das Bereitstellungsskript Folgendes ein:

    env\scripts\easy_install somepackage

Sie können auch in der Lage sind, verwenden Sie einfach\_installieren, um über eine Exe-Installer (einige sind kompatibel, daher leicht zip\_Installation unterstützt werden).  Ihr Repository Installationsprogramm hinzu, und rufen Sie einfach\_installieren, indem Sie den Pfad zur ausführbaren Datei übergeben.

Fügen Sie an das Bereitstellungsskript Folgendes ein:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Schließen Sie der virtuellen Umgebung in das Repository (erfordert Windows)
Hinweis: Wenn Sie diese Option verwenden zu können, stellen Sie sicher auf eine virtuelle Umgebung zu verwenden, die Version die Plattform/Architektur/entspricht, die auf die Web-app in Azure App Service verwendet wird (Windows/32-bit/2.7 oder 3.4).

Wenn Sie die virtuelle Umgebung im Repository einschließen, können Sie verhindern, dass das Bereitstellungsskript auf diese Weise virtuelle Umgebung Management in Azure durch eine leere Datei erstellen:

    .skipPythonDeployment

Es wird empfohlen, dass Sie die vorhandene virtuelle Umgebung in der app löschen, um zu verhindern, dass verbleibende Dateien bei die virtuelle Umgebung wurde automatisch verwaltet werden.

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
