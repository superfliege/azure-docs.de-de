---
title: Konfigurieren des Azure Machine Learning-Experimentieren-Diensts | Microsoft-Dokumentation
description: "Dieser Artikel bietet einen allgemeinen Überblick über den Azure Machine Learning-Experimentieren-Dienst mit Anweisungen zum Konfigurieren."
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 5635ef890537a2f37d9d6e9066d0258fc0cb346e
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Konfigurieren des Azure Machine Learning-Experimentieren-Diensts

## <a name="overview"></a>Übersicht
Mit dem Azure Machine Learning-Experimentieren-Dienst können Datenanalysten ihre Experimente ausführen, indem sie die Ausführungs- und Ausführungsverwaltungsfunktionen von Azure Machine Learning verwenden. Im Rahmen des Diensts wird ein Framework für flexible Experimente mit schnellen Iterationen bereitgestellt. Mit Azure Machine Learning Workbench können Sie mit lokalen Ausführungen auf Ihrem Computer beginnen und einen einfachen Pfad zum vertikalen und horizontalen Hochskalieren in andere Umgebungen nutzen, z.B. virtuelle Data Science-Remotecomputer mit GPU oder HDInsight-Cluster mit Ausführung von Spark.

Der Experimentieren-Dienst wurde konzipiert, um isolierte, reproduzierbare und einheitliche Ausführungen Ihrer Experimente zu ermöglichen. Er dient Ihnen als Hilfe beim Verwalten Ihrer Computeziele, Ausführungsumgebungen und Laufzeitkonfigurationen. Indem Sie die Azure Machine Learning Workbench-Funktionen für die Ausführung und Ausführungsverwaltung nutzen, können Sie leicht zwischen unterschiedlichen Umgebungen wechseln. 

Sie können ein Python- oder PySpark-Skript in einem Workbench-Projekt lokal oder bedarfsabhängig in der Cloud ausführen. 

Sie können Ihre Skripts in folgenden Umgebungen ausführen: 

* Python-Umgebung (3.5.2) auf Ihrem lokalen Computer, installiert von Workbench.
* Conda Python-Umgebung in einem Docker-Container auf einem lokalen Computer
* Conda Python-Umgebung in einem Docker-Container auf einem Linux-Remotecomputer (Beispiel: [Ubuntu-basierte DSVM in Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu))
* [HDInsight für Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) in Azure

>[!IMPORTANT]
>Der Azure Machine Learning-Experimentieren-Dienst unterstützt derzeit Python 3.5.2 und Spark 2.1.11 als Python- bzw. Spark-Laufzeitversionen. 


### <a name="key-concepts-in-experimentation-service"></a>Schlüsselkonzepte im Experimentieren-Dienst
Es ist wichtig, dass Sie mit den folgenden Konzepten der Azure Machine Learning-Experimentieren-Ausführung vertraut sind. In den nachfolgenden Abschnitten wird ausführlich beschrieben, wie diese Konzepte genutzt werden können. 

#### <a name="compute-target"></a>Computeziel
Mit dem _Computeziel_ wird angegeben, wo Ihr Programm ausgeführt werden soll, z.B. auf Ihrem Desktop, auf der Docker-Remoteinstanz auf einer VM oder in einem Cluster. Das Computeziel muss adressierbar und für Sie zugänglich sein. Mit Workbench haben Sie die Möglichkeit, Computeziele zu erstellen und mit der Workbench-Anwendung und der CLI zu verwalten. 

Mit dem Befehl _az ml computetarget attach_ in der CLI können Sie ein Computeziel erstellen, das Sie in Ihren Ausführungen verwenden können.

Folgende Computeziele werden unterstützt:
* Lokale Python-Umgebung (3.5.2) auf Ihrem Computer, installiert von Workbench.
* Lokale Docker-Instanz auf Ihrem Computer
* Docker-Remoteinstanz auf Linux-Ubuntu-VMs (Beispiel: [Ubuntu-basierte DSVM in Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu))
* [HDInsight für Spark-Cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/) in Azure

