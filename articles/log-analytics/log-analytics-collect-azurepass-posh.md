---
title: Sammeln von Azure PaaS-Ressourcenmetriken mit Log Analytics | Microsoft-Dokumentation
description: "Erhalten Sie Informationen zum Aktivieren der Sammlung von Azure PaaS-Ressourcenmetriken mithilfe von PowerShell für die Aufbewahrung und Analyse in Log Analytics."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/13/2017
ms.author: magoedte
ms.openlocfilehash: 7bae18e151fbdccf95f3fe5f569041d6dd9c42eb
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Konfigurieren der Sammlung von Azure PaaS-Ressourcenmetriken mit Log Analytics

Azure PaaS-Ressourcen (Platform as a Service), z. B. Azure SQL und Websites (Web-Apps), können Leistungsmetrikdaten nativ für Log Analytics ausgeben. Dieses Skript ermöglicht Benutzern das Aktivieren der Protokollierung von Metriken für PaaS-Ressourcen, die bereits in einer bestimmten Ressourcengruppe oder in einem gesamten Abonnement bereitgestellt wurden. 

Derzeit besteht keine Möglichkeit, die Protokollierung von Metriken für PaaS-Ressourcen über das Azure-Portal zu aktivieren. Aus diesem Grund müssen Sie ein PowerShell-Skript verwenden. Mit dieser nativen Protokollierungsfunktion für Metriken und der Überwachung von Log Analytics können Sie Azure-Ressourcen im gewünschten Umfang überwachen. 

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Azure Resource Manager-Module auf Ihrem Computer installiert sind, bevor Sie fortfahren:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>Es wird empfohlen, dass all Ihre Azure Resource Manager-Module dieselbe Version aufweisen, um die Kompatibilität sicherzustellen, wenn Sie Azure Resource Manager-Befehle von PowerShell ausführen.
>
Informationen zum Installieren der neuesten Version der Azure Resource Manager-Module auf Ihrem Computer finden Sie unter [Azure PowerShell installieren und konfigurieren](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.1#update-azps).  

## <a name="enable-azure-diagnostics"></a>Aktivieren der Azure-Diagnose  
Das Konfigurieren der Azure-Diagnose für PaaS-Ressourcen wird durch Ausführen des Skripts **Enable-AzureRMDiagnostics.ps1** erreicht, das über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52/DisplayScript) verfügbar ist.  Das Skript unterstützt die folgenden Szenarien:
  
* Angeben einer Ressource, die sich auf eine oder mehrere Ressourcengruppen in einem Abonnement bezieht  
* Angeben einer Ressource, die sich auf eine bestimmte Ressourcengruppe in einem Abonnement bezieht  
* Konfigurieren einer Ressource für die Weiterleitung zu einem anderen Arbeitsbereich

Es werden nur Ressourcen unterstützt, die das Sammeln von Metriken mithilfe der Azure-Diagnose und das direkte Senden an Log Analytics unterstützen.  Eine ausführliche Liste finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](log-analytics-azure-storage.md). 

Führen Sie die folgenden Schritte durch, um das Skript herunterzuladen und auszuführen.

1.  Geben Sie auf dem Windows-Startbildschirm **PowerShell** ein und klicken Sie in den Suchergebnissen mit der rechten Maustaste auf PowerShell.  Wählen Sie aus dem Menü **Als Administrator ausführen** aus.   
2. Speichern Sie die Skriptdatei **Enable-AzureRMDiagnostics.ps1** lokal, indem Sie den folgenden Befehl ausführen und einen Pfad zum Speichern des Skripts bereitstellen.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Führen Sie zum Starten `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.   
4. Führen Sie das folgende Skript `.\Enable-AzureRmDiagnostics.ps1` ohne Parameter aus, um die Datensammlung von einer bestimmten Ressource in Ihrem Abonnement zu aktivieren, oder mit dem Parameter `-ResourceGroup <myResourceGroup>`, um eine Ressource in einer bestimmten Ressourcengruppe anzugeben.   
5. Wählen Sie das entsprechende Abonnement aus der Liste aus, wenn Sie über mehrere verfügen, indem Sie den richtigen Wert eingeben.<br><br> ![Auswählen des vom Skript zurückgegebenen Abonnements](./media/log-analytics-collect-azurepass-posh/script-select-subscription.png)<br> Andernfalls wird automatisch das einzige verfügbare Abonnement ausgewählt.
6. Als Nächstes gibt das Skript eine Liste der Log Analytics-Arbeitsbereiche zurück, die im Abonnement registriert sind.  Wählen Sie in der Liste den entsprechenden Arbeitsbereich aus.<br><br> ![Auswählen des vom Skript zurückgegebenen Arbeitsbereichs](./media/log-analytics-collect-azurepass-posh/script-select-workspace.png)<br> 
7. Wählen Sie die Azure-Ressource aus, für die Sie die Sammlung aktivieren möchten. Wenn Sie z. B. „5“ eingeben, können Sie die Datensammlung für SQL Azure-Datenbanken aktivieren.<br><br> ![Auswählen des vom Skript zurückgegebenen Ressourcentyps](./media/log-analytics-collect-azurepass-posh/script-select-resource.png)<br>
   Es können nur Ressourcen ausgewählt werden, die das Sammeln von Metriken mithilfe der Azure-Diagnose und das direkte Senden an Log Analytics unterstützen.  Das Skript zeigt den Wert **True** unter der Spalte **Metriken** für die Liste der Ressourcen an, die es in Ihrem Abonnement oder in der angegebenen Ressourcengruppe ermittelt.    
8. Sie werden aufgefordert, Ihre Auswahl zu bestätigen.  Geben Sie **J** ein, um die Protokollierung von Metriken für alle ausgewählten Ressourcen für den definierten Bereich zu aktivieren, bei dem es sich in diesem Beispiel um alle SQL-Datenbanken im Abonnement handelt.  

Das Skript wird für alle Ressourcen ausgeführt, die den ausgewählten Kriterien entspricht, und es aktiviert das Sammeln von Metriken für diese Ressourcen. Nachdem es abgeschlossen ist, wird eine Nachricht angezeigt, dass die Konfiguration abgeschlossen ist.  

Kurz nach Abschluss des Vorgangs werden Daten aus der Azure PaaS-Ressource im Log Analytics-Repository angezeigt.  Ein Datensatz mit dem Typ `AzureMetrics` wird erstellt und das Analysieren dieser Datensätze wird von den Verwaltungslösungen [Azure SQL-Analyse](log-analytics-azure-sql.md) und [Azure Web-Apps-Analyse](log-analytics-azure-web-apps-analytics.md) unterstützt.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Aktualisieren einer Ressource zum Senden von Daten an einen anderen Arbeitsbereich
Wenn Sie über eine Ressource verfügen, die bereits Daten an einen Log Analytics-Arbeitsbereich sendet, und Sie sich später entscheiden, sie so neu zu konfigurieren, dass sie auf einen anderen Arbeitsbereich verweist, können Sie das Skript mit dem Parameter `-Update` ausführen.  

**Beispiel:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

Sie werden aufgefordert, dieselben Informationen wie beim Ausführen des Skripts zum Durchführen der Erstkonfiguration zu beantworten.  

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 

* Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md), um die Ereignisdatensätze in einzelnen Feldern zu analysieren.

* Lesen Sie [Erstellen eines benutzerdefinierten Dashboards für die Verwendung in Log Analytics](log-analytics-dashboards.md), um zu verstehen, wie Sie Ihre Protokollsuchen für die Organisation auf sinnvolle Weise darstellen.