---
title: "Bereitstellungsphase des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Bereitstellungsphase Ihrer Data Science-Projekte."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: b49b3ab696c22928c5f5a4566e059345fd810588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="deployment"></a>Bereitstellung

In diesem Thema werden die Ziele, Aufgaben und Projektleistungen in Zusammenhang mit der **Bereitstellung** des Team Data Science-Prozesses behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus 2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Zielsetzung
* Modelle mit einer Datenpipeline werden in einer Produktionsumgebung oder produktionsähnlichen Umgebung bereitgestellt, um die endgültige Benutzerakzeptanz zu testen. 

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase geht es um die folgende Hauptaufgabe:

* **Operationalisieren des Modells**: Stellen Sie das Modell und die Pipeline in einer Produktionsumgebung oder produktionsähnlichen Umgebung zur Nutzung der Anwendung bereit.

### <a name="41-operationalize-a-model"></a>4.1 Operationalisieren eines Modells
Sobald wir eine Gruppe von Modellen mit adäquater Leistung haben, können diese für die Nutzung durch andere Anwendungen operationalisiert werden. Abhängig von den Geschäftsanforderungen erfolgen Vorhersagen entweder in Echtzeit oder auf Batchbasis. Modelle werden bereitgestellt, indem sie mit einer offenen API-Schnittstelle verfügbar gemacht werden. Dank der Schnittstelle kann das Modell einfach von verschiedenen Anwendungen genutzt werden, z.B. Onlinewebsites, Kalkulationstabellen, Dashboards bzw. Branchen- oder Back-End-Anwendungen. Beispiele für die Operationalisierung von Modellen mit einem Azure Machine Learning-Webdienst finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](../studio/publish-a-machine-learning-web-service.md). Es ist auch eine bewährte Methode, in das bereitgestellte Produktionsmodell und die bereitgestellte Datenpipeline Telemetriedaten und Überwachung zu integrieren. Diese Vorgehensweise erleichtert nachfolgende Systemstatus-Berichtserstellung und Problembehandlung.  

## <a name="artifacts"></a>Artefakte
* Statusdashboard mit Systemintegrität und wichtigen Metriken.
* Endgültiger Modellierungsbericht mit Bereitstellungsdetails.
* Dokument mit der endgültigen Lösungsarchitektur.


## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des Team Data Science-Prozess:

* [1. Geschäftliche Aspekte](lifecycle-business-understanding.md)
* [2. Datenerfassung und -auswertung](lifecycle-data.md)
* [3. Modellierung](lifecycle-modeling.md)
* [4. Bereitstellung](lifecycle-deployment.md)
* [5. Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess).