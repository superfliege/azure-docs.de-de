---
title: Migration von Compute-Ressourcen von Azure Deutschland zu Azure weltweit
description: Stellt Hilfe für die Migration von Compute-Ressourcen zur Verfügung.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: e651d83fd3409c1e86eb4448ff8a53c563171177
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284345"
---
# <a name="migration-of-compute-resources-from-azure-germany-to-global-azure"></a>Migration von Compute-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Compute-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="compute-iaas"></a>Compute IaaS

Leider ist keine direkte Migration von Azure Deutschland zu Azure weltweit möglich. Es gibt jedoch mehrere Möglichkeiten, Ihre virtuellen Computer zu „duplizieren“.

### <a name="duplicate-with-azure-site-recovery"></a>Duplizieren mit Azure Site Recovery

Azure Site Recovery hilft Ihnen bei der Migration Ihrer virtuellen Computer von Azure Deutschland zu Azure weltweit. Da sich Quelle und Ziel in unterschiedlichen Mandanten befinden, können Sie die normale Azure-Notfallwiederherstellungsoption für virtuelle Computer nicht verwenden. Der Trick besteht darin, einen Site Recovery-Tresor in der Zielumgebung (Azure weltweit) einzurichten und dann wie beim Verschieben eines physischen Servers in Azure vorzugehen. Wählen Sie einen Replikationspfad von *nicht virtualisiert* zu Azure weltweit aus, und führen Sie (nach Abschluss der Replikation) ein Failover aus.

> [!NOTE]
> Diese Schritte sind die gleichen, die Sie für die Migration eines lokal ausgeführten physischen Servers zu Azure ausführen würden.

Ein [gutes Tutorial zu Site Recovery](../site-recovery/physical-azure-disaster-recovery.md) ist verfügbar. Für einen schnellen Überblick gibt es hier eine kürzere und leicht angepasste Version:

Installieren Sie einen Konfigurations-/Prozessserver in Ihrer Quellumgebung, um die Images der Server zu erstellen. Replizieren Sie die Images dann in den Recovery Services-Tresor in der Zielumgebung. Dies alles wird vom Konfigurationsserver erledigt, und es besteht keine Notwendigkeit, sich mit den einzelnen Servern zu beschäftigen.

- Melden Sie sich am Azure Deutschland-Portal an.
- Vergleichen Sie die Betriebssystemversion der zu migrierenden virtuellen Computer mit der [Supportmatrix](../site-recovery/vmware-physical-secondary-support-matrix.md).
- Richten Sie einen neuen virtuellen Computer in Ihrem Quell-VNET ein, der als Konfigurationsserver fungiert:
  - Wählen Sie DS4v3 oder höher (4 bis 8 Kerne, 16 GB Arbeitsspeicher) aus.
  - Fügen Sie einen zusätzlichen Datenträger mit mindestens 1 TB verfügbarem Speicherplatz (für die VM-Images) an.
  - Verwenden Sie Windows Server 2012R2 oder höher.
- Stellen Sie sicher, dass die Ports 443 und 9443 für das Subnetz in beide Richtungen geöffnet sind.
- Melden Sie sich bei diesem neuen virtuellen Computer (Konfigurationsserver) an.
- Melden Sie sich aus Ihrer Remotedesktopsitzung am Portal für Azure weltweit mit Ihren Anmeldeinformationen für Azure weltweit an.
- Richten Sie ein VNET ein, in dem die replizierten virtuellen Computer ausgeführt werden.
- Speicherkonto erstellen
- Richten Sie den Recovery Services-Tresor ein.
- Definieren Sie das Schutzziel (**In Azure** -- **Nicht virtualisiert/Andere**).
- Laden Sie die Installationsdatei für das einheitliche Site Recovery-Setup (**Infrastruktur vorbereiten** > **Quelle**) herunter. Wenn Sie die Portal-URL über den Konfigurationsserver geöffnet haben, wird die Datei auf den richtigen Server heruntergeladen. Wenn dies nicht der Fall ist, laden Sie die Installationsdatei auf den Konfigurationsserver hoch.
- Laden Sie den Tresorregistrierungsschlüssel herunter (und auf den Konfigurationsserver wie oben beschrieben hoch, wenn erforderlich).
- Führen Sie die Installation für das einheitliche Site Recovery-Setup auf dem Konfigurationsserver aus.
- Richten Sie die Zielumgebung ein (Sie sollten noch immer am Zielportal angemeldet sein).
- Definieren Sie die Replikationsrichtlinie.
- Starten Sie die Replikation.

Wenn die Replikation erstmalig erfolgreich war, sollten Sie das Szenario testen, indem Sie ein Testfailover einschließlich einer Überprüfung ausführen und den Test dann löschen. Im letzten Schritt führen Sie das tatsächliche Failover aus.

> [!CAUTION]
> Es erfolgt keine Synchronisierung zurück an den virtuellen Quellcomputer. Wenn Sie die Migration erneut ausführen möchten, müssen Sie alles bereinigen und ganz von vorn beginnen!

### <a name="duplicate-with-resource-manager-template-exportimport"></a>Duplizieren mit Resource Manager-Vorlagenexport/-import

Sie können die Resource Manager-Vorlage exportieren, die für die Bereitstellung auf Ihrem lokalen Computer verwendet wird. Bearbeiten Sie die Vorlage, um den Speicherort und andere Parameter oder Variablen zu ändern, und führen Sie dann eine erneute Bereitstellung in Azure weltweit aus. 

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

Exportieren Sie die Resource Manager-Vorlage im Portal, indem Sie die Ressourcengruppe auswählen. Klicken Sie auf *Bereitstellungen*, und wählen Sie die neueste Bereitstellung aus. Klicken Sie im linken Menü auf *Vorlage*, und laden Sie diese herunter.

Sie erhalten eine ZIP-Datei, die mehrere Dateien enthält. PowerShell, die CLI, Ruby- und .NET-Skripts unterstützen Sie beim Bereitstellen Ihrer Vorlage. Die Datei *parameters.json* verfügt über alle Eingaben aus der letzten Bereitstellung. Wahrscheinlich müssen Sie hier einige Einstellungen ändern. Bearbeiten Sie die Datei *template.json*, wenn Sie nur eine Teilmenge der Ressourcen erneut bereitstellen möchten.


### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Azure Site Recovery anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/site-recovery/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="reference"></a>Verweis

