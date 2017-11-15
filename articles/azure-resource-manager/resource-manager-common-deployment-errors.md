---
title: "Problembehandlung bei häufigen Azure-Bereitstellungsfehlern | Microsoft Docs"
description: "Informationen zum Beheben gängiger Fehler beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Bereitstellungsfehler, Azure-Bereitstellung, in Azure bereitstellen.
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2ebb469289afc36b08c90ae9839f5bdba41cd90b
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager

In diesem Artikel werden einige häufige Azure-Bereitstellungsfehler beschrieben, die ggf. auftreten können. Er enthält darüber hinaus Informationen zur Behebung der Fehler. Sie können unter [Ermitteln des Fehlercodes](#find-error-code) nachsehen, falls Sie den Fehlercode für Ihren Bereitstellungsfehler nicht kennen.

## <a name="error-codes"></a>Fehlercodes

| Fehlercode | Lösung | Weitere Informationen |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Beachten Sie die Einschränkungen bei der Namensgebung für Speicherkonten. | [Resolve errors for storage account names](resource-manager-storage-account-name-errors.md) (Beheben von Fehlern für Namen von Speicherkonten) |
| AccountPropertyCannotBeSet | Überprüfen Sie die verfügbaren Speicherkontoeigenschaften. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AnotherOperationInProgress | Warten Sie, bis der gleichzeitige Vorgang abgeschlossen ist. | |
| AuthorizationFailed | Ihr Konto oder Dienstprinzipal verfügt nicht über ausreichende Zugriffsberechtigungen zum Durchführen der Bereitstellung. Überprüfen Sie die Rolle, zu der Ihr Konto gehört, sowie deren Zugriffsberechtigungen für den Bereitstellungsumfang. | [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md) |
| BadRequest | Sie haben Bereitstellungswerte gesendet, die nicht den von Resource Manager erwarteten Werten entsprechen. Überprüfen Sie die interne Statusmeldung, um Hilfe zur Problembehandlung zu erhalten. | [Vorlagenreferenz](/azure/templates/) und [Unterstützte Speicherorte](resource-manager-template-location.md) |
| Konflikt: | Sie fordern einen Vorgang an, der im aktuellen Zustand der Ressource nicht zulässig ist. Eine Größenänderung für den Datenträger ist beispielsweise nur zulässig, wenn ein virtueller Computer erstellt wird oder die Zuweisung des virtuellen Computers aufgehoben wurde. | |
| DeploymentActive | Warten Sie, bis die gleichzeitige Bereitstellung für diese Ressourcengruppe abgeschlossen ist. | |
| DnsRecordInUse | Der Name des DNS-Eintrags muss eindeutig sein. Geben Sie entweder einen anderen Namen an, oder ändern Sie den vorhandenen Datensatz. | |
| ImageNotFound | Überprüfen Sie die Einstellungen für das VM-Image. | [Problembehandlung für Linux-Images](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) und [Problembehandlung für Windows-Images](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| InUseSubnetCannotBeDeleted | Dieser Fehler kann ggf. bei dem Versuch auftreten, eine Ressource zu aktualisieren, aber die Anforderung wird verarbeitet, indem die Ressource gelöscht und erstellt wird. Stellen Sie sicher, dass Sie alle unveränderten Werte angeben. | [Aktualisieren von Ressourcen](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Rufen Sie das Zugriffstoken für den entsprechenden Mandanten ab. Sie können das Token nur von dem Mandanten erhalten, zu dem Ihr Konto gehört. | |
| InvalidContentLink | Sie haben wahrscheinlich versucht, eine geschachtelte Vorlage zu verknüpfen, die nicht verfügbar ist. Überprüfen Sie den URI, den Sie für die geschachtelte Vorlage angegeben haben. Wenn die Vorlage in einem Speicherkonto vorhanden ist, stellen Sie sicher, dass auf den URI zugegriffen werden kann. Möglicherweise müssen Sie ein SAS-Token übergeben. | [Verknüpfte Vorlagen](resource-group-linked-templates.md) |
| InvalidParameter | Einer der Werte, die Sie für eine Ressource angegeben haben, stimmt nicht mit dem erwarteten Wert überein. Ursache für diesen Fehler können viele unterschiedliche Bedingungen sein. Beispielsweise kann ein Kennwort unzureichend oder ein Blobname fehlerhaft sein. Überprüfen Sie die Fehlermeldung, um zu ermitteln, welcher Wert korrigiert werden muss. | |
| InvalidRequestContent | Ihre Bereitstellungswerte enthalten entweder unerwartete Werte, oder es fehlen erforderliche Werte. Bestätigen Sie die Werte für Ihren Ressourcentyp. | [Vorlagenreferenz](/azure/templates/) |
| InvalidRequestFormat | Aktivieren Sie die Debugprotokollierung, wenn Sie die Bereitstellung durchführen, und überprüfen Sie den Inhalt der Anforderung. | [Debugprotokollierung](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | Überprüfen Sie den Ressourcennamespace, den Sie in der **type**-Eigenschaft angegeben haben. | [Vorlagenreferenz](/azure/templates/) |
| InvalidResourceReference | Die Ressource ist entweder noch nicht vorhanden, oder der Verweis darauf ist fehlerhaft. Überprüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. Überprüfen Sie, ob Ihre Verwendung der Funktion **reference** die für Ihr Szenario erforderlichen Parameter umfasst. | [Auflösen von Abhängigkeiten](resource-manager-not-found-errors.md) |
| InvalidResourceType | Überprüfen Sie den Ressourcentyp, den Sie in der **type**-Eigenschaft angegeben haben. | [Vorlagenreferenz](/azure/templates/) |
| InvalidTemplate | Überprüfen Sie die Vorlagensyntax auf Fehler. | [Beheben von Fehlern für eine ungültige Vorlage](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Überprüfen Sie, ob Ihr Konto zu demselben Mandanten wie die Ressourcengruppe gehört, für die Sie die Bereitstellung durchführen. | |
| LinkedInvalidPropertyId | Die Ressourcen-ID für eine Ressource wird nicht richtig aufgelöst. Überprüfen Sie, ob Sie alle erforderlichen Werte für die Ressourcen-ID angegeben haben, z.B. Abonnement-ID, Name der Ressourcengruppe, Ressourcentyp, Name der übergeordneten Ressource (falls erforderlich) und Ressourcenname. | |
| LocationRequired | Geben Sie einen Speicherort für die Ressource an. | [Standort festlegen](resource-manager-template-location.md) |
| MissingRegistrationForLocation | Überprüfen Sie den Registrierungsstatus des Ressourcenanbieters und die unterstützten Speicherorte. | [Lösen von Registrierungsfehlern](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Registrieren Sie Ihr Abonnement beim Ressourcenanbieter. | [Lösen von Registrierungsfehlern](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Überprüfen Sie den Registrierungsstatus des Ressourcenanbieters. | [Lösen von Registrierungsfehlern](resource-manager-register-provider-errors.md) |
| NotFound | Unter Umständen versuchen Sie, eine abhängige Ressource parallel zu einer übergeordneten Ressource bereitzustellen. Überprüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. | [Auflösen von Abhängigkeiten](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Bei der Bereitstellung wird versucht, einen Vorgang durchzuführen, bei dem das Kontingent für das Abonnement, die Ressourcengruppe oder die Region überschritten wird. Ändern Sie die Bereitstellung nach Möglichkeit so, dass die Kontingentwerte eingehalten werden. Erwägen Sie andernfalls, eine Änderung der Kontingente anzufordern. | [Beheben von Fehlern für Kontingente](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Stellen Sie sicher, dass eine übergeordnete Ressource vorhanden ist, bevor Sie die untergeordneten Ressourcen erstellen. | [Beheben von Fehlern für die übergeordnete Ressource](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | Die angegebene IP-Adresse enthält einen Adressbereich, der für Azure benötigt wird. Ändern Sie die IP-Adresse, um die Nutzung des reservierten Bereichs zu vermeiden. | [IP-Adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Die angegebene IP-Adresse liegt außerhalb des Subnetzbereichs. Ändern Sie die IP-Adresse so, das Sie innerhalb des Subnetzbereichs liegt. | [IP-Adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Einige Eigenschaften können auf einer bereitgestellten Ressource nicht geändert werden. Beschränken Sie Ihre Änderungen beim Aktualisieren einer Ressource auf die zulässigen Eigenschaften. | [Aktualisieren von Ressourcen](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Ihr Abonnement enthält eine Ressourcenrichtlinie, durch die eine Aktion verhindert wird, die Sie während der Bereitstellung ausführen möchten. Suchen Sie nach der Richtlinie, die die Aktion blockiert. Ändern Sie Ihre Bereitstellung nach Möglichkeit so, dass die Einschränkungen der Richtlinie beachtet werden. | [Beheben von Fehlern für Richtlinien](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Geben Sie einen Ressourcennamen an, der keinen reservierten Name enthält. | [Resolve reserved resource name errors](resource-manager-reserved-resource-name.md) (Beheben von Fehlern mit reservierten Ressourcennamen) |
| ResourceGroupBeingDeleted | Warten Sie, bis der Löschvorgang abgeschlossen ist. | |
| ResourceGroupNotFound | Überprüfen Sie den Namen der Zielressourcengruppe für die Bereitstellung. Er muss in Ihrem Abonnement bereits vorhanden sein. Überprüfen Sie Ihren Abonnementkontext. | [Azure CLI](/cli/azure/account?#az_account_set) [PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | Ihre Bereitstellung verweist auf eine Ressource, die nicht aufgelöst werden kann. Überprüfen Sie, ob Ihre Verwendung der Funktion **reference** die für Ihr Szenario erforderlichen Parameter enthält. | [Beheben von Fehlern für Verweise](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Bei der Bereitstellung wird versucht, Ressourcen zu erstellen, für die das Kontingent für das Abonnement, die Ressourcengruppe oder die Region überschritten wird. Ändern Sie Ihre Infrastruktur nach Möglichkeit so, dass die Kontingentvorgaben eingehalten werden. Erwägen Sie andernfalls, eine Änderung der Kontingente anzufordern. | [Beheben von Fehlern für Kontingente](resource-manager-quota-errors.md) |
| SkuNotAvailable | Wählen Sie die SKU (z.B. die VM-Größe) aus, die für den ausgewählten Speicherort verfügbar ist. | [Beheben von SKU-Fehlern](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Geben Sie einen eindeutigen Namen für das Speicherkonto an. | [Resolve errors for storage account names](resource-manager-storage-account-name-errors.md) (Beheben von Fehlern für Namen von Speicherkonten)  |
| StorageAccountAlreadyTaken | Geben Sie einen eindeutigen Namen für das Speicherkonto an. | [Resolve errors for storage account names](resource-manager-storage-account-name-errors.md) (Beheben von Fehlern für Namen von Speicherkonten) |
| StorageAccountNotFound | Überprüfen Sie das Abonnement, die Ressourcengruppe und den Namen des Speicherkontos, das Sie verwenden möchten. | |
| SubnetsNotInSameVnet | Ein virtueller Computer kann nur über ein virtuelles Netzwerk verfügen. Stellen Sie beim Bereitstellen von mehreren NICs sicher, dass diese demselben virtuellen Netzwerk angehören. | [Mehrere NICs](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>Ermitteln des Fehlercodes

Wenn während der Bereitstellung ein Fehler auftritt, gibt Resource Manager einen Fehlercode zurück. Sie können die Fehlermeldung über das Portal, PowerShell oder die Azure CLI anzeigen. Die äußere Fehlermeldung ist für die Problembehandlung ggf. zu allgemein. Sehen Sie sich die innere Meldung an, die ausführliche Informationen zum Fehler enthält. Weitere Informationen finden Sie unter [Bestimmen des Fehlercodes](resource-manager-troubleshoot-tips.md#determine-error-code).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](resource-manager-deployment-operations.md).
