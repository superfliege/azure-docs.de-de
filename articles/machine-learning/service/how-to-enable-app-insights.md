---
title: Aktivieren von Application Insights für den Azure Machine Learning-Dienst in Produktionsumgebungen
description: Erfahren Sie, wie Application Insights für den Azure Machine Learning-Dienst festgelegt wird, der in Azure Kubernetes Service bereitgestellt wurde
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 45871ab515c7ffd9520b1d77d3fd1e77abcc29ef
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114565"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Überwachen Sie Ihre Azure Machine Learning-Modelle in Produktionsumgebungen mit Application Insights

In diesem Artikel erfahren Sie, wie Sie **Application Insights** für Ihren **Azure Machine Learning**-Dienst einrichten. Nach der Aktivierung gibt Ihnen Application Insights die Möglichkeit diese Punkte zu überwachen:
* Anforderungsraten, Antwortzeiten und Fehlerraten
* Abhängigkeitsraten, Antwortzeiten und Fehlerraten
* Ausnahmen

Erfahren Sie [hier](../../application-insights/app-insights-overview.md) mehr über Application Insights. 

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalt dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
* Ein trainiertes Machine Learning-Modell, das im Azure Kubernetes Service (AKS) bereitgestellt werden soll. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.
* Ein [AKS-Cluster](how-to-deploy-to-aks.md).

## <a name="enable--disable-in-the-portal"></a>Aktivieren und Deaktivieren im Portal

Sie können Application Insights im Azure-Portal aktivieren und deaktivieren.

### <a name="enable"></a>Aktivieren

1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com).

1. Navigieren Sie zu Ihren Bereitstellungen, und wählen Sie den Dienst aus, für den Sie Application Insights aktivieren möchten.

   [![Bereitstellungen](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Klicken Sie auf **Bearbeiten**, und wechseln Sie zu **Erweiterte Einstellungen**.

   [![Bearbeiten](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. Wählen Sie in **Erweiterte Einstellungen** **Application Insights-Diagnose aktivieren** aus.

   [![Bearbeiten](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 

### <a name="disable"></a>Disable
Führen Sie die folgenden Schritte aus, um Application Insights im Azure-Portal zu deaktivieren:

1. Melden Sie sich beim Azure-Portal unter https://portal.azure.com an.
1. Wechseln Sie zu Ihrem Arbeitsbereich.
1. Wählen Sie **Bereitstellungen**, dann **Dienst auswählen** und dann **Bearbeiten** aus.

   [![Bearbeiten](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. Deaktivieren Sie in **Erweiterte Einstellungen** die Option **AppInsights-Diagnose aktivieren**. 

   [![deaktivieren](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 

## <a name="enable--disable-from-the-sdk"></a>Aktivieren und Deaktivieren aus dem SDK

### <a name="update-a-deployed-service"></a>Aktualisieren eines bereitgestellten Diensts
1. Identifizieren Sie den Dienst in Ihrem Arbeitsbereich (ws= Name Ihres Arbeitsbereichs)

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Aktualisieren Sie Ihren Dienst, und aktivieren Sie Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Protokollieren von benutzerdefinierten Ablaufverfolgungen in Ihrem Dienst
Wenn Sie benutzerdefinierte Ablaufverfolgungen protokollieren möchten, befolgen Sie den [Standardbereitstellungsprozess für AKS](how-to-deploy-to-aks.md), und führen Sie dann die folgenden Aktionen aus:

1. Aktualisieren der Bewertungsdatei durch Hinzufügen von Druckanweisungen.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Aktualisieren der AKS-Konfiguration.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Erstellen und Bereitstellen des Images.](how-to-deploy-to-aks.md)  

### <a name="disable-tracking-in-python"></a>Deaktivieren der Nachverfolgung in Python

Verwenden Sie den folgenden Code, um Application Insights zu deaktivieren:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Evaluieren von Daten
Die Daten Ihres Diensts werden in Ihrem Application Insights-Konto in der gleichen Ressourcengruppe gespeichert, in dem sich auch der Arbeitsbereich Ihres Azure Machine Learning-Diensts befindet.
So zeigen Sie sie an:
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und klicken Sie auf Ihre Application Insights-Ressource. 
2. Auf der Registerkarte **Übersicht** wird der Standardsatz Metriken für Ihren Dienst angezeigt.

   [![Übersicht](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Um Ihre benutzerdefinierten Ablaufverfolgungen anzuzeigen, klicken Sie auf **Analyse**.
4. Klicken Sie im Schema-Abschnitt auf **Ablaufverfolgungen**; anschließend können Sie Ihre Abfrage **ausführen**. Die Daten sollten unten in einem Tabellenformat angezeigt werden und sollten den benutzerdefinierten Aufrufen in Ihrer Bewertungsdatei zugeordnet sein. 

   [![Benutzerdefinierte Ablaufverfolgungen](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Klicken Sie [hier](../../application-insights/app-insights-overview.md), um weitere Informationen über die Verwendung von Application Insights zu erhalten.
    

## <a name="example-notebook"></a>Beispielnotebook

Im Notebook [00.Getting Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) werden die in diesem Artikel behandelten Konzepte veranschaulicht.  Notebook abrufen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie können Daten auch in Ihren Modellen in Produktionsumgebungen sammeln. Lesen Sie den Artikel [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md) 
