---
title: Zuordnen der Nutzung durch Azure-Partner und -Kunden
description: Übersicht über die Nachverfolgung der Kundennutzung für Azure Marketplace-Lösungen
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/26/2018
ms.author: ellacroi
ms.openlocfilehash: ce862758d97737d16ef26ca7172cad39f8d8336a
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359518"
---
# <a name="azure-partner-customer-usage-attribution"></a>Zuordnen der Nutzung durch Kunden von Azure-Partnern

Als Softwarepartner für Azure erfordern Ihre Lösungen entweder Azure-Komponenten, oder sie werden direkt in der Azure-Infrastruktur bereitgestellt.  Wenn ein Kunde eine Partnerlösung und seine eigenen Azure-Ressourcen bereitstellt, ist es heute für den Partner schwierig, einen Einblick in den Status dieser Bereitstellungen und die Auswirkungen auf das Azure-Wachstum zu erhalten. Umfangreichere Einsichten helfen Partnern bei der Abstimmung mit den Microsoft-Vertriebsteams und dem Erhalt von Gutschriften für Microsoft-Partnerprogramme.   

Microsoft erstellt eine neue Methode, um Partnern zu helfen, die Azure-Nutzung besser verfolgen zu können, die das Ergebnis der Bereitstellung Ihrer Software durch einen Kunden in Azure ist. Diese neue Methode basiert darauf, mit Azure Resource Manager die Bereitstellung von Azure-Diensten zu orchestrieren.

Als Microsoft-Partner können Sie die Azure-Nutzung allen Azure-Ressourcen zuordnen, die Sie für einen Kunden bereitstellen.  Diese Zuordnung kann über den Azure Marketplace, das Repository für Schnellstartvorlagen, private GitHub-Repositorys und sogar bei direkten Kundeninteraktionen erfolgen.  Um die Nachverfolgung zu ermöglichen, stehen zwei Ansätze zur Verfügung:

- Azure Resource Manager-Vorlagen: Azure Resource Manager-Vorlagen oder Lösungsvorlagen zum Bereitstellen der Azure-Dienste zur Ausführung der Software des Partners. Partner können Azure Resource Manager-Vorlagen erstellen, die die Infrastruktur und Konfiguration Ihrer Azure-Lösung definieren. Durch das Erstellen einer Azure Resource Manager-Vorlage können Sie und Ihre Kunden die Lösung während ihres Lebenszyklus bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden. 

- Azure Resource Manager-APIs: Partner können die Azure Resource Manager-APIs direkt aufrufen, um eine Azure Resource Manager-Vorlage bereitzustellen oder um die API-Aufrufe zu generieren, um Azure-Dienste direkt bereitzustellen. 

## <a name="method-1-azure-resource-manager-templates"></a>Methode 1: Azure Resource Manager-Vorlagen 

Heutzutage werden viele Partnerlösungen im Abonnement eines Kunden mithilfe von Azure Resource Manager-Vorlagen bereitgestellt.  Wenn Sie bereits über eine Azure Resource Manager-Vorlage im Azure Marketplace, auf GitHub oder als Schnellstart verfügen, sollte das Ändern der Vorlage, um diese neue Methode zur Nachverfolgung zu ermöglichen, unkompliziert sein.  Wenn Sie noch keine Azure Resource Manager-Vorlage verwenden, finden Sie hier einige Links, um Ihnen mit Azure Resource Manager-Vorlagen und deren Erstellung zu helfen: 

