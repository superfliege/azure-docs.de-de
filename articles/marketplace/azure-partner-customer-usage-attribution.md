---
title: Zuordnen der Nutzung durch Azure-Partner und -Kunden
description: Übersicht über die Nachverfolgung der Kundennutzung für Azure Marketplace-Lösungen
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: yijenj
ms.openlocfilehash: a0b3c220a1cd857bc8bea0eb5ab41625845fcc5d
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365620"
---
# <a name="azure-partner-customer-usage-attribution"></a>Zuordnen der Nutzung durch Kunden von Azure-Partnern

Als Softwarepartner für Azure erfordern Ihre Lösungen entweder Azure-Komponenten oder müssen direkt in der Azure-Infrastruktur bereitgestellt werden. Für Kunden, die eine Partnerlösung und eigene Azure-Ressourcen bereitstellen, kann es möglicherweise schwierig sein, einen Einblick in den Status der Bereitstellung und den Einfluss auf das Azure-Wachstum zu erhalten. Ein höherer Sichtbarkeitsgrad hilft bei der Abstimmung mit den Microsoft-Vertriebsteams und dem Erhalt von Gutschriften für Microsoft-Partnerprogramme.   

Microsoft bietet jetzt eine Methode an, um Partner bei der besseren Verfolgung der Azure-Nutzung durch Kundenbereitstellungen ihrer Software unter Azure zu unterstützen. Diese neue Methode verwendet Azure Resource Manager, um die Bereitstellung von Azure-Diensten zu orchestrieren.

Als Microsoft-Partner können Sie die Azure-Nutzung allen Azure-Ressourcen zuordnen, die Sie für einen Kunden bereitstellen. Sie können die Zuordnung über den Azure Marketplace, das Schnellstartrepository, private GitHub-Repositorys und direkte Kundeninteraktion herstellen. Für die Nachverfolgung stehen zwei Ansätze zur Verfügung:

- Azure Resource Manager-Vorlagen: Resource Manager-Vorlagen oder Lösungsvorlagen zum Bereitstellen der Azure-Dienste zum Ausführen der Software des Partners. Partner können Resource Manager-Vorlagen erstellen, um die Infrastruktur und Konfiguration ihrer Azure-Lösung zu definieren. Mit einer Resource Manager-Vorlage können Sie und Ihre Kunden die Lösung während des gesamten Lebenszyklus bereitstellen. Sie können sicher sein, dass Ihre Ressourcen in einem konsistenten Zustand bereitgestellt werden. 

- Azure Resource Manager-APIs: Partner können die Azure Resource Manager-APIs direkt aufrufen, um eine Resource Manager-Vorlage bereitzustellen, oder um API-Aufrufe für die direkte Bereitstellung von Azure-Diensten zu generieren. 

## <a name="use-resource-manager-templates"></a>Verwenden von Resource Manager-Vorlagen

Viele Partnerlösungen werden mithilfe von Resource Manager-Vorlagen im Abonnement eines Kunden bereitgestellt. Wenn Sie eine Resource Manager-Vorlage haben, die im Azure Marketplace, auf GitHub oder als Schnellstart verfügbar ist, sollte das Ändern der Vorlage für diese neue Nachverfolgungsmethode unkompliziert sein. Wenn Sie keine Azure Resource Manager-Vorlage verwenden, finden Sie hier einige Links, um Ihnen mit Azure Resource Manager-Vorlagen und deren Erstellung zu helfen: 

