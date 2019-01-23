---
title: Versionsanmerkungen für Application Insights | Microsoft Docs
description: Fügen Sie den Diagrammen im Metrik-Explorer in Application Insights Bereitstellungs- oder Buildmarker hinzu.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mbullwin
ms.openlocfilehash: 1ce937f5c9a378494e66867b6f919ee10eef2bf8
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259477"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anmerkungen zu Metrik-Diagrammen in Application Insights

Die Anmerkungen in den Diagrammen des [Metrik-Explorers](../../azure-monitor/app/metrics-explorer.md) geben an, wo Sie einen neuen Build oder ein anderes wichtiges Ereignis bereitgestellt haben. Dank dieser Anmerkungen sehen Sie auf einen Blick, ob Ihre Änderungen Auswirkungen auf die Leistung Ihrer Anwendung hatten. Sie können automatisch vom [Buildsystem von Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/) erstellt werden. Es können auch Anmerkungen erstellt werden, die alle gewünschten Ereignisse markieren, indem diese [aus PowerShell erstellt werden](#create-annotations-from-powershell).

> [!NOTE]
> In diesem Artikel wird die veraltete **Umgebung für klassische Metriken** verwendet. Anmerkungen sind derzeit nur in der klassischen Umgebung und in **[Arbeitsmappen](../../azure-monitor/app/usage-workbooks.md)** verfügbar. Weitere Informationen zur aktuellen Metrikumgebung finden Sie in [diesem Artikel](../../azure-monitor/platform/metrics-charts.md).

![Beispiel für Anmerkungen mit sichtbarer Korrelation mit Serverantwortzeit](./media/annotations/00.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Versionsanmerkungen mit Azure DevOps-Services-Build

Versionsanmerkungen sind ein Feature des cloudbasierten Azure Pipelines-Diensts von Azure DevOps Services. 

### <a name="install-the-annotations-extension-one-time"></a>Installieren der Erweiterung für Anmerkungen (einmalig)
Um Versionsanmerkungen erstellen zu können, müssen Sie eine der zahlreichen Azure DevOps Services-Erweiterungen installieren, die im Visual Studio Marketplace zur Verfügung stehen.

1. Melden Sie sich bei Ihrem [Azure DevOps Services](https://visualstudio.microsoft.com/vso/)-Projekt an.
2. Laden Sie im Visual Studio Marketplace die [Erweiterung Release Annotations](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) herunter, und fügen Sie sie Ihrer Azure DevOps Services-Organisation hinzu.

![Öffnen Sie rechts oben auf der Azure DevOps Services-Website den Marketplace. Wählen Sie „Azure DevOps Services“ und dann unter „Azure Pipelines“ die Option „Mehr anzeigen“ aus.](./media/annotations/10.png)

Sie müssen dies nur einmal für Ihre Azure DevOps Services-Organisation durchführen. Versionsanmerkungen können nun für jedes Projekt in Ihrer Organisation konfiguriert werden. 

### <a name="configure-release-annotations"></a>Konfigurieren von Versionsanmerkungen

Es wird ein separater API-Schlüssel für jede Azure DevOps Services-Releasevorlage benötigt.

1. Melden Sie sich im [Microsoft Azure-Portal](https://portal.azure.com) an, und öffnen Sie die Application Insights-Ressource, mit der Sie Ihre Anwendung überwachen. (Oder [erstellen Sie jetzt eine Ressource](../../azure-monitor/app/app-insights-overview.md), sofern noch nicht geschehen.)
2. Öffnen Sie **API-Zugriff**, **Application Insights-ID**.
   
    ![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Einstellungen“. Öffnen Sie „API-Zugriff“. Kopieren der Anwendungs-ID](./media/annotations/20.png)

4. Öffnen (oder erstellen) Sie in einem separaten Browserfenster die Releasevorlage, mit der Ihre Bereitstellungen über Azure DevOps Services verwaltet werden. 
   
    Fügen Sie eine Aufgabe hinzu, und wählen Sie im Menü die Aufgabe für Application Insights-Versionsanmerkungen aus.
   
    Fügen Sie die **Anwendungs-ID** ein, die Sie vom Blatt „API-Zugriff“ kopiert haben.
   
    ![Öffnen Sie in Azure DevOps Services „Release“, wählen Sie eine Releasepipeline aus, und wählen Sie dann „Bearbeiten“ aus. Klicken Sie auf „Aufgabe hinzufügen“, und wählen Sie die Option „Application Insights-Versionsanmerkung“. Fügen Sie die Application Insights-ID ein.](./media/annotations/30.png)
4. Legen Sie das Feld **APIKey** auf eine Variable (`$(ApiKey)`) fest.

5. Wechseln Sie zurück ins Azure-Fenster, erstellen Sie einen neuen API-Schlüssel, und erstellen Sie eine Kopie davon.
   
    ![Klicken Sie im Azure-Fenster auf dem Blatt „API-Zugriff“ auf „API-Schlüssel erstellen“. Geben Sie einen Kommentar an, aktivieren Sie das Schreiben von Anmerkungen, und klicken Sie auf „Schlüssel generieren“. Kopieren Sie den neuen Schlüssel.](./media/annotations/40.png)

6. Öffnen Sie die Registerkarte „Konfiguration“ der Versionsvorlage.
   
    Erstellen Sie eine Variablendefinition für `ApiKey`.
   
    Fügen Sie Ihren API-Schlüssel in die ApiKey-Variablendefinition ein.
   
    ![Wählen Sie im Azure DevOps Services-Fenster die Registerkarte „Konfiguration“ aus, und klicken Sie auf „Variable hinzufügen“. Legen Sie den Namen auf „ApiKey“ fest, und fügen Sie unter „Wert“ den gerade generierten Schlüssel ein. Klicken Sie anschließend auf das Schlosssymbol.](./media/annotations/50.png)
7. **Speichern** Sie abschließend die Releasepipeline.


## <a name="view-annotations"></a>Anmerkungen anzeigen
Wenn Sie nun diese Versionsvorlage zum Bereitstellen einer neuen Version verwenden, wird jedes Mal eine Anmerkung an Application Insights gesendet. Die Anmerkungen werden in Diagrammen im Metrik-Explorer angezeigt.

Klicken Sie auf einen Anmerkungsmarker, um Details zur Release, einschließlich Anfordernder, Quellcodeverwaltungsbranch, Releasepipeline, Umgebung usw., anzuzeigen.

![Klicken Sie auf einen Versionsanmerkungsmarker.](./media/annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Erstellen von benutzerdefinierten Anmerkungen in PowerShell
Sie können Anmerkungen auch von jedem beliebigen Prozess erstellen (ohne Azure DevOps Services zu verwenden). 


1. Erstellen Sie eine lokale Kopie des [PowerShell-Skript aus GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Rufen Sie die Anwendungs-ID ab und erstellen Sie auf dem Blatt „API-Zugriff“ einen API-Schlüssel .

3. Nennen Sie das Skript folgendermaßen:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Änderungen am Skript können einfach vorgenommen werden, um beispielsweise frühere Anmerkungen zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Arbeitselements](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automation mit PowerShell](../../azure-monitor/app/powershell.md)