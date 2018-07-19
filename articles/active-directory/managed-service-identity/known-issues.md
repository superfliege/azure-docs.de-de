---
title: FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory
description: Bekannte Probleme mit der verwalteten Dienstidentität für Azure Active Directory.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: 05096050dfc29aebd2859b298eef884dcd9a1111
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906216"
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Häufig gestellte Fragen (FAQs)

### <a name="does-msi-work-with-azure-cloud-services"></a>Funktioniert MSI mit Azure Cloud Services?

Nein, es ist keine Unterstützung von MSI in Azure Cloud Services geplant.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Funktioniert MSI mit ADAL (Active Directory Authentication Library) oder MSAL (Microsoft Authentication Library)?

Nein, MSI ist noch nicht in ADAL oder MSAL integriert. Ausführliche Informationen zum Anfordern eines MSI-Tokens mit dem MSI-REST-Endpunkt finden Sie unter [Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) eines virtuellen Azure-Computers für den Tokenabruf](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>Was ist die Sicherheitsgrenze einer verwalteten Dienstidentität?

Bei der Sicherheitsgrenze der Identität handelt es sich um die Ressource, an die sie angefügt ist. Die Sicherheitsgrenze einer VM-MSI ist beispielsweise der virtuelle Computer. Jeglicher Code, der auf diesem virtuellen Computer ausgeführt wird, kann den MSI-Endpunkt aufrufen und Token anfordern. Ähnlich verhält es sich mit anderen Ressourcen, die MSI unterstützen.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>Sollte ich den MSI VM IMDS-Endpunkt oder den MSI VM-Erweiterungsendpunkt verwenden?

Wenn Sie MSI mit virtuellen Computern verwenden, empfehlen wir die Verwendung des MSI IMDS-Endpunkts. Der Azure Instance Metadata Service ist ein REST-Endpunkt, der für alle virtuellen IaaS-Computer verfügbar ist, die mit Azure Resource Manager erstellt wurden. Die Verwendung von MSI über IMDS ist z.B. mit den folgenden Vorteilen verbunden:

1. Alle von Azure IaaS unterstützten Betriebssysteme können MSI über IMDS verwenden. 
2. Es ist nicht mehr erforderlich, eine Erweiterung auf Ihrem virtuellen Computer zu installieren, um MSI zu aktivieren. 
3. Die von MSI verwendeten Zertifikate sind auf dem virtuellen Computer nicht mehr vorhanden. 
4. Der Endpunkt ist eine bekannte, nicht routingfähige IP-Adresse, auf die nur innerhalb des virtuellen Computers zugegriffen werden kann. 

Die MSI VM-Erweiterung ist weiterhin verfügbar und kann zurzeit noch verwendet werden. Allerdings wird der IMDS-Endpunkt in Zukunft Standard werden. Die MSI VM-Erweiterung wird bald als veraltet eingestuft. 

Weitere Informationen zum Azure Instance Metada Service finden Sie in der [IMDS-Dokumentation](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Welche Linux-Distributionen werden unterstützt?

Alle Linux-Distributionen, die von Azure IaaS unterstützt werden, können über den IMDS-Endpunkt mit MSI verwendet werden. 

Hinweis: Die MSI VM-Erweiterung unterstützt nur die folgenden Linux-Distributionen:
- CoreOS Stable
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Andere Linux-Distributionen werden zurzeit nicht unterstützt, und die Erweiterung kann für nicht unterstützte Distributionen zu einem Fehler führen.

Die Erweiterung funktioniert für CentOS 6.9. Aufgrund fehlender Systemunterstützung in Version 6.9 wird die Erweiterung allerdings nicht automatisch neu gestartet, wenn sie abgestürzt ist oder beendet wurde. Sie wird neu gestartet, wenn der virtuelle Computer neu gestartet wird. Wenn Sie die Erweiterung manuell neu starten möchten, lesen Sie [Wie wird die MSI-Erweiterung neu gestartet?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>Wie wird die MSI-Erweiterung neu gestartet?
Unter Windows und bestimmten Versionen von Linux kann das folgende Cmdlet verwendet werden, um die Erweiterung manuell neu zu starten, wenn sie beendet wurde:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Hinweis: 
- Der Erweiterungsname und Typ für Windows lautet: ManagedIdentityExtensionForWindows
- Der Erweiterungsname und Typ für Linux lautet: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bekannte Probleme

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>Fehler in Verbindung mit dem „Automatisierungsskript“ bei dem Versuch, ein Schema für die MSI-Erweiterung zu exportieren

Wenn verwaltete Dienstidentität auf einem virtuellen Computer aktiviert ist, wird der folgende Fehler bei dem Versuch angezeigt, das Feature „Automatisierungsskript“ für den virtuellen Computer oder seine Ressourcengruppe zu verwenden:

![MSI-Automatisierungsskript-Exportfehler](../media/msi-known-issues/automation-script-export-error.png)

Das Schema der VM-Erweiterung „verwaltete Dienstidentität“ kann derzeit nicht in eine Ressourcengruppenvorlage exportiert werden. Die generierte Vorlage weist daher keine Konfigurationsparameter auf, die das Aktivieren der verwalteten Dienstidentität für die Ressource ermöglichen. Diese Abschnitte können anhand der Beispiele in [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe einer Vorlage](qs-configure-template-windows-vm.md) manuell hinzugefügt werden.

Wenn die Schemaexportfunktionalität für die MSI-VM-Erweiterung verfügbar ist, wird sie in [Exportieren von Ressourcengruppen, die VM-Erweiterungen enthalten](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions) aufgelistet.

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Das Blatt „Konfiguration“ wird nicht im Azure-Portal angezeigt

Wenn das Blatt „VM-Konfiguration“ auf Ihrem virtuellen Computer nicht angezeigt wird, wurde MSI in Ihrer Region noch nicht im Portal aktiviert.  Überprüfen Sie dies später erneut.  Sie können MSI für Ihren virtuellen Computer auch mit [PowerShell](qs-configure-powershell-windows-vm.md) oder der [Azure-CLI](qs-configure-cli-windows-vm.md) aktivieren.

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Zuweisen des Zugriffs auf virtuelle Computer im Blatt „Zugriffssteuerung (IAM)“ ist nicht möglich

Wenn im Azure-Portal unter **Zugriffssteuerung (IAM)** > **Berechtigungen hinzufügen** unter **Zugriff zuweisen zu** die Option **Virtueller Computer** nicht angezeigt wird, wurde MSI in Ihrer Region noch nicht im Portal aktiviert. Überprüfen Sie dies später erneut.  Sie können MSI dennoch für die Rollenzuweisung auswählen, indem Sie nach dem MSI-Dienstprinzipal suchen.  Geben Sie den Namen des virtuellen Computers im Feld **Auswählen** ein, und der Dienstprinzipal wird im Suchergebnis angezeigt.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Der virtuelle Computer kann nicht gestartet werden, nachdem er aus der Ressourcengruppe oder dem Abonnement verschoben wurde

Wenn Sie einen virtuellen Computer verschieben, der sich im Ausführungsstatus befindet, wird er während des Verschiebevorgangs weiterhin ausgeführt. Wenn der virtuelle Computer nach dem Verschieben allerdings beendet und neu gestartet wird, schlägt der Start fehl. Die Ursache dieses Problems besteht darin, dass der virtuelle Computer den Verweis auf die MSI-Identität nicht aktualisiert und weiterhin auf die alte Ressourcengruppe verweist.

**Problemumgehung** 
 
Lösen Sie ein Update auf dem virtuellen Computer aus, damit die richtigen Werte für die MSI abgerufen werden können. Sie können eine VM-Eigenschaftsänderung vornehmen, um den Verweis auf die MSI-Identität zu aktualisieren. Beispielsweise können Sie mit dem folgenden Befehl einen neuen Tagwert auf dem virtuellen Computer festlegen:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Mit diesem Befehl wird das neue Tag „fixVM“ mit dem Wert 1 auf dem virtuellen Computer festgelegt. 
 
Durch das Festlegen dieser Eigenschaft wird der virtuelle Computer mit dem richtigen MSI-Ressourcen-URI aktualisiert. Anschließend sollte es möglich sein, den virtuellen Computer zu starten. 
 
Nachdem der virtuelle Computer gestartet wurde, kann das Tag mithilfe des folgenden Befehls entfernt werden:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Bekannte Probleme mit vom Benutzer zugewiesenen Identitäten

- Die Zuweisung von Identitäten, die vom Benutzer zugewiesen werden, ist nur für VMs und VMSS möglich. WICHTIG: An der Zuweisung von Identitäten, die vom Benutzer zugewiesen werden, werden in den kommenden Monaten Änderungen vorgenommen.
- Doppelte vom Benutzer zugewiesene Identitäten auf der gleichen VM/VMSS verursachen Fehler bei der VM/VMSS. Dies betrifft Identitäten, die mit abweichender Groß-/Kleinschreibung hinzugefügt werden. Beispiel: „MyUserAssignedIdentity“ vs. „myuserassignedidentity“. 
- Die Bereitstellung der VM-Erweiterung kann auf einem virtuellen Computer möglicherweise aufgrund von Fehlern beim DNS-Lookup nicht ausgeführt werden. Starten Sie den virtuellen Computer neu, und wiederholen Sie den Vorgang. 
- Das Hinzufügen einer nicht vorhandenen vom Benutzer zugewiesenen Identität führt dazu, dass auf der VM ein Fehler auftritt. 
- Das Erstellen einer vom Benutzer zugewiesenen Identität mit Sonderzeichen (d.h. Unterstrich) im Namen wird nicht unterstützt.
- Namen von Identitäten, die vom Benutzer zugewiesen werden, können in End-to-End-Szenarien max. 24 Zeichen enthalten. Vom Benutzer zugewiesene Identitäten, deren Namen mehr als 24 Zeichen enthalten, können nicht zugewiesen werden.
- Bei Verwendung der VM-Erweiterung für verwaltete Identität ist die Begrenzung auf Unterstützung von 32 vom Benutzer zugewiesene verwaltete Identitäten festgelegt. Ohne die VM-Erweiterung für verwaltete Identität ist die Begrenzung auf Unterstützung von 512 Identitäten festgelegt.  
- Beim Hinzufügen einer zweiten vom Benutzer zugewiesenen Identität kann die clientID möglicherweise keine Token für die VM-Erweiterung anfordern. Starten Sie zum Umgehen dieses Problems die MSI-VM-Erweiterung mit den folgenden beiden Bash-Befehlen neu:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Wenn eine VM über eine vom Benutzer zugewiesene Identität, aber nicht über eine vom System zugewiesene Identität verfügt, wird die MSI auf der Portalbenutzeroberfläche als aktiviert dargestellt. Um die vom System zugewiesene Identität zu aktivieren, verwenden Sie eine Azure Resource Manager-Vorlage, die Azure CLI oder ein SDK.
