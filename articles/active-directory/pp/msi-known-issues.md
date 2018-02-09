---
title: "FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory"
description: "Bekannte Probleme mit der verwalteten Dienstidentität für Azure Active Directory."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 8820691f5b7c6dbd2c15faede75de123f779b167
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="faq-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-msi-work-with-azure-cloud-services"></a>Funktioniert MSI mit Azure Cloud Services?

Nein, es ist keine Unterstützung von MSI in Azure Cloud Services geplant.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Funktioniert MSI mit ADAL (Active Directory Authentication Library) oder MSAL (Microsoft Authentication Library)?

Nein, MSI ist noch nicht in ADAL oder MSAL integriert. Ausführliche Informationen zum Anfordern eines MSI-Tokens mit dem MSI-REST-Endpunkt finden Sie unter [Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) eines virtuellen Azure-Computers für den Tokenabruf](msi-how-to-use-vm-msi-token.md).

### <a name="what-are-the-supported-linux-distributions"></a>Welche Linux-Distributionen werden unterstützt?

Die folgenden Linux-Distributionen unterstützen MSI: 

- CoreOS Stable
- CentOS 7.1
- RedHat 7.2
- Ubuntu 15.04

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

### <a name="are-there-rbac-roles-for-user-assigned-identities"></a>Gibt es RBAC-Rollen für vom Benutzer zugewiesene Identitäten?
Ja:
1. MSI-Mitwirkender: 

- Kann: CRUD-Aktionen für vom Benutzer zugewiesene Identitäten durchführen. 
- Kann nicht: Eine vom Benutzer zugewiesene Identität einer Ressource zuweisen (d.h. die Identität einer VM zuweisen).

2. MSI-Operator: 

- Kann: Eine vom Benutzer zugewiesene Identität einer Ressource zuweisen (d.h. die Identität einer VM zuweisen).
- Kann nicht: CRUD-Aktionen für vom Benutzer zugewiesene Identitäten durchführen.

Hinweis: Die integrierte Rolle „Mitwirkender“ kann alle der oben beschriebenen Aktionen ausführen: 
- CRUD-Aktionen für vom Benutzer zugewiesene Identitäten durchführen
- Eine vom Benutzer zugewiesene Identität einer Ressource zuweisen (d.h. die Identität einer VM zuweisen).


## <a name="known-issues"></a>Bekannte Probleme

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>Fehler in Verbindung mit dem „Automatisierungsskript“ bei dem Versuch, ein Schema für die MSI-Erweiterung zu exportieren

Wenn verwaltete Dienstidentität auf einem virtuellen Computer aktiviert ist, wird der folgende Fehler bei dem Versuch angezeigt, das Feature „Automatisierungsskript“ für den virtuellen Computer oder seine Ressourcengruppe zu verwenden:

![MSI-Automatisierungsskript-Exportfehler](~/articles/active-directory/media/msi-known-issues/automation-script-export-error.png)

Das Schema der VM-Erweiterung „verwaltete Dienstidentität“ kann derzeit nicht in eine Ressourcengruppenvorlage exportiert werden. Die generierte Vorlage weist daher keine Konfigurationsparameter auf, die das Aktivieren der verwalteten Dienstidentität für die Ressource ermöglichen. Diese Abschnitte können anhand der Beispiele in [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe einer Vorlage](msi-qs-configure-template-windows-vm.md) manuell hinzugefügt werden.

Wenn die Schemaexportfunktionalität für die MSI-VM-Erweiterung verfügbar ist, wird sie in [Exportieren von Ressourcengruppen, die VM-Erweiterungen enthalten](~/articles/virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions) aufgelistet.

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Das Blatt „Konfiguration“ wird nicht im Azure-Portal angezeigt

Wenn das Blatt „VM-Konfiguration“ auf Ihrem virtuellen Computer nicht angezeigt wird, wurde MSI in Ihrer Region noch nicht im Portal aktiviert.  Überprüfen Sie dies später erneut.  Sie können MSI für Ihren virtuellen Computer auch mit [PowerShell](msi-qs-configure-powershell-windows-vm.md) oder der [Azure-CLI](msi-qs-configure-cli-windows-vm.md) aktivieren.

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

## <a name="known-issues-with-user-assigned-msi-private-preview-feature"></a>Bekannte Probleme mit vom Benutzer zugewiesenen MSIs *(Feature der privaten Vorschauversion)*

- Die einzige Möglichkeit, alle vom Benutzer zugewiesenen MSIs zu entfernen, stellt das Aktivieren der vom System zugewiesenen MSI dar. 
- Die Bereitstellung der VM-Erweiterung kann auf einem virtuellen Computer möglicherweise aufgrund von Fehlern beim DNS-Lookup nicht ausgeführt werden. Starten Sie den virtuellen Computer neu, und wiederholen Sie den Vorgang. 
- Das Hinzufügen einer nicht vorhandenen MSI führt dazu, dass auf der VM ein Fehler auftritt. *Hinweis: Das Problem, dass bei fehlender MSI ein Fehler bei der Identitätszuweisung auftritt, wird derzeit behoben.*
- Das Tutorial für Azure Storage ist im Moment nur in der Region „USA, Mitte (EUAP)“ verfügbar. 
- Das Erstellen einer vom Benutzer zugewiesenen MSI mit Sonderzeichen (d.h. Unterstrich) im Namen wird nicht unterstützt.
- Beim Hinzufügen einer zweiten vom Benutzer zugewiesenen Identität kann die Client-ID möglicherweise keine Token für diese anfordern. Starten Sie zum Umgehen dieses Problems die MSI-VM-Erweiterung mit den folgenden beiden Bash-Befehlen neu:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Der VM-Agent unter Windows unterstützt zurzeit keine vom Benutzer zugewiesenen MSIs. 
- Das Zuweisen einer Rolle zu einer MSI für den Zugriff auf eine Ressource erfordert derzeit keine besonderen Berechtigungen. 
- Wenn ein virtueller Computer über eine vom Benutzer zugewiesene MSI, aber nicht über eine vom System zugewiesene MSI verfügt, wird die MSI auf der Portal-Benutzeroberfläche als aktiviert dargestellt. Um die vom System zugewiesene MSI zu aktivieren, verwenden Sie eine Azure Resource Manager-Vorlage, die Azure-Befehlszeilenschnittstelle oder ein SDK.
