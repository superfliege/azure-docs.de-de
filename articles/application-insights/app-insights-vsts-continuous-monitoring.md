---
title: "Kontinuierliche Überwachung der DevOps-Releasepipeline mit VSTS und Azure Application Insights | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anweisungen zum schnellen Einrichten der kontinuierlichen Überwachung mit Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6ced5941d06c02a2576bf7f561bd535810e7ffd9
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Hinzufügen der kontinuierlichen Überwachung zur Releasepipeline

Visual Studio Team Services (VSTS) kann in Azure Application Insights integriert werden, um die kontinuierliche Überwachung Ihrer DevOps-Releasepipeline während des gesamten Lebenszyklus der Softwareentwicklung zu ermöglichen. 

VSTS unterstützt jetzt die kontinuierliche Überwachung, sodass Releasepipelines Überwachungsdaten von Application Insights und anderen Azure-Ressourcen integrieren können. Wenn eine Application Insights-Warnung erkannt wird, kann die Bereitstellung abgegrenzt bleiben oder zurückgesetzt werden, bis die Warnung aufgelöst wurde. Wenn alle Überprüfungen erfolgreich durchgeführt wurden, können Bereitstellungen vom Testen bis hin zur Produktion ohne manuellen Eingriff automatisch fortgesetzt werden. 

## <a name="configure-continuous-monitoring"></a>Konfigurieren der kontinuierlichen Überwachung

1. Wählen Sie ein vorhandenes VSTS-Projekt aus.

2. Zeigen Sie auf **Build und Release**. > Wählen Sie **Releases** aus. > Klicken Sie auf das **Pluszeichen** > **Releasedefinition erstellen**. > Suchen Sie nach **Überwachung** > **Azure App Service Deployment with Continuous Monitoring** (Azure App Service-Bereitstellung mit kontinuierlicher Überwachung).

   ![Neue VSTS-Releasedefinition](.\media\app-insights-continuous-monitoring\001.png)

3. Klicken Sie auf **Übernehmen**.

4. Wählen Sie neben dem roten Ausrufezeichen den blauen Text aus, um die Option **Umgebungsaufgaben anzeigen** anzuzeigen.

   ![Umgebungsaufgaben anzeigen](.\media\app-insights-continuous-monitoring\002.png)

   Es wird ein Dialogfeld für die Konfiguration geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus.

    | Parameter        | Wert |
   | ------------- |:-----|
   | **Umgebungsname**      | Name, der die Umgebung der Releasedefinition beschreibt. |
   | **Azure-Abonnement** | In dieser Dropdownliste werden alle mit dem VSTS-Konto verknüpften Azure-Abonnements eingetragen.|
   | **App Service-Name** | Abhängig von der sonstigen Auswahl muss möglicherweise für dieses Feld manuell ein neuer Wert eingetragen werden. |
   | **Ressourcengruppe**    | In diese Dropdownliste werden die verfügbaren Ressourcengruppen eingetragen. |
   | **Application Insights-Ressourcenname** | In diese Dropdownliste werden alle Application Insights-Ressourcen eingetragen, die der zuvor ausgewählten Ressourcengruppe entsprechen.

5. Wählen Sie **Configure Application Insights alerts** (Application Insights-Warnungen konfigurieren) aus.

6. Wählen Sie für die Standardwarnungsregeln **Speichern** aus. > Geben Sie einen beschreibenden Kommentar ein. > Klicken Sie auf **OK**.

## <a name="modify-alert-rules"></a>Ändern von Warnungsregeln

1. Um die vordefinierten Warnungseinstellungen zu ändern, klicken Sie auf das Feld mit den Auslassungszeichen **...** rechts neben **Warnungsregeln**.

   (Standardmäßig sind vier Standardregeln vorhanden: Verfügbarkeit, Anforderungsfehler, Serverantwortzeit, Serverausnahmen.)

2. Klicken Sie auf das Dropdownsymbol neben **Verfügbarkeit**.

3. Ändern Sie den **Schwellenwert** für die Verfügbarkeit entsprechend Ihren Servicelevelanforderungen.

   ![Ändern der Warnung](.\media\app-insights-continuous-monitoring\003.png)

4. Wählen Sie **OK** > **Speichern** aus. > Geben Sie einen beschreibenden Kommentar ein. > Klicken Sie auf **OK**.

## <a name="add-deployment-conditions"></a>Hinzufügen von Bereitstellungsbedingungen

1. Klicken Sie auf **Pipeline**. > Wählen Sie abhängig von der Phase, für die ein Gate zur kontinuierlichen Überwachung erforderlich ist, das Symbol für die **Bedingungen vor der Bereitstellung** oder die **Bedingungen nach der Bereitstellung** aus.

   ![Bedingungen vor der Bereitstellung](.\media\app-insights-continuous-monitoring\004.png)

2. Legen Sie **Gates** auf **Aktiviert** > **Approval gates** (Genehmigungsgates) fest. > Klicken Sie auf **Hinzufügen**.

3. Wählen Sie **Azure Monitor** aus. (Über diese Option können Sie auf Warnungen von Azure Monitor sowie von Application Insights zugreifen.)

    ![Azure Monitor](.\media\app-insights-continuous-monitoring\005.png)

4. Geben Sie einen Wert für **Gates Timeout** (Gatetimeout) ein.

5. Geben Sie ein **Samplingintervall** ein.

## <a name="deployment-gate-status-logs"></a>Statusprotokolle für Bereitstellungsgates

Nachdem Sie Bereitstellungsgates hinzugefügt haben, schützt eine Warnung in Application Insights, die den zuvor definierten Schwellenwert überschreitet, die Bereitstellung vor unerwünschter Releasehöherstufung. Nach Auflösung der Warnung kann die Bereitstellung automatisch fortgesetzt werden.

Wählen Sie zur Überwachung dieses Verhaltens die Option **Releases** aus. > Klicken Sie mit der rechten Maustaste auf den Releasenamen, und wählen Sie **Öffnen** > **Protokolle** aus.

![Protokolle](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum VSTS-Build und VSTS-Release finden Sie in diesen [Schnellstarts](https://docs.microsoft.com/en-us/vsts/build-release/).
