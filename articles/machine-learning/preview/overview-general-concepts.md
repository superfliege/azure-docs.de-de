---
title: "Konzeptionelle Übersicht über die Features der Azure Machine Learning-Vorschauversion | Microsoft-Dokumentation"
description: "Eine konzeptionelle Übersicht über die Features der Vorschauversion von Azure Machine Learning, z.B. Abonnements, Konten, Arbeitsbereiche, Projekte usw."
services: machine-learning
author: serinakaye
ms.author: serinak
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 3d4ba2ca6f7adc8b51030c02d9e9eeb2b9995bb4
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning: Konzepte

Dieser Artikel definiert und beschreibt die Konzepte, mit denen Sie zum Verwenden von Azure Machine Learning vertraut sein müssen. 

![Hierarchie der Konzepte](media/overview-general-concepts/hierarchy.png)

- **Abonnement:** Ein Azure-Abonnement gewährt Ihnen Zugriff auf Ressourcen in Azure. Da Azure Machine Learning eng mit Compute-, Speicher-und vielen anderen Ressourcen und Diensten von Azure integriert ist, fordert Workbench an, dass jeder Benutzer Zugriff auf ein gültiges Azure-Abonnement hat. Benutzer müssen außerdem über ausreichende Berechtigungen innerhalb dieses Abonnements verfügen, um Ressourcen erstellen zu können.


- **Konto für Experimente:** Ein Konto für Experimente ist eine Azure-Ressource, die für Azure ML erforderlich ist, und ein Abrechnungsinstrument. Es enthält Ihre Arbeitsbereiche, die wiederum Projekte enthalten. Sie können einem Konto für Experimente mehrere Benutzer, sog. _Arbeitsplätze_, hinzufügen. Sie benötigen Zugriff auf ein Konto für Experimente, um mit Azure ML Workbench Experimente durchführen zu können. 


- **Modellverwaltungskonto:** Ein Modellverwaltungskonto ist auch eine Azure-Ressource, die Azure ML zum Verwalten von Modellen benötigt. Sie können damit Modelle und Manifeste registrieren, Webdienste in Containern erstellen und diese lokal oder in der Cloud implementieren. Außerdem handelt es sich um das andere Abrechnungsinstrument von Azure ML.


- **Arbeitsbereich:** Ein Arbeitsbereich stellt die primäre Komponente für die Freigabe und Zusammenarbeit in Azure ML dar. Projekte sind in einem Arbeitsbereich gruppiert. Ein Arbeitsbereich kann anschließend mit mehreren Benutzern gemeinsam genutzt werden, die dem Konto für Experimente hinzugefügt wurden.


- **Projekt:** In Azure Machine Learning ist ein Projekt der logische Container für die gesamte Arbeit, die beim Lösen eines Problems anfällt. Es ist einem einzelnen Dateiordner auf dem lokalen Datenträger zugeordnet, dem Sie Dateien und Unterordner hinzufügen können. Zur Quellcodeverwaltung und Zusammenarbeit kann ein Projekt optional einem Git-Repository zugeordnet werden.  

- **Experiment:** In Azure ML besteht ein Experiment aus mindestens einer Quellcodedatei, die an einem einzelnen Einstiegspunkt ausgeführt werden kann. Es kann Aufgaben wie Datenerfassung, Feature-Entwicklung, Modelltraining oder Modellauswertung umfassen. Derzeit unterstützt Azure ML nur Python- oder PySpark-Experimente.


- **Modell:** In Azure Machine Learning beziehen sich Modelle auf das Produkt eines Machine Learning-Experiments. Sie stellen Rezepte dar, die bei ordnungsgemäßer Anwendung auf Daten vorhergesagte Werte generieren. Modelle können in Test- oder Produktionsumgebungen bereitgestellt und zum Bewerten neuer Daten verwendet werden. Einmal in der Produktion können Modelle auf Leistungs- und Datenabweichungen überwacht und bei Bedarf neu trainiert werden. 

- **Computeziel:** Ein Computeziel ist die Computeressource, die Sie zum Ausführen Ihrer Experimente konfigurieren. Dabei kann es sich um Ihren lokalen Computer (Windows oder macOS), einen auf Ihrem lokalen Computer oder in einer Linux-VM in Azure ausgeführten Docker-Container oder einen HDInsight Spark-Cluster handeln.


- **Ausführung:** Der Experimentation Service definiert eine Ausführung als die Lebensdauer der Ausführung eines Experiments in einem Computeziel. Azure ML erfasst automatisch Informationen zu jeder einzelnen Ausführung und stellt den gesamten Verlauf eines bestimmten Experiments in Form eines Ausführungsverlaufs dar.

- **Umgebung:** In Azure Machine Learning kennzeichnet eine Umgebung eine bestimmte Computeressource, die zum Bereitstellen und Verwalten Ihrer Modelle verwendet wird. Je nach Kontext und Konfiguration kann es sich dabei um Ihren lokalen Computer, eine Linux-VM in Azure oder einen Kubernetes-Cluster in Azure Container Service handeln. Ihr Modell wird in einem Docker-Container gehostet, der in diesen Umgebungen ausgeführt und als REST-API-Endpunkt verfügbar gemacht wird.


- **Verwaltete Modelle:** Mit der Modellverwaltung können Sie Modelle als Webdienste bereitstellen, verschiedene Versionen Ihrer Modelle verwalten und deren Leistung und Metriken überwachen. Verwaltete Modelle werden bei einem Azure Machine Learning-Modellverwaltungskonto registriert.

- **Manifeste:** Wenn das Modellverwaltungssystem ein Modell in der Produktion bereitstellt, enthält es ein Manifest, welches das Modell, Abhängigkeiten, das Bewertungsskript, Beispieldaten und das Schema umfassen kann. Das Manifest ist das Rezept zur Erstellung eines Docker-Containerimages. Mithilfe der Modellverwaltung können Sie die Manifeste automatisch generieren, verschiedene Versionen erstellen und diese Manifeste verwalten. 


- **Images:** Sie können die Manifeste verwenden, um Docker-Images (neu) zu generieren. Docker-Images in Containern ermöglichen die flexible Ausführung in der Cloud, auf lokalen Computern oder IoT-Geräten. Images sind eigenständig und schließen alle Abhängigkeiten ein, die zum Bewerten neuer Daten mit Modellen erforderlich sind. 

- **Dienste:** Mithilfe der Modellverwaltung können Sie Modelle als gepackte Webdienste bereitstellen. Webdienstlogik und -abhängigkeiten werden in einem Image gekapselt. Jeder Webdienst besteht aus einer Gruppe von Containern, die auf dem Image basieren und bereit sind, Anforderungen an eine gegebene URL zu erfüllen. Ein Webdienst zählt als eine einzelne Bereitstellung.

