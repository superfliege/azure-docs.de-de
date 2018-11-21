---
title: Aktivieren von Application Insights für den Azure Machine Learning-Dienst in Produktionsumgebungen
description: Erfahren Sie, wie Application Insights für den Azure Machine Learning-Dienst zur Bereitstellung in Azure Kubernetes Service festgelegt wird.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 285486d5fe641d49ee21d7340b62f83d75862553
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578296"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Überwachen Sie Ihre Azure Machine Learning-Modelle in Produktionsumgebungen mit Application Insights

In diesem Artikel erfahren Sie, wie Sie Azure Application Insights für Ihren Azure Machine Learning-Dienst einrichten. In Application Insights haben Sie die Möglichkeit, Folgendes zu überwachen:
* Anforderungsraten, Antwortzeiten und Fehlerraten
* Abhängigkeitsraten, Antwortzeiten und Fehlerraten
* Ausnahmen.

[Erfahren Sie mehr über Application Insights](../../application-insights/app-insights-overview.md). 

>[!NOTE]
> Der Code in diesem Artikel wurde mit Version 0.1.74 des Azure Machine Learning SDK getestet.


## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erfüllen dieser Voraussetzungen finden Sie unter [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
* Ein trainiertes Machine Learning-Modell, das im Azure Kubernetes Service (AKS) bereitgestellt werden soll. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.
* Ein [AKS-Cluster](how-to-deploy-to-aks.md).

## <a name="enable-and-disable-in-the-portal"></a>Aktivieren und Deaktivieren im Portal

Sie können Application Insights im Azure-Portal aktivieren und deaktivieren.

### <a name="enable"></a>Aktivieren

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich.

1. Wählen Sie auf der Registerkarte **Bereitstellungen** den Dienst aus, für den Sie Application Insights aktivieren möchten.

   [![Liste der Dienste auf der Registerkarte „Bereitstellungen“](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Wählen Sie **Bearbeiten** aus.

   [![Schaltfläche „Bearbeiten“](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. Aktivieren Sie in **Erweiterte Einstellungen** das Kontrollkästchen **AppInsights-Diagnose aktivieren**.

   [![Aktiviertes Kontrollkästchen zum Aktivieren der Diagnose](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 

### <a name="disable"></a>Disable
1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich.
1. Wählen Sie **Bereitstellungen**, den Dienst und dann **Bearbeiten** aus.

   [![Schaltfläche „Bearbeiten“](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. Deaktivieren Sie in **Erweiterte Einstellungen** das Kontrollkästchen **AppInsights-Diagnose aktivieren**. 

   [![Deaktiviertes Kontrollkästchen zum Aktivieren der Diagnose](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 

## <a name="enable-and-disable-from-the-sdk"></a>Aktivieren und Deaktivieren aus dem SDK

### <a name="update-a-deployed-service"></a>Aktualisieren eines bereitgestellten Diensts
1. Suchen Sie den Dienst in Ihrem Arbeitsbereich. Der Wert für `ws` ist der Name des Arbeitsbereichs.

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Aktualisieren Sie Ihren Dienst, und aktivieren Sie Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Protokollieren von benutzerdefinierten Ablaufverfolgungen in Ihrem Dienst
Wenn Sie benutzerdefinierte Ablaufverfolgungen protokollieren möchten, befolgen Sie den [Standardbereitstellungsprozess für AKS](how-to-deploy-to-aks.md). Führen Sie dann folgende Schritte aus:

1. Aktualisieren Sie die Bewertungsdatei durch Hinzufügen von Druckanweisungen.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Aktualisieren Sie die AKS-Konfiguration.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Erstellen Sie das Image, und stellen Sie es bereit.](how-to-deploy-to-aks.md)  

### <a name="disable-tracking-in-python"></a>Deaktivieren der Nachverfolgung in Python

Verwenden Sie den folgenden Code, um Application Insights zu deaktivieren:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Evaluieren von Daten
Die Daten Ihres Diensts werden in Ihrem Application Insights-Konto in der gleichen Ressourcengruppe gespeichert, in dem sich auch Ihr Azure Machine Learning-Dienst befindet.
So zeigen Sie sie an:
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Machine Learning Service-Arbeitsbereich, und klicken Sie auf den Link „Application Insights“.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Wählen Sie die Registerkarte **Übersicht** aus, um einen Standardsatz von Metriken für Ihren Dienst anzuzeigen.

   [![Übersicht](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Um Ihre benutzerdefinierten Ablaufverfolgungen anzuzeigen, wählen Sie **Analyse** aus.
4. Wählen Sie im Abschnitt „Schema“ den Eintrag **Ablaufverfolgungen** aus. Wählen Sie dann **Ausführen** aus, um die Abfrage auszuführen. Die Daten sollten in einem Tabellenformat angezeigt werden und den benutzerdefinierten Aufrufen in Ihrer Bewertungsdatei zugeordnet sein. 

   [![Benutzerdefinierte Ablaufverfolgungen](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Weitere Informationen zu Application Insights finden Sie unter [Was ist Application Insights?](../../application-insights/app-insights-overview.md)
    

## <a name="example-notebook"></a>Notebook mit Beispielen

Im Notebook [00.Getting Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) werden die in diesem Artikel behandelten Konzepte veranschaulicht.  Notebook abrufen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie können Daten auch in Ihren Modellen in Produktionsumgebungen sammeln. Lesen Sie den Artikel [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md). 


## <a name="other-references"></a>Andere Referenzen
* [Azure Monitor für Container](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json)
