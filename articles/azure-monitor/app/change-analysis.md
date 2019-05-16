---
title: Azure Monitor-Anwendungsänderungsanalyse – Verwenden der Azure Monitor-Anwendungsänderungsanalyse zum Ermitteln von Änderungen, die zu Problemen/Ausfällen der Livewebsite führen können | Microsoft-Dokumentation
description: Behandeln von Anwendungsproblemen der Livewebsite in Azure App Services mit der Azure Monitor-Anwendungsänderungsanalyse
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: b04bd57c66b52e9a2c14d43c9e89d7c54fc48ae2
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415839"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Azure Monitor-Anwendungsänderungsanalyse (öffentliche Vorschau)

Wenn es zu einem Problem/Ausfall der Livewebsite kommt, ist es wichtig, die Grundursache schnell bestimmen zu können. Mithilfe von Standardüberwachungslösungen können Sie schnell erkennen, dass ein Problem vorliegt, und häufig auch die fehlerhafte Komponente identifizieren. Dies führt aber nicht immer zu einer sofortigen Erklärung, warum der Fehler auftritt. Vor fünf Minuten hat Ihre Website noch funktioniert, jetzt ist sie fehlerhaft. Was hat sich in den letzten fünf Minuten geändert? Für die Beantwortung dieser Frage ist das neue Feature, die Azure Monitor-Anwendungsänderungsanalyse, konzipiert. Basierend auf dem Funktionsumfang von [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) liefert die Anwendungsänderungsanalyse Erkenntnisse über Azure App Service-Anwendungsänderungen, um die Transparenz zu erhöhen und die durchschnittliche Reparaturzeit (Mean Time To Repair, MTTR) zu verkürzen.

> [!IMPORTANT]
> Die Azure Monitor-Anwendungsänderungsanalyse befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-do-i-use-application-change-analysis"></a>Wie verwende ich die Anwendungsänderungsanalyse?

Die Azure Monitor-Anwendungsänderungsanalyse ist derzeit in die Oberfläche für Self-Service-**Diagnose und Problembehandlung** integriert, auf die Sie in Ihrer Azure App Service-Anwendung über den Abschnitt **Übersicht** zugreifen können:

![Screenshot der Azure App Service-Übersichtsseite mit roter Umrandung der Schaltflächen „Übersicht“ und „Diagnose und Problembehandlung“](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>Aktivieren der Änderungsanalyse

1. Wählen Sie **Verfügbarkeit und Leistung** aus.

    ![Screenshot der Problembehandlungsoptionen von „Verfügbarkeit und Leistung“](./media/change-analysis/availability-and-performance.png)

2. Klicken Sie auf die Kachel **Anwendungsabstürze**.

   ![Screenshot, auf dem die Kachel „Anwendungsabstürze“ hervorgehoben ist](./media/change-analysis/application-crashes-tile.png)

3. Wählen Sie zum Aktivieren der **Änderungsanalyse** die Option **Jetzt aktivieren** aus.

   ![Screenshot der Problembehandlungsoptionen von „Verfügbarkeit und Leistung“](./media/change-analysis/application-crashes.png)

4. Um alle Funktionen der Änderungsanalyse zu nutzen, legen Sie **Änderungsanalyse**, **Nach Codeänderungen suchen** und **Immer aktiviert** auf **Ein** fest, und wählen Sie dann **Speichern** aus.

    ![Screenshot der Aktivierungsoptionen auf der Benutzeroberfläche der Azure App Service-Änderungsanalyse](./media/change-analysis/change-analysis-on.png)

    Wenn die **Änderungsanalyse** aktiviert ist, können Sie Änderungen auf Ressourcenebene erkennen. Wenn **Nach Codeänderungen suchen** aktiviert ist, werden auch Änderungen an Bereitstellungsdateien und der Websitekonfiguration angezeigt. Durch Aktivieren von **Immer aktiviert** wird die Änderungsüberprüfungsleistung optimiert, aber abrechnungstechnisch können dadurch zusätzliche Kosten anfallen.

5.  Nachdem Sie alle Optionen aktiviert haben, können Sie durch Auswählen von **Diagnose und Problembehandlung** > **Verfügbarkeit und Leistung** > **Anwendungsabstürze** auf die Oberfläche der Änderungsanalyse zugreifen. Im Diagramm werden die Arten von Änderungen, die im Laufe der Zeit erfolgt sind, zusammen mit Details zu diesen Änderungen zusammengefasst:

     ![Screenshot der Gegenüberstellung der Änderungen](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>Problembehandlung

### <a name="unable-to-fetch-change-analysis-information"></a>Die Informationen der Änderungsanalyse können nicht abgerufen werden.

Dabei handelt es sich um ein vorübergehendes Problem mit der aktuellen Vorschauoberfläche für das Onboarding. Die Problemumgehung besteht darin, ein ausgeblendetes Tag für Ihre Web-App festzulegen und dann die Seite zu aktualisieren:

   ![Screenshot der Änderung des ausgeblendeten Tags](./media/change-analysis/hidden-tag.png)

Führen Sie die folgenden Schritte aus, um das ausgeblendete Tag mithilfe von PowerShell festzulegen:

1. Öffnen Sie Azure Cloud Shell:

    ![Screenshot der Azure Cloud Shell-Änderung](./media/change-analysis/cloud-shell.png)

2. Ändern Sie den Shelltyp in „PowerShell“:

   ![Screenshot der Azure Cloud Shell-Änderung](./media/change-analysis/choose-powershell.png)

3. Führen Sie den folgenden Befehl aus:

```powershell
$webapp=Get-AzWebApp -Name <name_of_your_webapp>
$tags = $webapp.Tags
$tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
```

> [!NOTE]
> Nachdem das ausgeblendete Tag hinzugefügt wurde, müssen Sie möglicherweise noch bis zu vier Stunden warten, bis die Änderungen angezeigt werden. Der Grund dafür ist das Häufigkeitsintervall von 4 Stunden, das der Änderungsanalysedienst verwendet, um Ihre Web-App zu überprüfen und gleichzeitig die Auswirkungen der Überprüfung auf die Leistung zu beschränken.

## <a name="next-steps"></a>Nächste Schritte

- Verbessern Sie die Überwachung von Azure App Services [durch Aktivieren der Application Insights-Features](azure-web-apps.md) von Azure Monitor.
- Erweitern Sie Ihre Kenntnisse über den [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview)-Dienst, der die Azure Monitor-Anwendungsänderungsanalyse unterstützt. 