*   [Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Leitfaden zum Erstellen einer Lösungsvorlage für den Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="instructions-add-a-guid-to-your-existing-azure-resource-manager-template"></a>Anweisungen: Hinzufügen einer GUID zur vorhandenen Azure Resource Manager-Vorlage

Das Hinzufügen der GUID ist eine einzelne Änderung der Hauptvorlagendatei:
 1. Erstellen Sie eine GUID. Nehmen wir an, dass der generierte Wert „eb7927c8-dd66-43e1-b0cf-c346a422063“ ist.
 2. Öffnen Sie die Azure Resource Manager-Vorlage.
 3. Fügen Sie eine neue Ressource in der Hauptvorlagendatei hinzu. Die Ressource muss nur in „mainTemplate.json“ oder „azuredeploy.json“ vorhanden sein, nicht in geschachtelten oder verknüpften Vorlagen.
 4. Geben Sie die GUID nach „pid-“ ein, wie oben gezeigt.

   Das sollte in etwa wie dieses Beispiel aussehen: `pid-eb7927c8-dd66-43e1-b0cf-c346a422063`

 5. Überprüfen Sie die Vorlage auf Fehler.
 6. Veröffentlichen Sie die Vorlage in den entsprechenden Repositorys erneut.

## <a name="sample-template-code"></a>Beispielcode der Vorlage

```

{ // add this resource to the mainTemplate.json (do not add the entire file)
      "apiVersion": "2018-02-01",
      "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your GUID here
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": []
        }
      }
    } // remove all comments from the file when done

```

## <a name="method-2-azure-resource-manager-apis"></a>Methode 2: Azure Resource Manager-APIs

In einigen Fällen bevorzugen Sie eventuell, Aufrufe direkt für die Azure Resource Manager-REST-APIs auszuführen, um Azure-Dienste bereitzustellen. [Azure unterstützt mehrere SDKs](https://docs.microsoft.com/azure/#pivot=sdkstools), um dies zu ermöglichen.  Sie können eines der SDKs verwenden oder die REST-APIs direkt aufrufen, um Ressourcen bereitzustellen.

Wenn Sie eine Azure Resource Manager-Vorlage verwenden, sollten Sie Ihre Lösung mithilfe der oben genannten Anweisungen markieren.  Wenn Sie keine Azure Resource Manager-Vorlage verwenden und direkte API-Aufrufe vornehmen, können Sie dennoch die Bereitstellung markieren, um die Nutzung von Azure-Ressourcen zuzuordnen. 

**Markieren einer Bereitstellung mithilfe der Azure Resource Manager-APIs:** Für diesen Ansatz nehmen Sie beim Entwerfen Ihrer API-Aufrufe eine GUID in den Benutzer-Agent-Header in der Anforderung auf. Die GUID sollte für jedes Angebot oder jede SKU hinzugefügt werden.  Die Zeichenfolge muss aus dem Präfix „pid-“ und der vom Partner generierten GUID bestehen.   

>[!Note] 
>GUID-Format für das Einfügen in den Benutzer-Agent: „pid-eb7927c8-dd66-43e1-b0cf-c346a422063“. Geben Sie Ihre GUID nach „pid-“ ein.

Das Format der Zeichenfolge ist wichtig. Wenn das Präfix „pid-“ nicht enthalten ist, können die Daten nicht abgefragt werden. Verschiedene SDKs gehen dabei unterschiedlich vor.  Zum Implementieren dieser Methode müssen Sie die Unterstützung und den Ansatz Ihres bevorzugten Azure SDK überprüfen. 

**Beispiel für die Verwendung des Python SDK:** Für Python müssen Sie das Attribut „config“ verwenden. Sie können es nur einem UserAgent hinzufügen. Beispiel: 

```python

client = azure.mgmt.servicebus.ServiceBusManagementClient(**parameters)
        client.config.add_user_agent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

>Dies muss für jeden Client durchgeführt werden. Es gibt keine globale statische Konfiguration. (Sie könnten eine Clientfactory verwenden, um sicherzustellen, dass jeder Client entsprechend vorgeht.) 
>[Weitere Referenzinformationen](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)

Markieren einer Bereitstellung mit Azure PowerShell oder Azure-Befehlszeilenschnittstelle: Wenn Sie Ressourcen über Azure PowerShell bereitstellen, können Sie Ihre GUID mithilfe der folgenden Methode anfügen:

```

[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

Um Ihre GUID anzufügen, wenn Sie Azure CLI verwenden, legen Sie die Umgebungsvariable AZURE_HTTP_USER_AGENT fest.  Sie können diese Variable für den Bereich eines Skripts oder global für den Bereich der Shell festlegen:

```

export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'


```

## <a name="registering-guidsoffers"></a>Registrieren von GUIDs/Angeboten

Damit die GUID in unsere Nachverfolgung eingeschlossen wird, muss sie registriert werden.  

Alle Registrierungen für Vorlagen-GUIDs erfolgen über das Azure Marketplace-Cloud-Partnerportal (CPP). 

Bewerben Sie sich noch heute beim [Azure Marketplace](http://aka.ms/listonazuremarketplace), damit Sie Zugriff auf das Cloud-Partnerportal erhalten.

*   Partner müssen [ein Profil im CPP haben](https://docs.microsoft.com/azure/marketplace/become-publisher) und sollten das Angebot im Azure Marketplace oder in AppSource listen. 
*   Partner können mehrere GUIDs registrieren. 
*   Partner können auch eine GUID für die nicht im Marketplace verfügbaren Lösungsvorlagen/-angebote registrieren.

Nachdem Sie die GUID in der Vorlage oder im Benutzer-Agent hinzugefügt und die GUID im CPP registriert haben, werden alle Bereitstellungen nachverfolgt. 

## <a name="verification-of-guid-deployment"></a>Überprüfen der GUID-Bereitstellung 

Nachdem Sie Ihre Vorlage geändert und eine Testbereitstellung durchgeführt haben, können Sie mit dem folgenden PowerShell-Skript die Ressourcen abrufen, die Sie bereitgestellt und markiert haben. 

Sie können damit überprüfen, ob die GUID erfolgreich der Azure Resource Manager-Vorlage hinzugefügt wurde. Es gilt nicht für die Azure Resource Manager-API-Bereitstellung.

Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus, das die zu überprüfende Bereitstellung enthält, bevor Sie das Skript ausführen. Es muss im Abonnementkontext der Bereitstellung ausgeführt werden.

Die GUID und der Ressourcengruppenname der Bereitstellung sind erforderliche Parameter.

Sie finden das ursprüngliche Skript [hier](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1).

```

Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName'
)

#get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

#find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

#find all deploymentOperations in a deployment by name (since PowerShell does not surface outputResources on the deployment or correlationId on the deploymentOperation)

foreach ($deployment in $deployments){

#get deploymentOperations by deploymentName and then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}


```

## <a name="guidance-on-creating-guids"></a>Anleitung zum Erstellen von GUIDs

Eine GUID (Globally Unique Identifier) ist eine eindeutige hexadezimale Referenznummer mit 32 Stellen. Zum Erstellen einer GUID sollten Sie einen GUID-Generator verwenden, um die GUIDs für die Nachverfolgung zu erstellen.  Sie können mehrere [Online-GUID-Generatoren](https://www.bing.com/search?q=guid%20generator&qs=n&form=QBRE&sp=-1&ghc=2&pq=guid%20g&sc=8-6&sk=&cvid=0BAFAFCD70B34E4296BB97FBFA3E1B4E) verwenden.

Es wird empfohlen, eine eindeutige GUID für jedes Angebot und jeden Distributionskanal zu erstellen.  Beispiel: Sie verfügen über zwei Lösungen, die beide über eine Vorlage bereitgestellt werden und die im Azure Marketplace und auf GitHub verfügbar sind.  Erstellen Sie vier GUIDs:

*   Angebot A im Azure Marketplace 
*   Angebot A auf GitHub
*   Angebot B im Azure Marketplace 
*   Angebot B auf GitHub

Die Berichterstellung erfolgt nach Partner (Microsoft-Partner-ID) und GUID. 

Sie können auch auswählen, GUIDs auf einer präziseren Ebene, also pro SKU, nachzuverfolgen (wobei SKUs Varianten eines Angebots sind).

## <a name="guidance-on-privacy-and-data-collection"></a>Anleitungen zum Datenschutz und zur Datenerfassung

Partner sollten die Kunden darüber informieren, dass Microsoft für Bereitstellungen mit Azure Resource Manager-GUID-Nachverfolgung die Azure-Nutzung, die mit diesen Bereitstellungen verbunden ist, dem Partner melden kann.  Im Folgenden finden Sie einige Beispielformulierungen. Wenn „PARTNER“ angegeben ist, sollten Sie Ihren Firmennamen eingeben. Darüber hinaus sollten Partner sicherstellen, dass die Formulierung mit den eigenen Richtlinien für Datenschutz und Datensammlung übereinstimmt, einschließlich Optionen für Kunden, von der Nachverfolgung ausgeschlossen zu werden: 

**Für Azure Resource Manager-Vorlagenbereitstellungen**

Wenn Sie diese Vorlage bereitstellen, kann Microsoft die Installation der Software von PARTNER mit den bereitgestellten Azure-Ressourcen identifizieren.  Microsoft kann die Azure-Ressourcen korrelieren, die zum Unterstützen der Software verwendet werden.  Microsoft erfasst diese Informationen, um die Nutzung der Produkte zu optimieren und um Geschäfte zu ermöglichen. Diese Daten werden gemäß den Datenschutzrichtlinien von Microsoft erfasst und unterliegen ihnen. Diese Datenschutzrichtlinien können unter https://www.microsoft.com/trustcenter eingesehen werden. 

**Für SDK- oder API-Bereitstellungen**

Wenn Sie Software von PARTNER bereitstellen, kann Microsoft die Installation der Software von PARTNER mit den bereitgestellten Azure-Ressourcen identifizieren.  Microsoft kann die Azure-Ressourcen korrelieren, die zum Unterstützen der Software verwendet werden.  Microsoft erfasst diese Informationen, um die Nutzung der Produkte zu optimieren und um Geschäfte zu ermöglichen. Diese Daten werden gemäß den Datenschutzrichtlinien von Microsoft erfasst und unterliegen ihnen. Diese Datenschutzrichtlinien können unter https://www.microsoft.com/trustcenter eingesehen werden.

## <a name="support"></a>Support

Hilfe erhalten Sie mit den folgenden Schritten:
 1. Besuchen Sie die Supportseite unter [go.microsoft.com/fwlink/?linkid=844975](https://go.microsoft.com/fwlink/?linkid=844975).
 2. Wählen Sie bei Problemen mit der Zuordnung der Nutzung als Problemtyp **Marketplace Onboarding** und als Kategorie **Other** aus, und klicken Sie dann auf **Start Request**. 
>[!Note]
>Wählen Sie bei Problemen mit dem Zugriff auf das Azure Marketplace-Cloud-Partnerportal als Problemtyp **Marketplace Onboarding** und als Kategorie **Access Problem** aus, und klicken Sie dann auf **Start Request**.
 3. Füllen Sie die Pflichtfelder auf der nächsten Seite aus, und klicken Sie auf **Continue**.
 4. Geben Sie in die Felder auf der nächsten Seite freien Text ein.  
 

 
>[!Important] 
>Geben Sie als Titel des Incidents **ISV Usage Tracking** ein, und beschreiben Sie Ihr Problem dann im großen Freitextfeld genau.  Füllen Sie den Rest des Formulars aus, und klicken Sie auf **Submit**.

## <a name="faqs"></a>Häufig gestellte Fragen

**Welchen Vorteil hat das Hinzufügen der GUID zur Vorlage?**

Microsoft bietet Partnern eine Ansicht der Kundenbereitstellungen ihrer Vorlagen und Erkenntnisse über den Einfluss auf die Nutzung.  Sowohl Microsoft als auch der Partner können diese Informationen auch dazu verwenden, eine engere Beziehung zwischen den Vertriebsteams zu fördern. Außerdem erhalten Microsoft und der Partner damit auch eine konsistentere Ansicht der Auswirkungen der einzelnen Partner auf das Azure-Wachstum. 

**Wer kann einer Vorlage eine GUID hinzufügen?**

Mit der Nachverfolgungsressource soll die Lösung des Partners mit der Azure-Nutzung durch den Kunden verknüpft werden.  Die Nutzungsdaten sind an die Microsoft Partner Network-Identität (MPN-ID) des Partners gebunden, und Berichte stehen den Partnern im Cloud-Partnerportal (CPP) zur Verfügung.  

**Kann eine GUID geändert werden, nachdem sie hinzugefügt wurde?**
 
Ja, ein Kunde oder Implementierungspartner kann die Vorlage anpassen und könnte die GUID ändern oder entfernen. Es wird empfohlen, dass Partner ihren Kunden und Partnern proaktiv die Funktion der Ressource und der GUID beschrieben, um das Entfernen oder Ändern der Nachverfolgungs-GUID zu verhindern.  Eine Änderung der GUID hat nur Auswirkungen auf neue, noch nicht vorhandene Bereitstellungen und Ressourcen.

**Wann ist die Berichterstellung verfügbar?**

Eine Betaversion der Berichterstellung sollte in Kürze verfügbar sein.  Die Berichterstellung wird in das Cloud-Partnerportal (CPP) integriert.

**Kann ich Vorlagen nachverfolgen, die nicht über ein Microsoft-Repository, sondern z.B. über GitHub bereitgestellt wurden?**

Ja, sofern die GUID vorhanden ist, wird die Nutzung nachverfolgt, wenn die Vorlage bereitgestellt wird.  
Partner müssen über ein Profil im Cloud-Partnerportal verfügen, um die entsprechenden Vorlagen zu registrieren, die außerhalb des Azure Marketplace veröffentlicht wurden. 

**Gibt es einen Unterschied, wenn die Vorlage über den Azure Marketplace oder über andere Repositorys wie GitHub bereitgestellt wird?**

Ja, Partner, die Angebote im Azure Marketplace veröffentlichen, können ausführlichere Daten zu Bereitstellungen aus dem Azure Marketplace erhalten.  Partner profitieren davon, dass sie ihr Angebot Kunden im Azure Marketplace-Portal und im Azure-Verwaltungsportal verfügbar machen. Angebote im Azure Marketplace generieren zudem Leads für den Partner.

**Was geschieht, wenn ich eine benutzerdefinierte Vorlage für eine einzelne Kundeninteraktion erstelle?**

Sie können gerne dennoch der Vorlage die GUID hinzufügen.  Wenn Sie eine vorhandene GUID verwenden, die registriert wurde, wird sie in die Berichte aufgenommen.  Wenn Sie eine neue GUID erstellen, müssen Sie die neue GUID registrieren, damit sie in die Nachverfolgung aufgenommen wird.

**Erhält der Kunde die Berichte auch?**

Kunden können derzeit ihre Nutzung einzelnen Ressourcen oder benutzerdefinierter Ressourcengruppen im Azure-Verwaltungsportal nachverfolgen.   

**Ist diese Nachverfolgungsmethode mit dem digitalen Partner des Eintrags (Digital Partner of Record, DPOR) vergleichbar?**

Diese neue Methode, die Bereitstellung und Nutzung der Lösung eines Partners zu verbinden, soll einen Mechanismus zum Verknüpfen einer Partnerlösung mit der Azure-Nutzung bieten.  Mit DPOR soll ein Beratungspartner (Systemintegrator) oder ein Verwaltungspartner (Managed Services Provider) dem Azure-Abonnement eines Kunden zugeordnet werden.   
