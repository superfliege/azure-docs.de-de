---
title: Aktivieren der Diagnoseprotokollierung in Azure Traffic Manager
description: Es wird beschrieben, wie Sie die Diagnoseprotokollierung für Ihr Traffic Manager-Profil aktivieren und auf die Protokolldateien zugreifen, die sich bei dem Vorgang ergeben.
services: traffic-manager
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: kumud
ms.openlocfilehash: abdc50d6d3d27ab7611994089345a997afc72cae
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082611"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Aktivieren der Diagnoseprotokollierung in Azure Traffic Manager

In diesem Artikel wird beschrieben, wie Sie die Diagnoseprotokollierung aktivieren und auf die Protokolldaten für ein Traffic Manager-Profil zugreifen.

Azure Traffic Manager-Diagnoseprotokolle können Erkenntnisse zum Verhalten der Traffic Manager-Profilressource liefern. Beispielsweise können Sie die Protokolldaten des Profils verwenden, um zu ermitteln, warum für einzelne Tests eines Endpunkts ein Timeout aufgetreten ist.

## <a name="enable-diagnostic-logging"></a>Aktivieren der Diagnoseprotokollierung

Sie können die nachfolgenden Befehle in [Azure Cloud Shell](https://shell.azure.com/powershell) oder über PowerShell auf Ihrem Computer ausführen. Azure Cloud Shell ist eine kostenlose interaktive Shell. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie PowerShell auf Ihrem Computer ausführen, müssen Sie mindestens Version 6.13.1 des PowerShell-Moduls *AzureRM* ausführen. Sie können `Get-Module -ListAvailable AzureRM` ausführen, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um sich bei Azure anzumelden.

1. **Rufen Sie das Traffic Manager-Profil ab:**

    Zum Aktivieren der Diagnoseprotokollierung benötigen Sie die ID eines Traffic Manager-Profils. Rufen Sie das Traffic Manager-Profil ab, für das Sie die Diagnoseprotokollierung aktivieren möchten, indem Sie [Get-AzureRmTrafficManagerProfile](/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile) verwenden. Die Ausgabe enthält die Informationen zur ID des Traffic Manager-Profils.

    ```azurepowershell-interactive
    Get-AzureRmTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Aktivieren der Diagnoseprotokollierung für das Traffic Manager-Profil:**

    Aktivieren Sie mit [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/set-azurermdiagnosticsetting?view=latest) die Diagnoseprotokollierung für das Traffic Manager-Profil, indem Sie die ID verwenden, die Sie im vorherigen Schritt abgerufen haben. Mit dem folgenden Befehl werden ausführliche Protokolle für das Traffic Manager-Profil in einem angegebenen Azure Storage-Konto gespeichert. 

      ```azurepowershell-interactive
    Set-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Überprüfen von Diagnoseeinstellungen:**

      Überprüfen Sie die Diagnoseeinstellungen für das Traffic Manager-Profil, indem Sie [Get-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermdiagnosticsetting?view=latest) verwenden. Mit dem folgenden Befehl werden die Kategorien angezeigt, die für eine Ressource protokolliert werden.

     ```azurepowershell-interactive
     Get-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Stellen Sie sicher, dass für alle Protokollkategorien, die der Traffic Manager-Profilressource zugeordnet sind, „Aktiviert“ angezeigt wird. Stellen Sie außerdem sicher, dass das Speicherkonto richtig festgelegt wurde.

## <a name="access-log-files"></a>Zugreifen auf Protokolldateien
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie im Portal zu Ihrem Azure Storage-Konto.
2. Wählen Sie auf der Seite **Übersicht** Ihres Azure-Speicherkontos unter **Dienste** die Option **Blobs**.
3. Wählen Sie unter **Container** die Option **insights-logs-probehealthstatusevents**, und navigieren Sie nach unten zur Datei „PT1H.json“. Klicken Sie anschließend auf **Herunterladen**, um eine Kopie dieser Protokolldatei herunterzuladen und zu speichern.

    ![Zugreifen auf Protokolldateien Ihres Traffic Manager-Profils über einen Blobspeicher](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Traffic Manager-Protokollschema

Für alle Diagnoseprotokolle, die über Azure Monitor verfügbar sind, wird ein Schema der obersten Ebene gemeinsam genutzt. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben. Informationen zum Diagnoseprotokollschema der obersten Ebene finden Sie unter [Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle](../azure-monitor/platform/tutorial-dashboards.md).

Die folgende Tabelle enthält das Protokollschema für die Azure Traffic Manager-Profilressource.

|||||
|----|----|---|---|
|**Feldname**|**Feldtyp**|**Definition**|**Beispiel**|
|EndpointName|Zeichenfolge|Der Name des Traffic Manager-Endpunkts, dessen Integritätsstatus aufgezeichnet wird.|*myPrimaryEndpoint*|
|Status|Zeichenfolge|Der Integritätsstatus des Traffic Manager-Endpunkts, der getestet wurde. Der Status kann entweder **Up** oder **Down** lauten.|**Up**|
|||||

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr zur [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).