Der Experimentieren-Dienst unterstützt derzeit Python 3.5.2 und Spark 2.1.11 als Python- bzw. Spark-Laufzeitversionen. 

>[!IMPORTANT]
> Windows-VMs mit Docker werden **nicht** als Remotecomputeziele unterstützt.

#### <a name="execution-environment"></a>Ausführungsumgebung
Mit der _Ausführungsumgebung_ werden die Ausführungszeitkonfiguration und die Abhängigkeiten definiert, die zum Ausführen des Programms in Workbench benötigt werden.

Sie verwalten ihre lokale Ausführungsumgebung mit Ihren bevorzugten Tools und Paket-Managern, wenn die Ausführung über die Standardlaufzeit von Workbench erfolgt. 

Conda wird zum Verwalten von lokalen Dockerausführungen bzw. Docker-Remoteausführungen und HDInsight-basierten Ausführungen verwendet. Für diese Computeziele wird die Konfiguration der Ausführungsumgebung über die Dateien **Conda_dependencies.yml** und **Spark_dependencies.yml** verwaltet. Diese Dateien sind in Ihrem Projekt im Ordner **aml_config** enthalten.

**Folgende Laufzeiten für Ausführungsumgebungen werden unterstützt:**
* Python 3.5.2
* Spark 2.1.11

### <a name="run-configuration"></a>Laufzeitkonfiguration
Zusätzlich zum Computeziel und zur Ausführungsumgebung wird von Azure Machine Learning ein Framework zum Definieren und Ändern von *Laufzeitkonfigurationen* bereitgestellt. Für unterschiedliche Ausführungen Ihres Experiments sind im Rahmen von iterativen Experimenten unter Umständen auch unterschiedliche Konfigurationen erforderlich. Es kann sein, dass Sie das „Sweeping“ für verschiedene Parameterbereiche durchführen, verschiedene Datenquellen nutzen und Spark-Parameter optimieren. Der Experimentieren-Dienst stellt ein Framework zum Verwalten der Laufzeitkonfiguration bereit.

Wenn Sie den Befehl _az ml computetarget attach_ ausführen, werden in Ihrem Projekt im Ordner **aml_config** zwei Dateien erstellt. Hierbei handelt es sich um eine COMPUTE- und eine RUNCONFIG-Datei im folgenden Format: _<Name_Ihres_Computeziels>.compute_ und _<Name_Ihres_Computeziels>.runconfig_. Die RUNCONFIG-Datei wird automatisch für Sie erstellt, wenn Sie ein Computeziel erstellen. Sie können andere Laufzeitkonfigurationen erstellen und verwalten, indem Sie in der CLI den Befehl _az ml runconfigurations_ verwenden. Außerdem können Sie sie in Ihrem Dateisystem erstellen und bearbeiten.

Für die Laufzeitkonfiguration in Workbench können Sie auch Umgebungsvariablen angeben. Sie können diese Umgebungsvariablen angeben und in Ihrem Code verwenden, indem Sie in Ihrer RUNCONFIG-Datei den folgenden Abschnitt hinzufügen: 

