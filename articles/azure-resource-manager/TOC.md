# [Dokumentation zum Azure Resource Manager](index.md)

# Übersicht
## [Was ist Resource Manager?](resource-group-overview.md)
## [Resource Manager-Bereitstellung und klassische Bereitstellung](resource-manager-deployment-model.md)

# Erste Schritte
## [Erstellen und Bereitstellen der Vorlage](resource-manager-create-first-template.md)
## [VS-Codeerweiterung für Vorlagen](resource-manager-vscode-extension.md)
## [Visual Studio mit Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Konzepte
## [Ressourcenanbieter und -typen](resource-manager-supported-services.md)
## [Verwaltungsgruppen](management-groups-overview.md)
## [Abonnementgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

# Anleitung
## Erstellen von Vorlagen
### [Vorlagenabschnitte](resource-group-authoring-templates.md)
#### [Parameter](resource-manager-templates-parameters.md)
#### [Variablen](resource-manager-templates-variables.md)
#### [Funktionen](resource-group-authoring-templates.md#functions)
#### [Ressourcen](resource-manager-templates-resources.md)
#### [Ausgaben](resource-manager-templates-outputs.md)
### [Verknüpfte und geschachtelte Vorlagen](resource-group-linked-templates.md)
### [Definieren der Abhängigkeit zwischen Ressourcen](resource-group-define-dependencies.md)
### [Erstellen mehrerer Instanzen](resource-group-create-multiple.md)
### [Aktualisieren von Ressourcen](/azure/architecture/building-blocks/extending-templates/update-resource)

## Bereitstellen
### Azure PowerShell
#### [Bereitstellen der Vorlage](resource-group-template-deploy.md)
#### [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md)
#### [Exportieren der Vorlage und erneutes Bereitstellen](resource-manager-export-template-powershell.md)
### Azure-Befehlszeilenschnittstelle
#### [Bereitstellen der Vorlage](resource-group-template-deploy-cli.md)
#### [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md)
#### [Exportieren der Vorlage und erneutes Bereitstellen](resource-manager-export-template-cli.md)
### Azure-Portal
#### [Bereitstellen von Ressourcen](resource-group-template-deploy-portal.md)
#### [Exportieren der Vorlage](resource-manager-export-template.md)
### [REST-API](resource-group-template-deploy-rest.md)
### [Mehrere Ressourcengruppen oder Abonnements](resource-manager-cross-resource-group-deployment.md)
### [Continuous Integration in Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md)

## Verwalten
### [Azure PowerShell](powershell-azure-resource-manager.md)
### [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
### [Azure-Portal](resource-group-portal.md)
### [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md)
### [Verschieben von Ressourcen in neue Gruppen oder Abonnements](resource-group-move-resources.md)
### [Erstellen von EA-Abonnements](programmatically-create-subscription.md)
### [Gewähren des Zugriffs zur Erstellung von EA-Abonnements](grant-access-to-create-subscription.md)
### [Erstellen von Verwaltungsgruppen](management-groups-create.md)
### [Verwalten Ihrer Verwaltungsgruppen](management-groups-manage.md)

## Steuern des Zugriffs
### Erstellen eines Dienstprinzipals
#### [Azure PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure-CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure-Portal](resource-group-create-service-principal-portal.md)
### [Authentifizierungs-API für den Zugriff auf Abonnements](resource-manager-api-authentication.md)
### [Sperren von Ressourcen](resource-group-lock-resources.md)

## Audit
### [Anzeigen von Aktivitätsprotokollen](resource-group-audit.md)
### [Bereitstellungsvorgänge anzeigen](resource-manager-deployment-operations.md)

## Problembehandlung
### [Behandeln von Bereitstellungsfehlern](resource-manager-common-deployment-errors.md)
### [AccountNameInvalid](resource-manager-storage-account-name-errors.md)
### [InvalidTemplate](resource-manager-invalid-template-errors.md)
### [Linux-Bereitstellungsprobleme](../virtual-machines/linux/troubleshoot-deploy-vm.md)
### [NoRegisteredProviderFound](resource-manager-register-provider-errors.md)
### [NotFound](resource-manager-not-found-errors.md)
### [ParentResourceNotFound](resource-manager-parent-resource-errors.md)
### [Bereitstellungs- und Zuteilungsprobleme bei Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)
### [Bereitstellungs- und Zuteilungsprobleme bei Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md)
### [RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
### [ReservedResourceName](resource-manager-reserved-resource-name.md)
### [ResourceQuotaExceeded](resource-manager-quota-errors.md)
### [SkuNotAvailable](resource-manager-sku-not-available-errors.md)
### [Windows-Bereitstellungsprobleme](../virtual-machines/windows/troubleshoot-deploy-vm.md)

# Verweis
## [Vorlagenformat](/azure/templates/)
## [Vorlagenfunktionen](resource-group-template-functions.md)
### [Array- und Objektfunktionen](resource-group-template-functions-array.md)
### [Vergleichsfunktionen](resource-group-template-functions-comparison.md)
### [Bereitstellen von Funktionen](resource-group-template-functions-deployment.md)
### [Logische Funktionen](resource-group-template-functions-logical.md)
### [Numerische Funktionen](resource-group-template-functions-numeric.md)
### [Ressourcenfunktionen](resource-group-template-functions-resource.md)
### [Zeichenfolgenfunktionen](resource-group-template-functions-string.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure-CLI](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# angeben
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Verwalten persönlicher Daten](resource-manager-personal-data.md)
## [Drosselungsanforderungen](resource-manager-request-limits.md)
## [Nachverfolgen asynchroner Vorgänge](resource-manager-async-operations.md)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
