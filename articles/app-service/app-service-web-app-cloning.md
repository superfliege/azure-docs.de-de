---
title: Klonen einer Web-App mithilfe von PowerShell
description: Erfahren Sie, wie Sie Ihre Web-Apps mit PowerShell in neue Web-Apps klonen.
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.openlocfilehash: 30817a1a6a8079e7a896305ab0b59e48fad4d644
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Klonen der Azure App Service-App mit PowerShell
Mit der Veröffentlichung von Microsoft Azure PowerShell Version 1.1.0 wurde `New-AzureRMWebApp` eine neue Option hinzugefügt, mit der Sie eine vorhandene Web-App in eine neu erstellte App in einer anderen Region oder derselben Region klonen können. Durch diese Option können Kunden eine Reihe von Apps in unterschiedlichen Regionen bereitstellen.

Das Klonen einer App wird zurzeit nur im Premium-Tarif der App Service-Pläne unterstützt. Das neue Feature verwendet die gleichen Einschränkungen wie das Web-Apps-Sicherungsfeature. Informationen dazu finden Sie unter [Sichern von Web-Apps in Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen App
Szenario: Sie möchten den Inhalt einer vorhandenen Web-App in der Region „USA, Süden-Mitte“ in eine neue Web-App in der Region „USA, Norden-Mitte“ klonen. Dies kann mithilfe der Azure Resource Manager-Version des PowerShell-Cmdlets zum Erstellen einer neuen Web-App mit der Option `-SourceWebApp` erfolgen.

Wenn der Name der Ressourcengruppe mit der Quell-Web-App bekannt ist, können Sie die Informationen der Quell-Web-App (in diesem Fall mit der Bezeichnung `source-webapp`) mit dem folgenden PowerShell-Befehl erhalten:

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Um einen neuen App Service-Plan zu erstellen, können Sie den Befehl `New-AzureRmAppServicePlan` wie im folgenden Beispiel verwenden:

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Mithilfe des Befehls `New-AzureRmWebApp` können Sie die neue Web-App in der Region „USA, Norden-Mitte“ erstellen und mit einem vorhandenen App Service-Plan im Premium-Tarif verknüpfen. Darüber hinaus können Sie dieselbe Ressourcengruppe wie für die Quell-Web-App verwenden oder eine neue Ressourcengruppe definieren, wie im folgenden Befehl gezeigt:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Um eine vorhandene Web-App zu klonen, einschließlich aller zugehörigen Bereitstellungsslots, müssen Sie den `IncludeSourceWebAppSlots`-Parameter verwenden. Die folgende PowerShell-Befehl veranschaulicht die Verwendung dieses Parameters mit dem Befehl `New-AzureRmWebApp`:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Um eine vorhandene Web-App innerhalb derselben Region zu klonen, müssen Sie eine neue Ressourcengruppe und einen neuen App Service-Plan in derselben Region erstellen und dann den folgenden PowerShell-Befehl zum Klonen der Web-App verwenden:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen App in einer App Service-Umgebung
Szenario: Sie möchten den Inhalt einer vorhandenen Web-App in der Region „USA, Süden-Mitte“ in eine neue Web-App in einer bereits vorhandenen App Service-Umgebung (ASE) klonen.

Wenn der Name der Ressourcengruppe mit der Quell-Web-App bekannt ist, können Sie die Informationen der Quell-Web-App (in diesem Fall mit der Bezeichnung `source-webapp`) mit dem folgenden PowerShell-Befehl erhalten:

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Wenn Sie den Namen der ASE sowie den Namen der Ressourcengruppe kennt, zu der die ASE gehört, können Sie die neue Web-App in der bereits vorhandenen ASE erstellen, wie im folgenden Befehl gezeigt:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

Der `Location`-Parameter ist aus Gründen der Kompatibilität mit älteren Versionen erforderlich. Er wird jedoch ignoriert, wenn Sie die App in einer ASE erstellen. 

## <a name="cloning-an-existing-app-slot"></a>Klonen eines vorhandenen App-Slots
Szenario: Sie möchten einen vorhandenen Web-App-Slot in eine neue Web-App oder in einen neuen Web-App-Slot klonen. Die neue Web-App kann in der gleichen Region wie der ursprüngliche Web-App-Slot oder in einer anderen Region sein.

Wenn der Name der Ressourcengruppe, die die Quell-Web-App enthält, bekannt ist, können Sie die Informationen des Quell-Web-App-Slots (in diesem Fall mit der Bezeichnung `source-webappslot`), der mit der Web-App `source-webapp` verknüpft ist, mit dem folgenden PowerShell-Befehl abrufen:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

Der folgende Befehl zeigt, wie ein Klon der Quell-Web-App in einer neuen Web-App erstellt wird:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Konfigurieren von Traffic Manager beim Klonen einer App
Das Erstellen von Web-Apps mit mehreren Regionen und das Konfigurieren von Azure Traffic Manager zum Weiterleiten von Datenverkehr an diese Web-Apps ist ein wichtiges Szenario, mit dem sichergestellt wird, dass die Apps der Kunden hoch verfügbar sind. Beim Klonen einer vorhandenen Web-App können Sie beide Web-Apps mit einem neuen oder vorhandenen Traffic Manager-Profil verbinden. Nur die Azure Resource Manager-Version von Traffic Manager wird unterstützt.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Erstellen eines neuen Traffic Manager-Profils beim Klonen einer App
Szenario: Sie möchten eine Web-App in eine andere Region klonen, während gleichzeitig ein Azure Resource Manager-Traffic Manager-Profil angelegt wird, das beide Web-Apps enthält. Der folgende Befehl zeigt das Erstellen eines Klons der Quell-Web-App in einer neuen Web-App bei gleichzeitigem Konfigurieren eines neuen Traffic Manager-Profils:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Hinzufügen neuer geklonter Web-Apps zu einem vorhandenen Traffic Manager-Profil
Szenario: Sie besitzen bereits ein Azure Resource Manager-Traffic Manager-Profil, dem Sie beide Web-Apps als Endpunkte hinzufügen möchten. Dazu müssen Sie zuerst die vorhandene Traffic Manager-Profil-ID zusammenstellen. Sie benötigen die Abonnement-ID, den Namen der Ressourcengruppe und den vorhandenen Traffic Manager-Profilnamen.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Das folgende Beispiel zeigt, wie nach der Zusammenstellung der Traffic Manager-ID ein Klon der Quell-Web-App als neue Web-App und gleichzeitig ein neues Traffic Manager-Profil erstellt werden:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Aktuelle Einschränkungen
Dieses Feature befindet sich zurzeit in der Vorschau, und neue Funktionen werden mit der Zeit hinzugefügt. Hier finden Sie die bekannten Einschränkungen für die aktuelle Version des App-Klonens:

* Einstellungen für automatische Skalierung werden nicht geklont
* Einstellungen des Sicherungszeitplans werden nicht geklont
* VNET-Einstellungen werden nicht geklont
* App Insights werden nicht automatisch auf der Ziel-Web-App eingerichtet
* Easy Auth-Einstellungen werden nicht geklont
* Kudu-Erweiterungen werden nicht geklont
* TiP-Regeln werden nicht geklont
* Datenbankinhalte werden nicht geklont.
* Ausgehende IP-Adressen ändern sich beim Klonen in eine andere Skalierungseinheit.

### <a name="references"></a>Referenzen
* [Klonen von Web-App](app-service-web-app-cloning.md)
* [Sichern von Web-Apps in Azure App Service](web-sites-backup.md)
* [Unterstützung des Azure Resource Manager für Azure Traffic Manager – Vorschau](../traffic-manager/traffic-manager-powershell-arm.md)
* [Einführung in die App Service-Umgebung](environment/intro.md)
* [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

