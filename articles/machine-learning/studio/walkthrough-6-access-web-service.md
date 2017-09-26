---
title: 'Schritt 6: Zugreifen auf den Machine Learning-Webdienst | Microsoft-Dokumentation'
description: "Exemplarische Vorgehensweise zur Entwicklung einer Vorhersagelösung – Schritt 6: Zugreifen auf einen aktiven Azure Machine Learning-Webdienst"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: d1a31bc95b249c3fa1caced138bbf3459eb6282c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Anleitung Schritt 6: Zugreifen auf den Azure Machine Learning-Webdienst

Dies ist der letzte Schritt der exemplarischen Vorgehensweise zum [Entwickeln einer Predictive Analytics-Lösung mit Azure Machine Learning](walkthrough-develop-predictive-solution.md)

1. [Erstellen eines Machine Learning-Arbeitsbereichs](walkthrough-1-create-ml-workspace.md)
2. [Hochladen vorhandener Daten](walkthrough-2-upload-data.md)
3. [Erstellen eines neuen Experiments](walkthrough-3-create-new-experiment.md)
4. [Trainieren und Bewerten der Modelle](walkthrough-4-train-and-evaluate-models.md)
5. [Bereitstellen des Webdiensts](walkthrough-5-publish-web-service.md)
6. **Zugreifen auf den Webdienst**

- - -
Im vorherigen Schritt in dieser exemplarischen Vorgehensweise haben wir einen Webdienst bereitgestellt, der unser Vorhersagemodell für Kreditrisiken verwendet. Jetzt sind Benutzer in der Lage, Daten an ihn zu senden und Ergebnisse zu erhalten. 

Der Webdienst ist ein Azure-Webdienst, der Daten auf eine von zwei Arten über REST-APIs empfangen und zurückgeben kann:  

* **Anfrage/Antwort:** Der Benutzer sendet über das HTTP-Protokoll eine oder mehrere Zeilen Kreditdaten an den Dienst, und dieser antwortet mit einem oder mehreren Sätzen von Ergebnissen.
* **Batchausführung:** Der Benutzer speichert Zeilen von Kreditdaten in einem Azure-Blob und sendet den Speicherorts des Blobs dann an den Dienst. Der Dienst bewertet alle Datenzeilen im Eingabeblob, speichert die Ergebnisse in einem anderen Blob und gibt die URL dieses Containers zurück.  

Die schnellste und einfachste Möglichkeit, auf einen klassischen Webdienst zuzugreifen, bietet die [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) (Azure ML-Web-App für den Anfrage-/Antwort-Dienst) oder die [Azure ML Batch Execution Service Web App Template ](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) (Azure ML-Web-App-Vorlage für den Batchausführungsdienst).

Die Web-App-Vorlagen können eine benutzerdefinierte Web-App erstellen, der die Eingabedaten und die zurückgegebenen Ergebnisse des Webdiensts bekannt sind. Dafür müssen Sie nur Zugriff auf den Webdienst und die Daten gewähren, und die Vorlage übernimmt den Rest.

Weitere Informationen zur Verwendung von Web-App-Vorlagen finden Sie unter [Verwenden eines Azure Machine Learning-Webdiensts mit einer Web-App-Vorlage](consume-web-service-with-web-app-template.md).

Sie können auch eine benutzerdefinierte Anwendung entwickeln, die mithilfe von in den Programmiersprachen R, C# und Python bereitgestelltem Startcode auf den Webdienst zugreift.

Umfassende Informationen finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](consume-web-services.md).


