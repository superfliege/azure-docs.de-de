---
title: Nutzen eines Studio-Webdiensts mit einer Web-App-Vorlage
titleSuffix: Azure Machine Learning Studio
description: Verwenden Sie eine Web-App-Vorlage in Azure Marketplace, um einen Vorhersagewebdienst in Azure Machine Learning zu nutzen.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 164aae7c6a4df18b8a28e1ff152e9c61c60f06b9
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299060"
---
# <a name="consume-an-azure-machine-learning-studio-web-service-by-using-a-web-app-template"></a>Verwenden eines Azure Machine Learning Studio-Webdiensts mit einer Web-App-Vorlage

Sie können ein Vorhersagemodell entwickeln und als Azure-Webdienst bereitstellen, indem Sie Folgendes verwenden:
- Azure Machine Learning Studio
- Tools wie R oder Python

Anschließend haben Sie die Möglichkeit, auf das operationalisierte Modell über eine REST-API zuzugreifen.

Es gibt eine Reihe von Möglichkeiten, die REST-API zu nutzen und auf den Webdienst zuzugreifen. Beispielsweise können Sie eine Anwendung in C#, R oder Python schreiben und dabei den Beispielcode verwenden, der für Sie beim Bereitstellen des Webdiensts generiert wurde. (Der Beispielcode ist im [Machine Learning-Webdiensteportal](https://services.azureml.net/quickstart) oder im Webdienstdashboard von Machine Learning Studio verfügbar.) Sie können auch die Microsoft Excel-Beispielarbeitsmappe verwenden, die gleichzeitig für Sie erstellt wurde.

Aber die schnellste und einfachste Möglichkeit, auf Ihren Webdienst zuzugreifen, bieten die Web-App-Vorlagen, die im [Azure Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/) verfügbar sind.



## <a name="azure-machine-learning-web-app-templates"></a>Azure Machine Learning-Web-App-Vorlagen
Die Web-App-Vorlagen, die auf dem Azure Marketplace verfügbar sind, können eine benutzerdefinierte Web-App erstellen, der die Eingabedaten und die erwarteten Ergebnisse des Webdiensts bekannt sind. Dafür müssen Sie nur der Web-App Zugriff auf den Webdienst und die Daten gewähren, und die Vorlage übernimmt den Rest.

Zwei Vorlagen sind verfügbar:

