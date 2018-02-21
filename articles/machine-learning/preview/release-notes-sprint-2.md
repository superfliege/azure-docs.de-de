---
title: Anmerkungen zu dieser Version von Azure ML Workbench (Sprint 2, Dezember 2017)
description: "In diesem Dokument werden die Aktualisierungen für die Version „Sprint 2“ von Azure ML detailliert beschrieben."
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 630e6e22bb41c777a043a7e6580239e254db9f1f
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="sprint-2---december-2017"></a>Sprint 2 – Dezember 2017 

#### <a name="version-number-01171115263"></a>Versionsnummer: 0.1.1711.15263

>So können Sie die [Versionsnummer ermitteln](known-issues-and-troubleshooting-guide.md).

Willkommen beim dritten Update von Azure Machine Learning Workbench. Dieses Update enthält Verbesserungen in der Workbench-App, der Befehlszeilenschnittstelle (CLI) und den Back-End-Diensten. Vielen Dank, dass Sie uns ein Lächeln oder Stirnrunzeln gesendet haben. Bei vielen der unten angegebenen Aktualisierungen wurde Ihr Feedback direkt umgesetzt. 

## <a name="notable-new-features"></a>Wichtige neue Features
- [Unterstützung für SQL Server und Azure SQL-Datenbank als Datenquelle](data-prep-appendix2-supported-data-sources.md#types) 
- [Deep Learning in Spark mit GPU-Unterstützung unter Verwendung von MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Alle AML-Container sind nach der Bereitstellung mit Azure IoT Edge-Geräten kompatibel (keine zusätzlichen Schritte erforderlich)](http://aka.ms/aml-iot-edge-blog)
- Liste der registrierten Modelle und Detailansichten im Azure-Portal verfügbar
- Zugriff auf Computeziele unter Verwendung der Authentifizierung mit SSH-Schlüsseln zusätzlich zum Zugriff über Benutzername/Kennwort 
- Neue Musterhäufigkeitsprüfung bei der Datenvorbereitung 

## <a name="detailed-updates"></a>Detaillierte Aktualisierungen
Die nachstehende Liste zeigt die detaillierten Aktualisierungen für jeden Komponentenbereich von Azure Machine Learning in diesem Sprint.

### <a name="installer"></a>Installer
- Der Installer kann eigenständig Updates ausführen, sodass Fehlerbehebungen und neue Features ohne eine erneute Installation unterstützt werden können.

### <a name="workbench-authentication"></a>Workbench-Authentifizierung
- Verschiedene Fehlerbehebungen für das Authentifizierungssystem. Teilen Sie uns mit, wenn weiterhin Anmeldeprobleme auftreten.
- Änderungen an der Benutzeroberfläche erleichtern das Auffinden der Proxy-Manager-Einstellungen.

### <a name="workbench"></a>Workbench
- Die schreibgeschützte Dateiansicht wird jetzt mit einem hellblauen Hintergrund angezeigt.
- Die Schaltfläche „Bearbeiten“ wurde nach rechts verschoben, damit sie leichter aufzufinden ist.
- Die Dateiformate „dsource“, „dprep“ und „ipynb“ können jetzt als unformatierter Text gerendert werden.
- Die Workbench verfügt nun über eine neue Bearbeitungsoberfläche, um den Benutzer an die Verwendung externer IDEs zum Bearbeiten von Skripts heranzuführen und die Workbench nur zum Bearbeiten von Dateitypen mit umfangreicher Bearbeitungsfunktionalität zu nutzen (z.B. Notebooks, Datenquellen, Datenvorbereitungspakete).
- Die Liste der Arbeitsbereiche und Projekte, auf die der Benutzer Zugriff hat, wird wesentlich schneller geladen.

### <a name="data-preparation"></a>Vorbereitung der Daten 
- Mithilfe einer Musterhäufigkeitsprüfung können die Zeichenfolgenmuster in einer Spalte angezeigt werden. Sie können Ihre Daten auch mithilfe dieser Muster filtern. Die angezeigte Ansicht ähnelt der Prüfung für die Anzahl von Werten. Der Unterschied liegt darin, dass die Musterhäufigkeit anstelle der Anzahl eindeutiger Daten die Anzahl eindeutiger Datenmuster zeigt. Sie können auch einen Filter verwenden, mit dem alle Zeilen, die einem bestimmten Muster entsprechen, entweder angezeigt werden oder nicht angezeigt werden.

![Abbildung zur Musterhäufigkeitsprüfung für die Produktnummer](media/release-notes-sprint-2/pattern-inspector-product-number.png)

- Leistungsverbesserungen bei gleichzeitiger Empfehlung von Grenzfällen zur Überprüfung in der Transformation „Derive Column by Example“ (Spalte nach Beispiel ableiten)

- [Unterstützung für SQL Server und Azure SQL-Datenbank als Datenquelle](data-prep-appendix2-supported-data-sources.md#types) 

![Abbildung zum Erstellen einer neuen SQL Server-Datenquelle](media/release-notes-sprint-2/sql-server-data-source.png)

- Die Ansicht „Auf einen Blick“ wurde für Zeilen- und Spaltenanzahl aktiviert.

![Abbildung zur Zeilen- und Spaltenanzahl auf einen Blick](media/release-notes-sprint-2/row-col-count.png)

- Die Datenvorbereitung ist jetzt in allen Computekontexten aktiviert.
- Datenquellen mit Verwendung einer SQL Server-Datenbank sind in allen Computekontexten aktiviert.
- Rasterspalten zur Datenvorbereitung können nach Datentyp gefiltert werden.
- Ein Problem beim Konvertieren mehrerer Spalten in Datumswerte wurde behoben.
- Es wurde ein Problem behoben, durch das der Benutzer in „Derive Column by Example“ die Ausgabespalte als Quelle auswählen konnte, wenn der Name der Ausgabespalte im erweiterten Modus durch den Benutzer geändert wurde.

### <a name="job-execution"></a>Auftragsausführung
Sie können ab sofort mithilfe der auf SSH-Schlüsseln basierenden Authentifizierung ein Remotedocker- oder Clustercomputeziel erstellen und darauf zugreifen, indem Sie die folgenden Schritte ausführen:
- Fügen Sie mithilfe des folgenden Befehls in der CLI ein Computeziel an.

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
[!NOTE] Die Option „-k“ (oder „--use-azureml-ssh-key“) im Befehl gibt an, dass ein SSH-Schlüssel generiert und verwendet werden soll.

- Azure ML Workbench generiert einen öffentlichen Schlüssel und gibt diesen in Ihrer Konsole aus. Melden Sie sich beim Computeziel mit demselben Benutzernamen an, und fügen Sie die Datei „~/.ssh/authorized_keys“ mit diesem öffentlichen Schlüssel an.

- Sie können dieses Computeziel vorbereiten und zur Ausführung verwenden, und Azure ML Workbench verwendet diesen Schlüssel für die Authentifizierung.  

Weitere Informationen zum Erstellen von Computezielen finden Sie unter [Konfigurieren des Azure Machine Learning-Experimentieren-Diensts](experimentation-service-configuration.md).

### <a name="visual-studio-tools-for-ai"></a>Visual Studio-Tools für AI
- Ab sofort werden die [Visual Studio-Tools für AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017) unterstützt. 

### <a name="command-line-interface-cli"></a>Befehlszeilenschnittstelle (Command Line Interface, CLI)
- Der neu hinzugefügte Befehl `az ml datasource create` ermöglicht das Erstellen einer Datenquellendatei über die Befehlszeile.

### <a name="model-management-and-operationalization"></a>Modellverwaltung und Operationalisierung
- [Alle AML-Container sind nach der Operationalisierung mit Azure IoT Edge-Geräten kompatibel (keine zusätzlichen Schritte erforderlich)](http://aka.ms/aml-iot-edge-blog) 
- Verbesserungen an den Fehlermeldungen in der o16n-CLI
- Fehlerbehebungen für die Portalbenutzeroberfläche für die Modellverwaltung  
- Konsistente Groß- und Kleinschreibung für Modellverwaltungsattribute auf der Detailseite
- Timeout für Aufrufe zur Echtzeitbewertung auf 60 Sekunden festgelegt
- Liste der registrierten Modelle und Detailansichten im Azure-Portal verfügbar

![Modelldetails im Portal](media/release-notes-sprint-2/model-list.jpg)

![Modellübersicht im Portal](media/release-notes-sprint-2/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Deep Learning in Spark mit [GPU-Unterstützung](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Unterstützung für Resource Manager-Vorlagen für eine einfache Ressourcenbereitstellung
- Unterstützung für das SparklyR-Ökosystem
- [AZTK-Integration](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Beispielprojekte
- Die [Iris](https://github.com/Azure/MachineLearningSamples-Iris)- und [MMLSpark](https://github.com/Azure/mmlspark)-Beispiele wurden mit der neuen Azure ML SDK-Version aktualisiert.

## <a name="breaking-changes"></a>ÄNDERUNGEN MIT AUSWIRKUNG AUF DIE LAUFFÄHIGKEIT
- Die Option `--type` in `az ml computetarget attach` wurde zu einem Unterbefehl hochgestuft. 

    - `az ml computetarget attach --type remotedocker` ist jetzt `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` ist jetzt `az ml computetarget attach cluster`