- [Physische Computer nach Azure mit Site Recovery](../site-recovery/physical-azure-disaster-recovery.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)
- [Azure-Standorte: Übersicht](https://azure.microsoft.com/global-infrastructure/locations/)
- [Erneutes Bereitstellen einer Vorlage](../azure-resource-manager/resource-group-template-deploy.md)







## <a name="cloud-services"></a>Cloud Services

Cloud Services können erneut bereitgestellt werden, indem die `.cspkg`- und `.cscfg`-Definitionen erneut angegeben werden.

### <a name="by-portal"></a>Über das Portal

- [Erstellen Sie einen neuen Clouddienst](../cloud-services/cloud-services-how-to-create-deploy-portal.md) mit Ihren `.cspkg`- und `.cscfg`-Dateien.
- Aktualisieren Sie den [CNAME- oder A-Eintrag](../cloud-services/cloud-services-custom-domain-name-portal.md) zur Umleitung des Datenverkehrs an den neuen Clouddienst.
- Nachdem der Datenverkehr auf den neuen Clouddienst verweist, löschen Sie den alten Clouddienst in Azure Deutschland.

### <a name="by-powershell"></a>Über PowerShell

- [Erstellen Sie einen neuen Clouddienst](/powershell/module/servicemanagement/azure/new-azureservice) mit Ihren `.cspkg`- und `.cscfg`-Dateien.

```powershell
New-AzureService -ServiceName <yourServiceName> -Label <MyTestService> -Location <westeurope>
```

- [Erstellen Sie eine neue Bereitstellung](/powershell/module/servicemanagement/azure/new-azuredeployment) mit Ihren `.cspkg`- und `.cscfg`-Dateien. Ausführlichere Informationen finden Sie [hier].

```powershell
New-AzureDeployment -ServiceName <yourServiceName> -Slot <Production> -Package <YourCspkgFile.cspkg> -Configuration <YourConfigFile.cscfg>
```

- Aktualisieren Sie den [CNAME- oder A-Eintrag](../cloud-services/cloud-services-custom-domain-name-portal.md) zur Umleitung des Datenverkehrs an den neuen Clouddienst.
- Nachdem der Datenverkehr auf den neuen Clouddienst verweist, [löschen Sie den alten Clouddienst](/powershell/module/servicemanagement/azure/remove-azureservice) in Azure Deutschland.

```powershell
Remove-AzureService -ServiceName <yourOldServiceName>
```

### <a name="by-rest-api"></a>Über die REST-API

- [Erstellen Sie einen neuen Clouddienst](/rest/api/compute/cloudservices/rest-create-cloud-service) in der Zielumgebung.

```http
https://management.core.windows.net/<subscription-id>/services/hostedservices
```

- Erstellen Sie eine neue Bereitstellung mithilfe der [API zum Erstellen einer Bereitstellung](https://msdn.microsoft.com/library/azure/ee460813.aspx). Wenn Sie Ihre `.cspkg`- und `.cscfg`-Dateien ermitteln müssen, können Sie die [Get-Package-API](https://msdn.microsoft.com/library/azure/jj154121.aspx) aufrufen.

```http
https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deploymentslots/production
```

- Nachdem der Datenverkehr auf den neuen Clouddienst verweist, [löschen Sie den alten Clouddienst](https://docs.microsoft.com/rest/api/compute/cloudservices/rest-delete-cloud-service) in Azure Deutschland.

```http
https://management.core.cloudapi.de/<subscription-id>/services/hostedservices/<old-cloudservice-name>
```

### <a name="references"></a>Referenzen
- [Cloud Services: Übersicht](../cloud-services/cloud-services-choose-me.md)











## <a name="service-fabric"></a>Service Fabric

Es ist nicht möglich Service Fabric-Ressourcen von Azure Deutschland zu Azure weltweit zu migrieren. Sie müssen in der neuen Umgebung eine erneute Bereitstellung ausführen.

Sie können einige Informationen zu Ihrer aktuellen Service Fabric-Umgebung erfassen, indem Sie PowerShell-Cmdlets verwenden. Sie rufen alle Cmdlets für Service Fabric ab, indem Sie `Get-Help *ServiceFabric*` in PowerShell eingeben.

 
### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Service Fabric anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/service-fabric/#step-by-step-tutorials) auf.
- [Erstellen eines neuen Clusters](../service-fabric/service-fabric-cluster-creation-via-portal.md)

### <a name="references"></a>Referenzen

- [Service Fabric: Übersicht](../service-fabric/service-fabric-overview.md)












## <a name="batch"></a>Batch

Es ist nicht möglich, Batch-Kontodaten aus einer Region in eine andere zu migrieren. Dem Konto können aktuell ausgeführte virtuelle Computer zugeordnet sein, die mit Daten in Speicherkonten, Datenbanken oder anderen Speichersystemen interagieren.

Stellen Sie Ihre Bereitstellungsskripts, Vorlagen oder Ihren Code in der neuen Region erneut bereit, einschließlich der folgenden Elemente:

- [Erstellen eines Batch-Kontos](../batch/batch-account-create-portal.md)
- [Erhöhen des Batch-Kontokontingents](../batch/batch-quota-limit.md)
- Erstellen von Batch-Pools
- Erstellen Sie neue Speicherkonten, Datenbanken oder andere Dienste zum persistenten Speichern von Eingabe- und Ausgabedaten.
- Aktualisieren Sie die Konfiguration und Ihren Code, um auf das neue Batch-Konto zu verweisen, und verwenden Sie neue Anmeldeinformationen.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Azure Batch anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/batch/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Azure Batch: Übersicht](../batch/batch-technical-overview.md)










## <a name="functions"></a>Functions

Die Migration von Functions zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Die empfohlene Vorgehensweise besteht darin, die Resource Manager-Vorlage zu exportieren, den Speicherort zu ändern und eine erneute Bereitstellung in der Zielregion vorzunehmen.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Functions anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/azure-functions/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [Übersicht zu Azure Functions](../azure-functions/functions-overview.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)
- [Azure-Standorte: Übersicht](https://azure.microsoft.com/global-infrastructure/locations/)
- [Erneutes Bereitstellen einer Vorlage](../azure-resource-manager/resource-group-template-deploy.md)











## <a name="virtual-machines-scale-set"></a>VM-Skalierungsgruppe

Die empfohlene Vorgehensweise besteht darin, die Resource Manager-Vorlage zu exportieren, diese an die neue Umgebung anzupassen und eine erneute Bereitstellung in der Zielregion vorzunehmen. Sie sollten nur die Basisvorlage exportieren und in der neuen Umgebung erneut bereitstellen, da die einzelnen VMSS-Instanzen alle gleich sein sollten.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu VM-Skalierungsgruppen anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/virtual-machine-scale-sets/#step-by-step-tutorials) auf.
- Machen Sie sich damit vertraut, wie eine [Azure Resource Manager-Vorlage exportiert wird](../azure-resource-manager/resource-manager-export-template.md).
- Lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [Azure-VM-Skalierungsgruppe: Übersicht](../virtual-machine-scale-sets/overview.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)
- [Azure-Standorte: Übersicht](https://azure.microsoft.com/global-infrastructure/locations/)
- [Erneutes Bereitstellen einer Vorlage](../azure-resource-manager/resource-group-template-deploy.md)










## <a name="app-service---web-apps"></a>App Service – Web-Apps

Die Migration von App Services von Azure Deutschland zu Azure weltweit wird zurzeit nicht unterstützt. Die empfohlene Vorgehensweise besteht im Exportieren als Resource Manager-Vorlage und der erneuten Bereitstellung nach dem Ändern der Standorteigenschaft in die neue Zielregion.

> [!IMPORTANT]
> Ändern Sie den Speicherort, Geheimnisse des Schlüsseltresors, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu App Services anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/app-service/#step-by-step-tutorials) auf.
- Machen Sie sich mit den Vorgehensweisen zum [Exportieren einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-export-template.md) vertraut, oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="references"></a>Referenzen

- [App Service: Übersicht](../app-service/app-service-web-overview.md)
- [Exportieren einer Resource Manager-Vorlage mithilfe von PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md#export-resource-group-as-template)
- [Azure-Standorte: Übersicht](https://azure.microsoft.com/global-infrastructure/locations/)
- [Erneutes Bereitstellen einer Vorlage](../azure-resource-manager/resource-group-template-deploy.md)
