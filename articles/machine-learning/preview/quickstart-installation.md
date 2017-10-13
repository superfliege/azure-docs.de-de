---
title: "Schnellstartanleitung für die Installation für Azure Machine Learning-Dienste | Microsoft-Dokumentation"
description: "In dieser Schnellstartanleitung wird erläutert, wie Sie Azure Machine Learning-Ressourcen erstellen und Azure Machine Learning Workbench installieren."
services: machine-learning
author: hning86
ms.author: haining, raymondl, chhavib
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: hero-article
ms.date: 09/20/2017
ms.openlocfilehash: bb3ac21d2e8b8efd7b4f6943fcc3256f18cbdd68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Erstellen von Vorschaukonten für Azure Machine Learning und Installieren von Azure Machine Learning Workbench
Azure Machine Learning ist eine integrierte End-to-End-Lösung für Data Science-Anwendungen und erweiterte Analysen, die sich an professionelle Datenspezialisten richtet und zum Vorbereiten von Daten, Entwickeln von Experimenten und Bereitstellen von Modellen auf Cloudniveau verwendet werden kann.

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie Konten für Experimente und Modellverwaltung in der Vorschauversion der Azure Machine Learning-Dienste erstellen. Darüber hinaus wird gezeigt, wie Sie die Azure Machine Learning Workbench-Desktopanwendung und CLI-Tools installieren. Anschließend erhalten Sie eine kurze Übersicht über die Features der Azure Machine Learning-Vorschauversion. Darin wird mithilfe des zeitlosen Schwertlilien-Datasets ([Iris flower dataset](https://en.wikipedia.org/wiki/iris_flower_data_set)) ein Modell erstellt, um auf der Grundlage einiger physischen Merkmale die Schwertlilienart vorherzusagen.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Derzeit kann die Azure Machine Learning Workbench-Desktop-App nur unter folgenden Betriebssystemen installiert werden: 
- Windows 10
- Windows Server 2016
- macOS Sierra

>[!NOTE]
>macOS High Sierra wird noch nicht unterstützt.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-azure-machine-learning-accounts"></a>Erstellen von Azure Machine Learning-Konten
Verwenden Sie das Azure-Portal für die Bereitstellung von Azure Machine Learning-Konten. 
1. Wählen Sie in der linken oberen Ecke des Portals die Schaltfläche **Neu** (+) aus.

2. Geben Sie in der Suchleiste „Machine Learning“ ein. Wählen Sie das Suchergebnis mit dem Namen **Machine Learning-Experimentieren (Vorschauversion)** aus.  Klicken Sie auf das Sternsymbol, um diese Auswahl im Azure-Portal als Favorit festzulegen.

   ![Suche in Azure Machine Learning](media/quickstart-installation/portal-more-services.png)

3. Klicken Sie auf **+ Hinzufügen**, um ein neues Konto für Machine Learning-Experimentieren zu konfigurieren. Das ausführliche Formular wird geöffnet.

   ![Machine Learning-Experimentieren-Konto](media/quickstart-installation/portal-create-experimentation.png)

4. Geben Sie in das Formular für Machine Learning-Experimentieren die folgenden Informationen ein:

   Einstellung|Empfohlener Wert|Beschreibung
   ---|---|---
   Experimentation account name (Name des Experimentieren-Kontos) | _Eindeutiger Name_ |Wählen Sie einen eindeutigen Namen, der Ihr Konto identifiziert. Sie können Ihren eigenen Namen oder einen Abteilungs- oder Projektnamen verwenden, der sich am besten für das Experiment eignet. Der Name muss zwischen 2 und 32 Zeichen lang sein und darf nur alphanumerische Zeichen und den Bindestrich „-“ enthalten. 
   Abonnement | _Ihr Abonnement_ |Das Azure-Abonnement, das Sie für das Experiment verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
   Ressourcengruppe | _Ihre Ressourcengruppe_ | Sie können entweder einen neuen Ressourcengruppennamen erstellen oder einen bereits vorhandenen Namen aus Ihrem Abonnement verwenden.
   Standort | _Die Region, die Ihren Benutzern am nächsten liegt_ | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist.
   Number of seats (Anzahl von Arbeitsplätzen) | 2 | Geben Sie die Anzahl von Arbeitsplätzen ein. Diese Auswahl wirkt sich auf den [Preis](https://azure.microsoft.com/pricing/details/machine-learning/) aus. Die ersten beiden Arbeitsplätze sind kostenlos. Verwenden Sie für diese Schnellstartanleitung zwei Arbeitsplätze. Sie können die Anzahl der Arbeitsplätze später im Azure-Portal nach Bedarf aktualisieren.
   Speicherkonto | _Eindeutiger Name_ | Wählen Sie **Create new** (Neu erstellen), und geben Sie einen Namen ein, um ein neues Azure-Speicherkonto zu erstellen. Aktivieren Sie alternativ **Use existing** (Vorhandenes verwenden), und wählen Sie Ihr vorhandenes Speicherkonto aus der Dropdownliste aus. Das Speicherkonto ist erforderlich. Es wird zum Speichern von Projektartefakten und Ausführungsverlaufsdaten verwendet. 
   Workspace for Experimentation account (Arbeitsbereich für Experimentieren-Konto) | _Eindeutiger Name_ | Geben Sie einen Namen für den neuen Arbeitsbereich an. Der Name muss zwischen 2 und 32 Zeichen lang sein und darf nur alphanumerische Zeichen und den Bindestrich „-“ enthalten.
   Assign owner for the workspace (Besitzer für den Arbeitsbereich zuweisen) | _Ihr Konto_ | Wählen Sie Ihr eigenes Konto als Besitzer des Arbeitsbereichs aus.
   Create Model Management Account (Konto für die Modellverwaltung erstellen) | *check* | Beim Erstellen des Experimentieren-Kontos haben Sie die Möglichkeit, auch das Konto für die Machine Learning-Modellverwaltung zu erstellen. Diese Ressource wird verwendet, wenn Sie bereit sind, Ihre Modelle als Echtzeitwebdienste bereitzustellen und zu verwalten. Es wird empfohlen, das Modellverwaltungskonto zur gleichen Zeit wie das Experimentieren-Konto zu erstellen.
   Kontoname | _Eindeutiger Name_ | Wählen Sie einen eindeutigen Namen, der Ihr Modellverwaltungskonto identifiziert. Sie können Ihren eigenen Namen verwenden oder einen Abteilungs- oder Projektnamen, der am besten zu dem Experiment passt. Der Name muss zwischen 2 und 32 Zeichen lang sein und darf nur alphanumerische Zeichen und den Bindestrich „-“ enthalten. 
   Model Management pricing tier (Modellverwaltungstarif) | **DEVTEST** | Klicken Sie auf **Es wurde kein Tarif ausgewählt.**, um den Tarif für das neue Modellverwaltungskonto anzugeben. Wählen Sie zum Einsparen von Kosten den Tarif **DEVTEST**, sofern er in Ihrem Abonnement verfügbar ist (eingeschränkte Verfügbarkeit). Wählen Sie andernfalls den Tarif S1. Klicken Sie auf **OK**, um die Tarifauswahl zu speichern. 
   An Dashboard anheften | _check_ | Aktivieren Sie die Option **An Dashboard anheften**, um das Machine Learning-Experimentieren-Konto komfortabel auf der Titelseite des Dashboards im Azure-Portal nachverfolgen zu können.

5. Klicken Sie auf **Erstellen**, um den Erstellungsprozess zu starten.

6. Klicken Sie oben rechts auf der Symbolleiste im Azure-Portal auf **Benachrichtigungen** (Glockensymbol), um den Bereitstellungsprozess zu überwachen. 

   Benachrichtigung: Die Bereitstellung wird ausgeführt... Der Status ändert sich anschließend in „Bereitstellung erfolgreich“. Bei erfolgreicher Bereitstellung wird die Seite Ihres Machine Learning-Experimentieren-Kontos angezeigt.
   
   ![Benachrichtigungen im Azure-Portal](media/quickstart-installation/portal-notification.png)

Führen Sie abhängig von dem auf dem lokalen Computer verwendeten Betriebssystem die Schritte in einem der nächsten beiden Abschnitte aus, um Azure Machine Learning Workbench auf Ihrem Computer zu installieren. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Installieren von Azure Machine Learning Workbench unter Windows
Installieren Sie Azure Machine Learning Workbench auf dem Computer unter Windows 10, Windows Server 2016 oder einem neueren Betriebssystem.

1. Laden Sie den aktuellen Azure Machine Learning Workbench-Installer **[AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi)** herunter.

2. Doppelklicken Sie im Explorer auf den heruntergeladenen Installer _AmlWorkbenchSetup.msi_.

   >[!IMPORTANT]
   >Laden Sie den Installer vollständig auf den Datenträger herunter, und starten Sie ihn von dort. Starten Sie ihn nicht direkt über das Download-Widget des Browsers.

3. Schließen Sie die Installation anhand der Anweisungen auf dem Bildschirm ab.

   Der Installer lädt alle erforderlichen abhängigen Komponenten wie Python, Miniconda und andere zugehörige Bibliotheken herunter. Die Installation aller Komponenten kann etwa eine halbe Stunde dauern. 

4. Azure Machine Learning Workbench ist jetzt im folgenden Verzeichnis installiert:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>Installieren von Azure Machine Learning Workbench unter macOS
Installieren Sie Azure Machine Learning Workbench auf dem Computer unter macOS Sierra.

1. Installieren Sie die OpenSSL-Bibliothek mithilfe von [Homebrew](http://brew.sh). Ausführlichere Informationen finden Sie unter [Voraussetzungen für .NET Core unter Mac](https://docs.microsoft.com/dotnet/core/macos-prerequisites).
   ```
   # install Homebrew first if you don't have it already
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # install latest openssl needed for .NET Core 1.x
   brew update
   brew install openssl
   mkdir -p /usr/local/lib
   ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
   ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
   ```

2. Laden Sie den aktuellen Azure Machine Learning Workbench-Installer **[AmlWorkbench.dmg](https://aka.ms/azureml-wb-dmg)** herunter.

   >[!IMPORTANT]
   >Laden Sie den Installer vollständig auf den Datenträger herunter, und starten Sie ihn von dort. Starten Sie ihn nicht direkt über das Download-Widget des Browsers.

3. Doppelklicken Sie im Finder auf den heruntergeladenen Installer _AmlWorkbench.dmg_.

4. Schließen Sie die Installation anhand der Anweisungen auf dem Bildschirm ab.

   Der Installer lädt alle erforderlichen abhängigen Komponenten wie Python, Miniconda und andere zugehörige Bibliotheken herunter. Die Installation aller Komponenten kann etwa eine halbe Stunde dauern. 

5. Azure Machine Learning Workbench ist jetzt im folgenden Verzeichnis installiert: 

   `/Applications/AmlWorkbench.app`

## <a name="run-azure-machine-learning-workbench-to-log-in-the-first-time"></a>Ausführen von Azure Machine Learning Workbench für die erste Anmeldung
1. Wenn die Installation abgeschlossen ist, klicken Sie auf dem letzten Bildschirm des Installers auf die Schaltfläche **Launch Workbench** (Workbench starten). Wenn Sie den Installer geschlossen haben, navigieren Sie zur Verknüpfung für Machine Learning Workbench auf dem Desktop, und starten Sie das Menü **Azure Machine Learning Workbench**, um die App zu starten.

2. Melden Sie sich mit dem gleichen Konto bei Workbench an, das Sie zuvor zum Bereitstellen der Azure-Ressourcen verwendet haben. 

3. Wenn der Anmeldevorgang erfolgreich war, versucht Workbench, die zuvor erstellten Machine Learning-Experimentieren-Konten zu finden. Die Anwendung sucht nach allen Azure-Abonnements, auf die Ihre Anmeldeinformationen Zugriff haben. Wenn mindestens ein Experimentieren-Konto gefunden wird, wird Workbench mit diesem Konto geöffnet. Anschließend werden die in diesem Konto gefundenen Arbeitsbereiche und Projekte aufgeführt. 

   >[!TIP]
   > Wenn Sie Zugriff auf mehr als ein Experimentieren-Konto haben, können Sie durch Klicken auf das Avatarsymbol in der unteren linken Ecke der Workbench-App zu einem anderen Konto wechseln.

Informationen zum Erstellen einer Umgebung für die Bereitstellung Ihrer Webdienste finden Sie unter [Model management setup](deployment-setup-configuration.md) (Einrichtung der Modellverwaltung).

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
1. Starten Sie die Azure ML Workbench-App, und melden Sie sich an. 

2. Klicken Sie auf **Datei** --> **Neues Projekt** (oder auf das **+**-Zeichen im Bereich **PROJEKTE**). 

3. Füllen Sie die Felder **Projektname** und **Projektverzeichnis** aus. Die **Projektbeschreibung** ist optional, aber hilfreich. Lassen Sie das Feld **Visualstudio.com GIT Repository URL** (Git-Repository-URL für Visualstudio.com) vorerst leer. Wählen Sie einen Arbeitsbereich aus, und legen Sie als Projektvorlage **Classifying Iris** (Klassifizieren von Schwertlilien) fest.

   >[!TIP]
   >Optional können Sie in das Textfeld des Git-Repositorys die URL eines Git-Repositorys eingeben, das in einem [VSTS (Visual Studio Team Service)](https://www.visualstudio.com)-Projekt gehostet wird. Dieses Git-Repository muss bereits vorhanden und leer sein. Es darf keinen Masterbranch enthalten. Darüber hinaus benötigen Sie Schreibzugriff auf das Repository. Wenn Sie jetzt ein Git-Repository hinzufügen, können Sie später Roaming- und Freigabeszenarien aktivieren. [Weitere Informationen](using-git-ml-project.md)

4. Klicken Sie auf die Schaltfläche **Erstellen**, um das Projekt zu erstellen. Ein neues Projekt wird erstellt und für Sie geöffnet. Nun können Sie die Projektstartseite, Datenquellen, Notebooks und Quellcodedateien untersuchen. 

    >[!TIP]
    >Sie können das Projekt auch in VS Code oder anderen Editoren öffnen. Konfigurieren Sie dazu einfach einen IDE-Link (Integrated Development Environment, integrierte Entwickungsumgebung), und öffnen Sie dann das Projektverzeichnis darin. [Weitere Informationen](how-to-configure-your-IDE.md) 

## <a name="run-a-python-script"></a>Ausführen eines Python-Skripts
Führen Sie nun ein Skript auf dem lokalen Computer aus. 

1. Jedes Projekt wird mit seiner eigenen **Projektdashboard**-Seite geöffnet. Wählen Sie auf der Befehlszeile oben in der Anwendung links von der Schaltfläche „Ausführen“ `local` als Ausführungsziel und `iris_sklearn.py` als auszuführendes Skript.  Das Beispiel enthält weitere Dateien, die Sie sich später ansehen können. 

   ![Abbildung](media/quickstart-installation/run_control.png)

2. Geben Sie `0.01` in das Textfeld **Argumente** ein. Diese Zahl wird im Code verwendet, um die Regularisierungsrate festzulegen. Mit diesem Wert wird konfiguriert, wie das lineare Regressionsmodell trainiert wird. 

3. Klicken Sie auf die Schaltfläche **Ausführen**, um mit der Ausführung von `iris_sklearn.py` auf Ihrem Computer zu beginnen. 

   Dieser Code verwendet den Algorithmus für [logistische Regression](https://en.wikipedia.org/wiki/logistic_regression) aus der beliebten Python-Bibliothek [scikit-learn](http://scikit-learn.org/stable/index.html), um das Modell zu erstellen.

4. Das Panel **Aufträge** wird von rechts eingeblendet, falls es noch nicht angezeigt wird. Im Panel wird ein `iris_sklearn`-Auftrag hinzugefügt. Der Status ändert sich von **Wird übermittelt...** in **Wird ausgeführt...**, wenn mit der Ausführung des Auftrags begonnen wird. Nach einigen Sekunden ändert er sich in **Abgeschlossen**. 

5. Herzlichen Glückwunsch. Sie haben ein Python-Skript in Azure ML Workbench ausgeführt.

6. Wiederholen Sie die Schritte 2 bis 4 mehrmals. Verwenden Sie jedes Mal andere Argumentwerte im Bereich von `10` bis `0.001`.

## <a name="view-run-history"></a>Anzeigen des Ausführungsverlaufs
1. Navigieren Sie zur Ansicht **Ausführungen**, und klicken Sie in der Ausführungsliste auf **iris_sklearn.py**. Das Dashboard mit dem Ausführungsverlauf für `iris_sklearn.py` wird geöffnet. Es zeigt alle Ausführungen für `iris_sklearn.py`. 

   ![Abbildung](media/quickstart-installation/run_view.png)

2. Auf dem Dashboard mit dem Ausführungsverlauf werden darüber hinaus die wichtigsten Metriken, eine Reihe von Standarddiagrammen und eine Liste der Metriken für jede Ausführung angezeigt. Passen Sie diese Ansicht durch Sortieren, Filtern und Anpassen der Konfigurationen an. Klicken Sie dazu auf das Konfigurations- oder das Filtersymbol.

   ![Abbildung](media/quickstart-installation/run_dashboard.png)

3. Klicken Sie auf eine abgeschlossene Ausführung. Eine ausführliche Ansicht für diese spezielle Ausführung (einschließlich zusätzlicher Metriken, der erstellten Dateien und anderer möglicherweise nützlicher Protokolle) wird angezeigt.

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun ein Konto für Azure Machine Learning-Experimentieren und ein Konto für die Azure Machine Learning-Modellverwaltung erstellt. Sie haben die Azure Machine Learning Workbench-Desktop-App und -Befehlszeilenschnittstelle installiert. Sie haben ein neues Projekt und durch Ausführung eines Skripts ein Modell erstellt und den Ausführungsverlauf des Skripts untersucht.

Eine ausführlichere Erläuterung dieses Workflows, u.a. Informationen zum Bereitstellen des Schwertlilienmodells als Webdienst, finden Sie im Tutorial zum Klassifizieren von Schwertlilien. Dieses Tutorial in voller Länge enthält ausführliche Schritte für die [Datenvorbereitung](tutorial-classifying-iris-part-1.md), das [Experimentieren](tutorial-classifying-iris-part-2.md) und die [Modellverwaltung](tutorial-classifying-iris-part-3.md). 

> [!div class="nextstepaction"]
> [Tutorial zum Klassifizieren von Schwertlilien](tutorial-classifying-iris-part-1.md)