*   [Erstellen und Bereitstellen Ihrer ersten Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Erstellen einer Lösungsvorlage für Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="add-a-guid-to-your-template"></a>Hinzufügen einer GUID zu Ihrer Vorlage

Um einen global eindeutigen Bezeichner (GUID) hinzuzufügen, nehmen Sie an der Hauptvorlagendatei eine einzige Änderung vor:

1. [Erstellen Sie eine GUID](#create-guids) (z. B. eb7927c8-dd66-43e1-b0cf-c346a422063), und [registrieren Sie die GUID](#register-guids-and-offers).

1. Öffnen Sie die Resource Manager-Vorlage.

1. Fügen Sie eine neue Ressource in der Hauptvorlagendatei hinzu. Die Ressource darf sich nur in der Datei **mainTemplate.json** oder **azuredeploy.json** befinden, nicht in geschachtelten oder verknüpften Vorlagen.

1. Geben Sie den GUID-Wert nach dem Präfix **pid-** ein (z. B. pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Überprüfen Sie die Vorlage auf Fehler.

1. Veröffentlichen Sie die Vorlage in den entsprechenden Repositorys erneut.

1. [Überprüfen Sie den Erfolg der GUID in der Vorlagenbereitstellung](#verify-the-guid-deployment).

### <a name="sample-template-code"></a>Beispielcode der Vorlage

![Beispielcode der Vorlage](media/marketplace-publishers-guide/tracking-sample-code-for-lu-1.PNG)

## <a name="use-the-resource-manager-apis"></a>Verwenden der Resource Manager-APIs

In einigen Fällen bevorzugen Sie möglicherweise zum Bereitstellen von Azure-Diensten direkte Aufrufe an die Resource Manager-REST-APIs. [Azure unterstützt mehrere SDKs](https://docs.microsoft.com/azure/#pivot=sdkstools), um diese Aufrufe zu ermöglichen. Sie können eines der SDKs verwenden oder die REST-APIs direkt aufrufen, um Ressourcen bereitzustellen.

Wenn Sie eine Azure Resource Manager-Vorlage verwenden, sollten Sie Ihre Lösung mithilfe der zuvor beschriebenen Anweisungen markieren. Wenn Sie keine Resource Manager-Vorlage verwenden und direkte API-Aufrufe vornehmen, können Sie dennoch die Bereitstellung markieren, um die Nutzung von Azure-Ressourcen zuzuordnen. 

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Markieren einer Bereitstellung mithilfe der Resource Manager-APIs

Nehmen Sie bei diesem Nachverfolgungsansatz beim Entwerfen Ihrer API-Aufrufe eine GUID in den Benutzer-Agent-Header in der Anforderung auf. Fügen Sie die GUID für jedes Angebot oder SKU hinzu. Formatieren Sie die Zeichenfolge mit dem Präfix **pid-**, und fügen Sie die vom Partner generierte GUID ein. Nachfolgend finden Sie ein Beispiel für das GUID-Format zum Einfügen in den Benutzer-Agent: 

![GUID-Beispielformat](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> Das Format der Zeichenfolge ist wichtig. Wenn das Präfix **pid-** nicht enthalten ist, können die Daten nicht abgefragt werden. Verschiedene SDKs gehen bei der Nachverfolgung unterschiedlich vor. Zum Implementieren dieser Methode müssen Sie die Unterstützung und den Nachverfolgungsansatz Ihres bevorzugten Azure SDK überprüfen. 

### <a name="example-the-python-sdk"></a>Beispiel: Python SDK

Bei Python wird das Attribut **config** verwendet. Sie können das Attribut nur einem Benutzer-Agent hinzufügen. Hier sehen Sie ein Beispiel:

![Attribut zu einem Benutzer-Agent hinzufügen](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Fügen Sie das Attribut für jeden Client hinzu. Es gibt keine globale statische Konfiguration. Sie können eine Clientfactory markieren, um sicherzustellen, dass jeder Client nachverfolgt wird. Weitere Informationen finden Sie im [Clientfactory-Beispiel auf GitHub](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79).

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Markieren einer Bereitstellung mithilfe von Azure PowerShell

Wenn Sie Ressourcen mithilfe von Azure PowerShell bereitstellen, fügen Sie Ihre GUID mit der folgenden Methode an:

```
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Markieren einer Bereitstellung mithilfe der Azure CLI

Wenn Sie Ihre GUID mithilfe der Azure CLI anfügen, legen Sie die Umgebungsvariable **AZURE_HTTP_USER_AGENT** fest. Sie können diese Variable im Rahmen eines Skripts festlegen. Sie können die Variable auch global für den Bereich der Shell festlegen:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```

## <a name="create-guids"></a>Erstellen von GUIDs

Eine GUID ist ein eindeutiger Verweis mit 32 Hexadezimalziffern. Zum Erstellen einer GUID für die Nachverfolgung sollten Sie einen GUID-Generator verwenden. Es wird empfohlen, die Sie das [GUID-Generatorformular von Azure Storage](https://aka.ms/StoragePartners) nutzen. Wenn Sie den GUID-Generator von Azure Storage jedoch nicht verwenden möchten, gibt es mehrere [Online-GUID-Generatoren](https://www.bing.com/search?q=guid%20generator), die Sie verwenden können.

> [!Note]
> Es wird dringend empfohlen, dass Sie das [GUID-Generatorformular von Azure Storage](https://aka.ms/StoragePartners) verwenden, um Ihre GUID zu erstellen. Weitere Informationen finden Sie in den [häufig gestellten Fragen](#faq).

Erstellen Sie für jedes Angebot und jeden Distributionskanal eine eindeutige GUID. Wenn Sie mithilfe einer Vorlage zwei Lösungen bereitstellen, die jeweils im Azure Marketplace und auf GitHub verfügbar sind, müssen Sie vier GUIDS erstellen:

*   Angebot A im Azure Marketplace 
*   Angebot A auf GitHub
*   Angebot B im Azure Marketplace 
*   Angebot B auf GitHub

Die Berichterstellung erfolgt über den Partnerwert (Microsoft-Partner-ID) und die GUID. 

Sie können GUIDs auch auf einer differenzierteren Ebene (z. B. pro SKU) nachverfolgen (wobei SKUs Varianten eines Angebots sind).

## <a name="register-guids-and-offers"></a>Registrieren von GUIDs und Angeboten

Eine GUID muss registriert werden, um sie in die Nachverfolgung einzubeziehen.  

Alle Registrierungen für Vorlagen-GUIDs erfolgen über das Azure Marketplace-Cloud-Partnerportal (CPP). 

Nachdem Sie die GUID in der Vorlage oder im Benutzer-Agent hinzugefügt und die GUID im CPP registriert haben, werden alle Bereitstellungen nachverfolgt. 

1. Stellen Sie beim [Azure Marketplace](http://aka.ms/listonazuremarketplace) einen Antrag, damit Sie Zugriff auf das Cloud-Partnerportal (CPP) erhalten.

   * Partner müssen [ein Profil im CPP](https://docs.microsoft.com/azure/marketplace/become-publisher) haben. Wir empfehlen Ihnen, das Angebot im Azure Marketplace oder in AppSource aufzulisten.
   * Partner können mehrere GUIDs registrieren.
   * Partner können auch eine GUID für die nicht im Marketplace verfügbaren Lösungsvorlagen und Angebote registrieren.
 
1. Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

1. Wählen Sie oben rechts Ihr Kontosymbol und dann **Herausgeberprofil** aus.

   ![Herausgeberprofil auswählen](media/marketplace-publishers-guide/guid-image-for-lu.png)

1. Wählen Sie auf der Seite **Profil** die Option **Nachverfolgungs-GUID hinzufügen** aus.

   ![„Nachverfolgungs-GUID hinzufügen“ auswählen](media/marketplace-publishers-guide/guid-how-to-add-tracking.png)

1. Geben Sie im Feld **Nachverfolgungs-GUID** Ihre Nachverfolgungs-GUID ein. Geben Sie nur die GUID ohne das Präfix **pid-** ein. Geben Sie im Feld **Benutzerdefinierte Beschreibung** den Angebotsnamen oder eine Beschreibung ein.

   ![Seite „Profil“](media/marketplace-publishers-guide/guid-dev-center-login.png)
   
   ![GUID und Angebotsbeschreibung eingeben](media/marketplace-publishers-guide/guid-dev-center-example.png)

1. Wenn Sie mehrere GUIDs registrieren möchten, klicken Sie erneut auf **Nachverfolgungs-GUID hinzufügen**. Auf der Seite werden weitere Felder angezeigt.

   ![„Nachverfolgungs-GUID hinzufügen“ erneut auswählen](media/marketplace-publishers-guide/guid-dev-center-example-add.png)
   
   ![Weitere GUID und Angebotsbeschreibung eingeben](media/marketplace-publishers-guide/guid-dev-center-example-description.png)

1. Wählen Sie **Speichern**aus.

   ![„Speichern“ auswählen](media/marketplace-publishers-guide/guid-dev-center-save.png)

Nachdem Sie die GUID in der Vorlage oder im Benutzer-Agent hinzugefügt und die GUID im CPP registriert haben, werden alle Bereitstellungen nachverfolgt. 

## <a name="verify-the-guid-deployment"></a>Überprüfen der GUID-Bereitstellung 

Nachdem Sie Ihre Vorlage geändert und eine Testbereitstellung ausgeführt haben, können Sie mit dem folgenden PowerShell-Skript die Ressourcen abrufen, die Sie bereitgestellt und markiert haben. 

Mit dem Skript können Sie überprüfen, ob die GUID erfolgreich der Resource Manager-Vorlage hinzugefügt wurde. Das Skript gilt nicht für die Bereitstellung über die Resource Manager-API.

Melden Sie sich bei Azure an. Wählen Sie das Abonnement mit der Bereitstellung aus, die Sie vor dem Ausführen des Skripts überprüfen möchten. Führen Sie das Skript im Abonnementkontext der Bereitstellung aus.

Die **GUID** und der Name der **Ressourcengruppe** der Bereitstellung sind erforderliche Parameter.

Sie erhalten [das ursprüngliche Skript](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) auf GitHub.

```
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="notify-your-customers"></a>Benachrichtigen der Kunden

Partner sollten ihre Kunden über Bereitstellungen informieren, die eine Nachverfolgung mithilfe von Resource Manager-GUIDs verwenden. Microsoft erstellt für den Partner Berichte über die Azure-Nutzung, die mit diesen Bereitstellungen verknüpft ist. Die folgenden Beispiele enthalten Inhalte, die Sie für die Benachrichtigung Ihrer Kunden über diese Bereitstellungen verwenden können. Ersetzen Sie in den Beispielen \<PARTNER> durch den Namen Ihres Unternehmens. Der Partner muss sicherstellen, dass die Benachrichtigung den eigenen Richtlinien für Datenschutz und Datensammlung entspricht, und Optionen für Kunden vorsehen, die von der Nachverfolgung ausgenommen werden möchten. 

### <a name="notification-for-resource-manager-template-deployments"></a>Benachrichtigung für Bereitstellungen über Resource Manager-Vorlagen

Wenn Sie diese Vorlage bereitstellen, kann Microsoft die Installation der Software von \<PARTNER> mit den bereitgestellten Azure-Ressourcen identifizieren. Microsoft kann die Azure-Ressourcen korrelieren, die zum Unterstützen der Software verwendet werden. Microsoft erfasst diese Informationen, um die Nutzung der Produkte zu optimieren und um Geschäfte zu ermöglichen. Diese Daten unterliegen den Datenschutzrichtlinien von Microsoft und werden gemäß diesen Richtlinien gesammelt. Diese Datenschutzrichtlinien können unter https://www.microsoft.com/trustcenter eingesehen werden. 

### <a name="notification-for-sdk-or-api-deployments"></a>Benachrichtigung für SDK- oder API-Bereitstellungen

Wenn Sie Software von \<PARTNER> bereitstellen, kann Microsoft die Installation der \<PARTNER>-Software mit den bereitgestellten Azure-Ressourcen identifizieren. Microsoft kann die Azure-Ressourcen korrelieren, die zum Unterstützen der Software verwendet werden. Microsoft erfasst diese Informationen, um die Nutzung der Produkte zu optimieren und um Geschäfte zu ermöglichen. Diese Daten unterliegen den Datenschutzrichtlinien von Microsoft und werden gemäß diesen Richtlinien gesammelt. Diese Datenschutzrichtlinien können unter https://www.microsoft.com/trustcenter eingesehen werden.

## <a name="get-support"></a>Support

Wenn Sie Hilfe benötigen, gehen Sie wie folgt vor.

1. Navigieren Sie zur [Supportseite](https://go.microsoft.com/fwlink/?linkid=844975). 

1. Wählen Sie unter **Problemtyp** die Option **Marketplace Onboarding** aus.

1. Wählen Sie die **Kategorie** für Ihr Problem aus:

   - Wählen Sie für Zuordnungsprobleme bei der Nutzung **Sonstige** aus.
   - Wählen Sie bei Problemen mit dem Zugriff auf das Azure Marketplace-CPP **Zugriffsprobleme** aus.
   
    ![Problemkategorie auswählen](media/marketplace-publishers-guide/lu-article-incident.png)

1. Wählen Sie **Anfrage starten** aus.

1. Geben Sie auf der nächsten Seite die erforderlichen Werte ein. Wählen Sie **Weiter**.

1. Geben Sie auf der nächsten Seite die erforderlichen Werte ein.

   > [!Important] 
   > Geben Sie im Feld **Incident-Titel** den Wert **ISV-Nutzungsverfolgung** ein. Beschreiben Sie Ihr Problem im Detail.
   
   ![„ISV-Nutzungsverfolgung“ als „Incident-Titel“ eingeben](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Füllen Sie das Formular vollständig aus, und wählen Sie dann **Senden** aus.

## <a name="faq"></a>Häufig gestellte Fragen

**Welchen Vorteil hat das Hinzufügen der GUID zur Vorlage?**

Microsoft stellt Partnern eine Ansicht der Kundenbereitstellungen ihrer Vorlagen und Erkenntnisse über den Einfluss auf die Nutzung bereit. Sowohl Microsoft als auch der Partner können diese Informationen dazu verwenden, eine engere Beziehung zwischen den Vertriebsteams zu fördern. Microsoft und der Partner können damit auch eine konsistentere Ansicht des Einflusses der einzelnen Partner auf das Azure-Wachstum erhalten. 

**Wer kann einer Vorlage eine GUID hinzufügen?**

Mit der Nachverfolgungsressource soll die Lösung des Partners mit der Azure-Nutzung durch den Kunden verknüpft werden. Die Nutzungsdaten sind an die Microsoft Partner Network-Identität des Partners (MPN-ID) gebunden. Die Berichterstellung steht Partnern im Cloud-Partnerportal (CPP) zur Verfügung.

**Kann eine GUID geändert werden, nachdem sie hinzugefügt wurde?**
 
Ja, ein Kunde oder Implementierungspartner kann die Vorlage anpassen und die GUID ändern oder entfernen. Es wird empfohlen, dass Partner ihren Kunden und Partnern proaktiv die Funktion der Ressource und der GUID beschrieben, um das Entfernen oder Ändern der Nachverfolgungs-GUID zu verhindern. Eine Änderung der GUID hat nur Auswirkungen auf neue, noch nicht vorhandene Bereitstellungen und Ressourcen.

**Wann ist die Berichterstellung verfügbar?**

Eine Betaversion der Berichterstellung sollte in Kürze verfügbar sein. Die Berichterstellung wird in das Cloud-Partnerportal (CPP) integriert.

**Kann ich Vorlagen nachverfolgen, die nicht über ein Microsoft-Repository, sondern z.B. über GitHub bereitgestellt wurden?**

Ja, solange die GUID bei der Bereitstellung der Vorlage vorhanden ist, wird die Nutzung nachverfolgt. Partner müssen über ein Profil im Cloud-Partnerportal verfügen, um die entsprechenden Vorlagen zu registrieren, die außerhalb vom Azure Marketplace veröffentlicht werden. 

**Gibt es einen Unterschied, wenn die Vorlage über den Azure Marketplace oder über andere Repositorys wie GitHub bereitgestellt wird?**

Ja, Partner, die Angebote im Azure Marketplace veröffentlichen, können ausführlichere Daten zu Bereitstellungen über den Azure Marketplace erhalten. Partner profitieren davon, dass sie ihr Angebot Kunden im Azure Marketplace-Portal und im Azure-Portal verfügbar machen. Angebote im Azure Marketplace generieren zudem Leads für den Partner.

**Was geschieht, wenn ich eine benutzerdefinierte Vorlage für eine einzelne Kundeninteraktion erstelle?**

Sie können gerne dennoch die GUID der Vorlage hinzufügen. Wenn Sie eine vorhandene registrierte GUID verwenden, wird sie in die Berichterstellung einbezogen. Wenn Sie eine neue GUID erstellen, müssen Sie die neue GUID registrieren, damit sie in die Nachverfolgung aufgenommen wird.

**Erhält der Kunde die Berichte auch?**

Kunden können ihre Nutzung einzelner Ressourcen oder benutzerdefinierter Ressourcengruppen im Azure-Portal nachverfolgen.   

**Ist diese Nachverfolgungsmethode mit dem digitalen Partner des Eintrags (Digital Partner of Record, DPOR) vergleichbar?**

Diese neue Methode für das Verbinden von Bereitstellung und Nutzung mit der Lösung eines Partners stellt einen Mechanismus zum Verknüpfen einer Partnerlösung mit der Azure-Nutzung bereit. Mit DPOR soll ein Beratungspartner (Systemintegrator) oder Verwaltungspartner (Managed Services Provider) dem Azure-Abonnement eines Kunden zugeordnet werden.   

**Was ist der Vorteil bei der Verwendung des GUID-Generatorformulars von Azure Storage?**

Das GUID-Generatorformular von Azure Storage generiert eine GUID garantiert im erforderlichen Format. Wenn Sie außerdem eine der Verfolgungsmethoden von Azure Storage für die Datenebene verwenden, können Sie dieselbe GUID für die Verfolgung der Marketplace-Steuerungsebene verwenden. Auf diese Weise können Sie eine einzelne einheitliche GUID für die Partnerzuordnung nutzen, ohne separate GUIDs verwalten zu müssen.