```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

Auf diese Umgebungsvariablen kann in Ihrem Code zugegriffen werden. Mit dem folgenden Python-Codeausschnitt wird beispielsweise die Umgebungsvariable „EXAMPLE_ENV_VAR1“ ausgegeben:
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**Die folgende Abbildung zeigt den allgemeinen Ablauf für die erste Experimentausführung.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Experimentausführungsszenarien
In diesem Abschnitt geht es um Ausführungsszenarien, und es wird beschrieben, wie in Azure Machine Learning Experimente ausgeführt werden: lokal, auf einem virtuellen Remotecomputer und in einem HDInsight-Cluster. Dieser Abschnitt enthält eine exemplarische Vorgehensweise mit den Schritten von der Erstellung eines Computeziels bis zur Ausführung Ihrer Experimente.

>[!NOTE]
>Für den restlichen Teil dieses Artikels verwenden wir die CLI-Befehle (Befehlszeilenschnittstelle), um die Konzepte und Funktionen zu veranschaulichen. Die hier beschriebenen Funktionen können auch von Workbench aus verwendet werden.

## <a name="launching-the-cli"></a>Starten der CLI
Eine einfache Möglichkeit zum Starten der CLI ist das Öffnen eines Projekts in Workbench und das Navigieren zu **Datei > Eingabeaufforderung öffnen**.

![](media/experimentation-service-configuration/opening-cli.png)

Mit diesem Befehl wird ein Terminalfenster gestartet, in dem Sie Befehle zum Ausführen von Skripts im aktuellen Projektordner eingeben können. Dieses Terminalfenster wird mit der Python 3.5.2-Umgebung konfiguriert, die über Workbench installiert wird.

>[!NOTE]
> Wenn Sie im Befehlsfenster einen Befehl vom Typ _az ml_ ausführen, müssen Sie gegenüber Azure authentifiziert sein. In der CLI wird ein unabhängiger Authentifizierungscache und anschließend die Desktop-App verwendet. Mit der Anmeldung bei Workbench sind Sie also nicht gegenüber Ihrer CLI-Umgebung authentifiziert. Führen Sie die unten angegebenen Schritte aus, um sich zu authentifizieren. Das Authentifizierungstoken wird für einen bestimmten Zeitraum lokal zwischengespeichert, sodass Sie diese Schritte nur wiederholen müssen, wenn das Token abläuft. Führen Sie die folgenden Befehle aus, wenn das Token abläuft oder wenn Authentifizierungsfehler angezeigt werden:

```
# to authenticate 
$ az login

# to list subscriptions
$ az account list -o table

# to set current subscription to a particular subscription ID 
$ az account set -s <subscription_id>