* [Azure Machine Learning Studio-Web-App-Vorlage für den Anfrage-/Antwort-Dienst](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure Machine Learning Studio-Web-App-Vorlage für den Batchausführungsdienst](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Jede Vorlage erstellt mit dem API-URI und dem Schlüssel für den Webdienst eine ASP.NET-Beispielanwendung. Danach wird die Anwendung mithilfe der Vorlage als eine Website in Azure bereitgestellt.

Die Vorlage für den Anforderung-/Antwort-Dienst (Request-Response Service, RRS) erstellt eine Web-App, mit der Sie eine einzelne Zeile mit Daten an den Webdienst senden können, um ein einzelnes Ergebnis zu erhalten. Die Vorlage für den Batchausführungsdienst (Batch Execution Service, BES) erstellt eine Web-App, mit der Sie viele Zeilen mit Daten senden können, um mehrere Ergebnisse zu erhalten.

Es ist keine Codierung erforderlich, um diese Vorlagen zu verwenden. Sie geben lediglich den API-Schlüssel und den URI an, und die Vorlage erstellt die Anwendung für Sie.

So rufen Sie den API-Schlüssel und den Anforderungs-URI für einen Webdienst ab:

1. Klicken Sie oben im [Webdiensteportal](https://services.azureml.net/quickstart) auf **Webdienste**. Oder klicken Sie für einen klassischen Webdienst auf **Klassische Webdienste**.
2. Klicken Sie auf den Webdienst, auf den Sie zugreifen möchten.
3. Klicken Sie für einen klassischen Webdienst auf den Endpunkt, auf den Sie zugreifen möchten.
4. Klicken Sie oben auf **Nutzen**.
5. Kopieren Sie den primären oder den sekundären Schlüssel, und speichern Sie ihn.
6. Wenn Sie eine RRS-Vorlage erstellen, kopieren Sie den URI für **Anforderung/Antwort**, und speichern Sie ihn. Wenn Sie eine BES-Vorlage erstellen, kopieren Sie den URI für **Batchanforderungen**, und speichern Sie ihn.


## <a name="how-to-use-the-request-response-service-template"></a>Verwenden der Vorlage für den Anforderung-/Antwort-Dienst (RRS)
Führen Sie die folgenden Schritte aus, um die RRS-Web-App-Vorlage zu verwenden (siehe auch die folgende Abbildung).

![Prozess zur Verwendung einer RRS-Webvorlage][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Request URI][image4] -->

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Neu**, navigieren Sie zu **Azure ML-Anforderung-/Antwort-Dienst-Web-App**, und wählen Sie dann **Erstellen** aus.
3. Im Bereich **Erstellen**:

   * Geben Sie Ihrer Web-App einen eindeutigen Namen. Die URL der Web-App besteht aus diesem Namen gefolgt von **.azurewebsites.net**. Beispiel: **http://carprediction.azurewebsites.net**.
   * Wählen Sie das Azure-Abonnement und die Dienste aus, mit denen der Webdienst ausgeführt wird.
   * Klicken Sie auf **Erstellen**.

   ![Web-App erstellen][image5]

4. Wenn Azure das Bereitstellen der Web-App beendet hat, wählen Sie die **URL** auf der Seite mit den Web-App-Einstellungen in Azure aus, oder geben Sie die URL in einem Webbrowser ein. Geben Sie z.B. **http://carprediction.azurewebsites.net** ein.
5. Bei der ersten Ausführung der Web-App werden Sie aufgefordert, die **Post-URL der API** und den **API-Schlüssel** anzugeben. Geben Sie die Werte ein, die Sie zuvor gespeichert haben (Anforderungs-URI bzw. API-Schlüssel). Klicken Sie auf **Submit** (Senden).

   ![Post-URI und API-Schlüssel eingeben][image6]

6. Die Web-App zeigt die Seite **Web-App-Konfiguration** mit den aktuellen Einstellungen für den Webdienst an. Hier können Sie die von der Web-App verwendeten Einstellungen ändern.

   > [!NOTE]
   > Wenn Sie hier Einstellungen ändern, werden sie nur für diese Web-App geändert. Die Standardeinstellungen des Webdiensts werden nicht geändert. Angenommen, Sie ändern hier den Text in **Beschreibung**. In diesem Fall wird die im Webdienstdashboard in Machine Learning Studio angezeigte Beschreibung nicht geändert.
   >
   >

    Wenn Sie fertig sind, klicken Sie auf **Änderungen speichern** und dann auf **Zur Startseite wechseln**.

7. Auf der Startseite können Sie Werte eingeben, die an den Webdienst gesendet werden. Klicken Sie abschließend auf **Senden**. Das Ergebnis wird dann zurückgegeben.

Wenn Sie zur Seite **Konfiguration** zurückkehren möchten, wechseln Sie zur Seite **setting.aspx** der Web-App. Wechseln Sie z.B. zu **http://carprediction.azurewebsites.net/setting.aspx**. Sie werden aufgefordert, den API-Schlüssel erneut einzugeben. Sie benötigen diesen, um auf die Seite zuzugreifen und die Einstellungen zu aktualisieren.

Sie können die Web-App im Azure-Portal wie andere Web-Apps beenden, neu starten oder löschen. Solange sie ausgeführt wird, können Sie zur Webadresse der Startseite wechseln und neue Werte eingeben.

## <a name="how-to-use-the-batch-execution-service-template"></a>Verwenden der Vorlage für den Batchausführungsdienst (BES)
Sie können die BES-Web-App-Vorlage auf die gleiche Weise wie die RRS-Vorlage verwenden. Der Unterschied besteht darin, dass Sie mit der erstellten Web-App mehrere Datenzeilen senden und mehrere Ergebnisse empfangen können.

Die Eingabewerte für einen Webdienst mit Batchausführung können aus Azure Storage oder einer lokalen Datei stammen. Die Ergebnisse werden in einem Azure Storage-Container gespeichert. Daher benötigen Sie einen Azure Storage-Container, um die von der Web-App zurückgegebenen Ergebnisse darin zu speichern. Außerdem müssen Sie Ihre Eingabedaten bereithalten.

![Prozess zur Verwendung einer BES-Webvorlage][image2]

1. Führen Sie das gleiche Verfahren zum Erstellen der BES-Web-App wie für die RRS-Vorlage aus. Wechseln Sie in diesem Fall jedoch zur [Azure Machine Learning Studio-Web-App-Vorlage für den Batchausführungsdienst](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/), um die BES-Vorlage im Azure Marketplace zu öffnen. Klicken Sie auf **Web-App erstellen**.

2. Um anzugeben, wo die Ergebnisse gespeichert werden sollen, geben Sie die Informationen zum Zielcontainer auf der Startseite der Web-App an. Geben Sie zudem an, woher die Web-App die Eingabewerte erhält: aus einer lokalen Datei oder einem Azure Storage-Container.
   Klicken Sie auf **Submit** (Senden).

   ![Speicherinformationen][image7]

Die Web-App zeigt eine Seite mit dem Auftragsstatus an. Nach Abschluss des Auftrags werden Sie über den Speicherort der Ergebnisse im Azure Blob-Speicher informiert. Sie haben auch die Möglichkeit, die Ergebnisse in eine lokale Datei herunterzuladen.

## <a name="for-more-information"></a>Weitere Informationen
Weitere Informationen:

* Weitere Informationen zum Erstellen eines Machine Learning-Experiments mit Machine Learning Studio finden Sie unter [Erstellen Ihres ersten Experiments im Azure Machine Learning Studio](create-experiment.md).
* Informationen zum Bereitstellen Ihres Machine Learning-Experiments als Webdienst finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](publish-a-machine-learning-web-service.md).
* Andere Möglichkeiten für den Zugriff auf den Webdienst finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
