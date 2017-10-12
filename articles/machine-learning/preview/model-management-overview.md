---
title: "Konzeptionelle Übersicht über die Azure Machine Learning-Modellverwaltung | Microsoft-Dokumentation"
description: "Informationen zu den Modellverwaltungskonzepten für Azure Machine Learning"
services: machine-learning
author: nk773
ms.author: neerajkh, padou
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 31b859d86e82c92839462280721c5f84f1d923cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-management"></a>Azure Machine Learning-Modellverwaltung

Mit der Azure Machine Learning-Modellverwaltung können Sie Machine Learning-Workflows und -Modelle verwalten und bereitstellen. 

Die Modellverwaltung enthält Funktionen für:
- Versionsverwaltung der Modelle
- Nachverfolgung von Modellen in der Produktion
- Bereitstellung von Modellen für die Produktion über die Azure ML-Compute-Umgebung mit [Azure Container Service](https://azure.microsoft.com/services/container-service/) und [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Erstellung von Docker-Containern mit den Modellen und lokales Testen
- Automatisiertes erneutes Trainieren von Modellen
- Erfassung der Modelltelemetrie für handlungsrelevante Einblicke 

Die Azure Machine Learning-Modellverwaltung enthält eine Registrierung von Modellversionen. Darüber hinaus stellt es automatisierte Workflows zur Paketerstellung und Bereitstellung von Machine Learning-Containern als REST-APIs bereit. Die Modelle und ihre Laufzeitabhängigkeiten werden mit Vorhersage-APIs in Linux-basierte Docker-Container gepackt. 

Mit Azure Machine Learning-Compute-Umgebungen können Sie skalierbare Cluster zum Hosten der Modelle einrichten und verwalten. Die Compute-Umgebung basiert auf Azure Container Service. Azure Container Service enthält die automatische Offenlegung von Machine Learning-APIs als REST-API-Endpunkte mit den folgenden Funktionen:

- Authentifizierung
- Lastenausgleich
- Automatische Skalierung
- Verschlüsselung

Diese Funktionen werden von der Azure Machine Learning-Modellverwaltung über CLI, API und das Azure-Portal bereitgestellt. 

Die Azure Machine Learning-Modellverwaltung verwendet die folgenden Informationen:

 - eine Modelldatei oder ein Verzeichnis mit den Modelldateien
 - eine vom Benutzer erstellte Python-Datei, die eine Bewertungsfunktion für das Modell implementiert
 - eine Conda-Abhängigkeitsdatei mit den Laufzeitabhängigkeiten
 - eine Auswahl der Laufzeitumgebung 
 - eine Schemadatei für API-Parameter 

Diese Informationen werden zur Ausführung der folgenden Aktionen verwendet:

- Registrieren eines Modells
- Erstellen eines Manifests für das Erstellen eines Containers
- Erstellen eines Docker-Containerimages
- Bereitstellen eines Containers für Azure Container Service
 
Die folgende Abbildung gibt einen Überblick darüber, wie Modelle für den Cluster registriert und bereitgestellt werden: 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Erstellen und Verwalten von Modellen 
Mit der Azure Machine Learning-Modellverwaltung können Sie Modelle registrieren, um die Modellversionen in der Produktion nachzuverfolgen. Zur Vereinfachung der Reproduzierbarkeit und Governance erfasst der Dienst alle Abhängigkeiten und zugehörige Informationen. Für ausführlichere Informationen zur Leistung können Sie die Modelltelemetrie mithilfe des bereitgestellten SDK erfassen. Die Modelltelemetrie wird in einem vom Benutzer bereitgestellten Speicher archiviert. Sie kann später zum Analysieren der Modellleistung, für erneutes Training und genauere Einblicke in Ihr Unternehmen verwendet werden.

## <a name="create-and-manage-manifests"></a>Erstellen und Verwalten von Manifesten 
Modelle erfordern zusätzliche Artefakte für die Bereitstellung in der Produktion. Mit dem System können Sie ein Manifest erstellen, das Modell, Abhängigkeiten, Rückschlussskript (auch Bewertungsskript genannt), Beispieldaten, Schema usw. umfasst. Das Manifest fungiert als Rezept zur Erstellung eines Docker-Containerimages. Unternehmen können Manifeste automatisch generieren, verschiedene Versionen erstellen und die Manifeste verwalten. 

## <a name="create-and-manage-docker-container-images"></a>Erstellen und Verwalten von Docker-Containerimages 
Mit dem Manifest aus dem vorherigen Schritt können Sie Docker-basierte Containerimages in ihrer jeweiligen Umgebung erstellen. Docker-basierte Images in Containern verschaffen Unternehmen die Möglichkeit, diese Images flexibel in den folgenden Compute-Umgebungen auszuführen:

- [Azure Container Service basierend auf Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Lokale Containerdienste
- Entwicklungsumgebungen
- IoT-Geräte

Diese Docker-basierten Containerimages sind eigenständig und enthalten alle Abhängigkeiten, die zum Generieren von Vorhersagen erforderlich sind. 

## <a name="deploy-docker-container-images"></a>Bereitstellen von Docker-Containerimages 
Mit der Azure Machine Learning-Modellverwaltung können Sie Docker-basierte Containerimages mit einem einzigen Befehl für Azure Container Service bereitstellen, das von der ML-Compute-Umgebung verwaltet wird. Diese Bereitstellungen werden mit einem Front-End-Server erstellt, der die folgenden Funktionen enthält:

- Vorhersagen mit niedriger Latenz und mit Skalierung
- Lastenausgleich
- Automatische Skalierung von ML-Endpunkten
- Autorisierung per API-Schlüssel
- Swagger-Dokument für API

Mit den folgenden Konfigurationseinstellungen können Sie die Bereitstellungsskalierung und Telemetrie steuern:

- Systemprotokollierung und Modelltelemetrie für jede Webdienstebene: Sofern die Funktion aktiviert ist, werden alle stdout-Protokolle an [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) gestreamt. Die Modelltelemetrie wird in einem von Ihnen bereitgestellten Speicher archiviert. 
- Grenzwerte für automatische Skalierung und Parallelität: Mit diesen Einstellungen wird automatisch die Anzahl der bereitgestellten Container basierend auf der Auslastung im vorhandenen Cluster erhöht. Zudem steuern sie den Durchsatz und die Konsistenz der Vorhersagelatenz.

## <a name="consumption"></a>Nutzung 
Mit der Azure Machine Learning-Modellverwaltung werden zusammen mit dem Swagger-Dokument REST-APIs für das bereitgestellte Modell erstellt. Sie können bereitgestellte Modelle nutzen, indem Sie mithilfe von API-Schlüsseln und Modelleingaben die REST-APIs aufrufen, um so die Vorhersagen als Teil der Branchenanwendungen abzurufen. Zum Aufrufen von REST-APIs ist der Beispielcode in GitHub für die Programmiersprachen Java, [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py) und C# verfügbar. Die Befehlszeilenschnittstelle (CLI) für die Azure Machine Learning-Modellverwaltung stellt eine einfache Möglichkeit dar, mit diesen REST-APIs zu arbeiten. Verwenden Sie zur Nutzung der APIs einen einzelnen CLI-Befehl, eine Swagger-fähige Anwendung oder Curl. 

## <a name="retraining"></a>Erneutes Training 
Die Azure Machine Learning-Modellverwaltung stellt APIs bereit, mit denen Sie Ihre Modelle erneut trainieren können. Sie können die APIs auch zum Aktualisieren von vorhandenen Bereitstellungen durch aktualisierte Versionen des Modells verwenden. Als Teil des Data Science-Workflows wird das Modell in Ihrer Experimentumgebung erneut erstellt. Registrieren Sie anschließend das Modell über die Modellverwaltung, und aktualisieren Sie vorhandene Bereitstellungen. Updates werden mithilfe eines einzigen UPDATE-CLI-Befehls ausgeführt. Mit dem UPDATE-Befehl werden vorhandene Bereitstellungen aktualisiert, ohne dass die API-URL oder der Schlüssel verändert werden. Die Anwendungen, die das Modell nutzen, funktionieren weiterhin ohne Änderung des Codes und rufen mit dem neuen Modell bessere Vorhersagen ab.

Die folgende Abbildung zeigt den vollständigen Workflow basierend auf den beschriebenen Konzepten:

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) 
- Welche Datentypen werden unterstützt? Kann ich NumPy-Arrays als Eingabe direkt an den Webdienst übergeben?

   Wenn Sie eine Schemadatei bereitstellen, die mit dem generate_schema-SDK erstellt wurde, können Sie NumPy und/oder Pandas DF übergeben. Sie können auch alle serialisierbaren JSON-Eingaben übergeben. Oder Sie können Images als binär codierte Zeichenfolgen übergeben.

- Werden vom Webdienst mehrere Eingaben unterstützt oder verschiedene Eingaben analysiert? 

   Ja, Sie können mehrere Eingaben als Wörterbuch in eine JSON-Anforderung packen. Jede Eingabe entspricht dann einem einzelnen eindeutigen Wörterbuchschlüssel.

- Ist der Aufruf, der durch eine Anforderung an den Webdienst aktiviert wird, ein blockierender Aufruf oder ein asynchroner Aufruf?

   Wurde der Dienst mit Echtzeit-Option als Teil der CLI oder API erstellt, handelt es sich um einen blockierenden/synchronen Aufruf. Seine Geschwindigkeit soll der Echtzeit entsprechen. Auf der Clientseite können Sie ihn jedoch mit der asynchronen HTTP-Bibliothek aufrufen, um ein Blockieren des Clientthreads zu verhindern.

- Wie viele Anforderungen kann der Webdienst gleichzeitig verarbeiten?

   Dies hängt von der Skalierung des Clusters und des Webdiensts ab. Sie können den Dienst auf die hundertfache Anzahl von Replikaten hochskalieren, damit er eine Vielzahl von Anforderungen gleichzeitig verarbeiten kann. Sie können auch die maximale Anzahl von gleichzeitigen Anforderungen pro Replikat konfigurieren, um den Durchsatz des Diensts zu erhöhen.

- Wie viele Anforderungen kann der Webdienst in die Warteschlange stellen?

   Das können Sie selbst konfigurieren. Der Standardwert ist 10 Anforderungen pro einzelnes Replikat. Sie können den Wert jedoch erhöhen/verringern, um ihn an Ihre Anwendungsanforderungen anzupassen. In der Regel erhöht eine Erhöhung der Anzahl der Anforderungen in der Warteschlange den Durchsatz des Diensts, verschlechtert aber gleichzeitig die Wartezeiten. Damit die Wartezeiten konstant bleiben, können Sie für die Warteschlange einen niedrigen Wert festlegen (1-5) und gleichzeitig die Anzahl der Replikate erhöhen, die den Durchsatz verarbeiten. Sie können auch die automatische Skalierung aktivieren, damit sich die Anzahl der Replikate automatisch an die Auslastung anpasst. 

- Kann der gleiche Computer oder Cluster für mehrere Webdienst-Endpunkte verwendet werden?

   Absolut. Sie können Hunderte von Diensten/Endpunkten im gleichen Cluster ausführen. 

## <a name="next-steps"></a>Nächste Schritte
Eine Einführung in die Modellverwaltung finden Sie unter [Modellverwaltung konfigurieren](model-management-configuration.md).