# to verify your current Azure subscription
$ az account show
```

>[!NOTE] 
>Stellen Sie beim Ausführen des Befehls _az ml_ in einem Projektordner sicher, dass das Projekt zu einem Azure Machine Learning-Experimentieren-Konto unter dem _aktuellen_ Azure-Abonnement gehört. Andernfalls können Ausführungsfehler auftreten.


## <a name="running-scripts-and-experiments"></a>Ausführen von Skripts und Experimenten
Mit Workbench können Sie Ihre Python- und PySpark-Skripts auf unterschiedlichen Computezielen ausführen, indem Sie den Befehl _az ml experiment submit_ verwenden. Für diesen Befehl ist die Definition einer Laufzeitkonfiguration erforderlich. 

Workbench erstellt eine entsprechende RUNCONFIG-Datei, wenn Sie ein Computeziel erstellen, aber Sie können den Befehl _az ml runconfiguration create_ für die Erstellung zusätzlicher Laufzeitkonfigurationen verwenden. Außerdem können Sie die Dateien mit der Laufzeitkonfiguration auch manuell bearbeiten.

Laufzeitkonfigurationen werden in Workbench als Teil der Benutzeroberfläche für Experimentausführungen angezeigt. 

>[!NOTE]
>Weitere Informationen zur Datei mit der Laufzeitkonfiguration finden Sie unter [Experiment Execution Configuration Reference](experimentation-service-configuration-reference.md) (Konfiguration der Experimentausführung – Referenz).

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Lokales Ausführen eines Skripts unter von Workbench installierter Laufzeit
Mit Workbench können Sie Ihre Skripts direkt unter der von Workbench installierten Python 3.5.2-Laufzeit ausführen. Diese Standardlaufzeit wird beim Einrichten von Workbench installiert und enthält Azure Machine Learning-Bibliotheken und -Abhängigkeiten. Ausführungsergebnisse und Artefakte für lokale Ausführungen werden weiterhin unter dem Ausführungsverlaufsdienst in der Cloud gespeichert.

Im Gegensatz zu Docker-basierten Ausführungen wird diese Konfiguration _nicht_ von Conda verwaltet. Sie müssen Paketabhängigkeiten für Ihre lokale Workbench-Python-Umgebung manuell bereitstellen.

Sie können den folgenden Befehl ausführen, um Ihr Skript lokal in der von Workbench installierten Python-Umgebung auszuführen. 

```
$az ml experiment submit -c local myscript.py
```

Ermitteln Sie den Pfad zur Python-Standardumgebung, indem Sie im Workbench-CLI-Fenster den folgenden Befehl eingeben.
```
$ conda env list
```

>[!NOTE]
>Das lokale Ausführen von PySpark direkt für lokale Spark-Umgebungen wird derzeit **nicht** unterstützt. PySpark-Skripts, die unter lokalen Docker-Instanzen ausgeführt werden, werden von Workbench unterstützt. Für das Docker-Basisimage von Azure Machine Learning ist Spark 2.1.11 vorinstalliert. 

_**Übersicht über die lokale Ausführung für ein Python-Skript:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Ausführen eines Skripts auf einer lokalen Docker-Instanz
Sie können Ihre Projekte auch per Experimentieren-Dienst in einem Docker-Container auf Ihrem lokalen Computer ausführen. Workbench stellt ein Docker-Basisimage bereit, das über Azure Machine Learning-Bibliotheken und Spark 2.1.11-Laufzeit verfügt, um lokale Spark-Ausführungen zu vereinfachen. Docker muss auf dem lokalen Computer bereits ausgeführt werden.

Zum Ausführen Ihres Python- oder PySpark-Skripts auf der lokalen Docker-Instanz können Sie in der CLI die folgenden Befehle verwenden:

```
$az ml experiment submit -c docker myscript.py
```
oder
```
az ml experiment submit --run-configuration docker myscript.py
```

Die Ausführungsumgebung auf der lokalen Docker-Instanz wird über das Docker-Basisimage von Azure Machine Learning vorbereitet. Workbench lädt dieses Image bei der ersten Ausführung herunter und überlagert es mit Paketen, die in Ihrer Datei „conda_dependencies.yml“ angegeben sind. Die erste Ausführung wird durch diesen Vorgang verlangsamt, aber die nachfolgenden Ausführungen laufen dann deutlich schneller ab, da in Workbench zwischengespeicherte Überlagerungen wiederverwendet werden. 

>[!IMPORTANT]
>Sie müssen zuerst den Befehl _az ml experiment prepare -c docker_ ausführen, um das Docker-Image für Ihre erste Ausführung vorzubereiten. Außerdem können Sie den Parameter **PrepareEnvironment** in der Datei „docker.runconfig“ auf „true“ festlegen. Mit dieser Aktion wird Ihre Umgebung im Rahmen der Laufzeitausführung automatisch vorbereitet.  

>[!NOTE]
>Wenn Sie ein PySpark-Skript in Spark ausführen, wird zusätzlich zu „conda_dependencies.yml“ auch die Datei „spark_dependencies.yml“ verwendet.

Bei der Ausführung Ihrer Skripts auf einem Docker-Image profitieren Sie von den folgenden Vorteilen:

1. Es wird sichergestellt, dass Ihr Skript auch in anderen Ausführungsumgebungen zuverlässig ausgeführt werden kann. Bei der Ausführung in einem Docker-Container können Sie lokale Verweise, die sich unter Umständen auf die Portabilität auswirken können, ermitteln und vermeiden. 

2. Sie können Code für Laufzeiten und Frameworks, deren Installation und Konfiguration komplex ist (z.B. Apache Spark), schnell testen, ohne dass Sie die Installation selbst durchführen müssen.


_**Übersicht über die lokale Docker-Ausführung für ein Python-Skript:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Ausführen eines Skripts auf einer Docker-Remoteinstanz
In einigen Fällen reichen die Ressourcen, die auf Ihrem lokalen Computer verfügbar sind, unter Umständen nicht zum Trainieren des gewünschten Modells aus. In dieser Situation können Sie Ihre Python- oder PySpark-Skripts mit dem Experimentieren-Dienst leicht auf leistungsstärkeren VMs ausführen, indem Sie die Docker-Remoteausführung nutzen. 

Der virtuelle Remotecomputer sollte die folgenden Anforderungen erfüllen:
* Auf dem virtuellen Remotecomputer sollte Linux-Ubuntu ausgeführt werden, und er sollte per SSH zugänglich sein. 
* Auf virtuellen Remotecomputern muss Docker ausgeführt werden.

>[!IMPORTANT]
> Windows-VMs mit Docker werden **nicht** als Remotecomputeziele unterstützt.


Sie können den folgenden Befehl verwenden, um für eine Docker-basierte Remoteausführung sowohl die Definition des Computeziels als auch die Laufzeitkonfiguration zu erstellen.

```
az ml computetarget attach --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" --type remotedocker
```

Nachdem Sie das Computeziel konfiguriert haben, können Sie zum Ausführen Ihres Skripts den folgenden Befehl verwenden.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Beachten Sie, dass die Ausführungsumgebung über die Spezifikationen in der Datei „conda_dependencies.yml“ konfiguriert wird. Die Datei „spark_dependencies.yml“ wird auch verwendet, wenn das PySpark-Framework in der RUNCONFIG-Datei angegeben ist. 

Da der Docker-Erstellungsprozess für virtuelle Remotecomputer genau dem Prozess für lokale Docker-Ausführungen entspricht, sollten Sie mit einer ähnlichen Ausführungsoberfläche rechnen.

>[!TIP]
>Wenn Sie es vorziehen, die Wartezeit durch die Erstellung des Docker-Image für Ihre erste Ausführung zu vermeiden, können Sie den folgenden Befehl verwenden, um das Computeziel vor dem Ausführen des Skripts vorzubereiten. az ml experiment prepare -c <remotedocker>


_**Übersicht über die Ausführung eines virtuellen Remotecomputers für ein Python-Skript:**_
![](media/experimentation-service-configuration/remote-vm-run.png)


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Ausführen eines Skripts in einem HDInsight-Cluster
HDInsight ist eine beliebte Plattform für die Big Data-Analyse mit Unterstützung von Apache Spark. Workbench ermöglicht das Experimentieren mit Big Data unter Verwendung von HDInsight Spark-Clustern. 

Sie können ein Computeziel und eine Laufzeitkonfiguration für einen HDInsight Spark-Cluster erstellen, indem Sie den folgenden Befehl verwenden:

```
$ az ml computetarget attach --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword" --type cluster 
```

>[!NOTE]
>Wenn Sie anstelle einer IP-Adresse einen FQDN nutzen und Ihr HDI Spark-Cluster den Namen _foo_ hat, befindet sich der SSH-Endpunkt auf dem Treiberknoten mit dem Namen _foo-ssh.azurehdinsight.net_. Vergessen Sie nicht das Postfix **-ssh** im Servernamen, wenn Sie einen FQDN für den Parameter _--address_ verwenden.


Nachdem Sie über den Computekontext verfügen, können Sie zum Ausführen Ihres PySpark-Skripts den folgenden Befehl verwenden:

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench führt die Vorbereitung und Verwaltung der Ausführungsumgebung im HDInsight-Cluster mit Conda durch. Die Konfiguration wird mit den Konfigurationsdateien _conda_dependencies.yml_ und _spark_dependencies.yml_ verwaltet. 

Sie benötigen SSH-Zugriff auf den HDInsight-Cluster, um Experimente in diesem Modus auszuführen. 

>[!NOTE]
>Die unterstützte Konfiguration sind HDInsight Spark-Cluster mit Linux (Ubuntu mit Python/PySpark 3.5.2 und Spark 2.1.11).

_**Übersicht über die HDInsight-basierte Ausführung für ein PySpark-Skript**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>Ausführen eines Skripts per GPU
Eine Anleitung zum Ausführen Ihrer Skripts per GPU finden Sie im Artikel [Verwenden von GPUs in Azure Machine Learning](how-to-use-gpu.md).


## <a name="next-steps"></a>Nächste Schritte
* [Create and Install Azure Machine Learning (Erstellen und Installieren von Azure Machine Learning)](quickstart-installation.md)
* [Modellverwaltung](model-management-overview.md)
