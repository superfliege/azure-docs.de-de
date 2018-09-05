---
title: Konfigurieren einer verwalteten Dienstidentität für eine Azure-VM-Skalierungsgruppe über das Azure-Portal
description: Schritt-für-Schritt-Anweisungen zum Konfigurieren einer verwalteten Dienstidentität für eine Azure-VMSS über das Azure-Portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: cbe2e3d9f60ced5c707ce5a701a5aac937ccc072
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887988"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Konfigurieren einer verwalteten Dienstidentität für eine VM-Skalierungsgruppe im Azure-Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie eine vom System und vom Benutzer zugewiesene Identität für eine VM-Skalierungsgruppe über das Azure-Portal aktivieren und deaktivieren.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgende Rollenzuweisung:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) zum Aktivieren und Entfernen einer vom System zugewiesenen verwalteten Identität auf einer VM-Skalierungsgruppe.

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität 

In diesem Abschnitt erfahren Sie, wie Sie die vom System zugewiesene Identität für eine VM-Skalierungsgruppe über das Azure-Portal aktivieren und deaktivieren.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität bei der Erstellung einer VM-Skalierungsgruppe

Derzeit unterstützt das Azure-Portal keine Aktivierung einer vom System zugewiesenen Identität während der Erstellung einer VM-Skalierungsgruppe. Lesen Sie stattdessen den folgenden Schnellstartartikel zu VM-Skalierungsgruppen, um zunächst eine VM-Skalierungsgruppe zu erstellen, und fahren Sie dann mit dem nächsten Abschnitt mit Einzelheiten zur Aktivierung einer vom System zugewiesenen Identität für eine VM-Skalierungsgruppe fort:

- [Erstellen einer VM-Skalierungsgruppe im Azure-Portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-identity-on-an-existing-virtual-machine-scale-set"></a>Aktivieren einer vom System zugewiesenen Identität in einer vorhandenen VM-Skalierungsgruppe

So aktivieren Sie eine vom System zugewiesene Identität in einer VM-Skalierungsgruppe, die ursprünglich ohne sie bereitgestellt wurde

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Ein** aus, und klicken Sie dann auf **Speichern**:

   [![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-identity-from-a-virtual-machine-scale-set"></a>Entfernen einer vom System zugewiesenen Identität aus einer VM-Skalierungsgruppe

Wenn die VM-Skalierungsgruppe keine vom System zugewiesene Identität mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen für die VM-Skalierungsgruppe erteilt.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Aus** aus, und klicken Sie dann auf **Speichern**:

   ![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie im Azure-Portal eine vom Benutzer zugewiesene Identität einer VM-Skalierungsgruppe hinzufügen und aus dieser entfernen.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Zuweisen einer benutzerzugewiesenen Identität bei der Erstellung einer VM-Skalierungsgruppe

Derzeit unterstützt das Azure-Portal nicht das Zuweisen einer vom System zugewiesenen Identität während der Erstellung einer VM-Skalierungsgruppe. Lesen Sie stattdessen den folgenden Schnellstartartikel zu VM-Skalierungsgruppen, um zunächst eine VM-Skalierungsgruppe zu erstellen, und fahren Sie dann mit dem nächsten Abschnitt mit Einzelheiten zur Zuweisung einer vom Benutzer zugewiesenen Identität fort:

- [Erstellen einer VM-Skalierungsgruppe im Azure-Portal](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-identity-to-an-existing-virtual-machine-scale-set"></a>Zuweisen einer vom Benutzer zugewiesenen Identität zu einer vorhandenen Azure VM-Skalierungsgruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.
2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen** und dann auf **\+Hinzufügen**.

   ![Hinzufügen einer vom Benutzer zugewiesenen Identität zu VM-Skalierungsgruppen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Klicken Sie auf die vom Benutzer zugewiesene Identität, die Sie der VM-Skalierungsgruppe hinzufügen möchten, und dann auf **Hinzufügen**.
   
   ![Hinzufügen einer vom Benutzer zugewiesenen Identität zu VM-Skalierungsgruppen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Entfernen einer benutzerzugewiesenen Identität aus einer VM-Skalierungsgruppe

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen**, den Namen der vom Benutzer zugewiesenen Identität, die Sie löschen möchten, und dann auf **Entfernen**. (Klicken Sie im Bestätigungsbereich auf **Ja**.)

   ![Entfernen einer vom Benutzer zugewiesenen Identität aus einer VM-Skalierungsgruppe](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="related-content"></a>Verwandte Inhalte

- Eine Übersicht über die verwaltete Dienstidentität finden Sie in dieser [Übersicht](overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der verwalteten Dienstidentität einer Azure-VM-Skalierungsgruppe über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](howto-assign-access-portal.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
