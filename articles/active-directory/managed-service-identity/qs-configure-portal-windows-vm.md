---
title: 'Gewusst wie: Konfigurieren einer verwalteten Dienstidentität auf einer Azure-VM mit dem Azure-Portal'
description: Schritt-für-Schritt-Anweisungen zum Konfigurieren einer verwalteten Dienstidentität auf einer Azure-VM über das Azure-Portal.
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
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 552fce2ffd8b6bd786010da82e702ee98c3f8647
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888555"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Konfigurieren einer verwalteten VM-Dienstidentität über das Azure-Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie eine vom System und vom Benutzer zugewiesene Identität für einen virtuellen Azure-Computer über das Azure-Portal aktivieren und deaktivieren. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgende Rollenzuweisung:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) zum Aktivieren und Entfernen einer vom System zugewiesenen Identität von einer Azure-VM

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie eine vom System zugewiesene Identität für einen virtuellen Computer mit dem Azure-Portal aktivieren und deaktivieren.

### <a name="enable-system-assigned-identity-during-creation-of-a-vm"></a>Aktivieren einer vom System zugewiesenen Identität beim Erstellen eines virtuellen Computers

Derzeit unterstützt das Azure-Portal keine Aktivierung einer vom System zugewiesenen Identität während der Erstellung einer VM. Lesen Sie stattdessen die folgenden Schnellstartartikel, um zunächst einen virtuellen Computer zu erstellen, und fahren Sie dann mit dem nächsten Abschnitt mit Einzelheiten zum Aktivieren einer vom System zugewiesenen Identität zum virtuellen Computer fort:

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-identity-on-an-existing-vm"></a>Aktivieren einer vom System zugewiesenen Identität auf einem vorhandenen virtuellen Computer

So aktivieren Sie die vom System zugewiesene Identität auf einem virtuellen Computer, der ursprünglich ohne sie bereitgestellt wurde

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie **Identität** aus.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Ein** aus, und klicken Sie dann auf **Speichern**:

   ![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-identity-from-a-vm"></a>Entfernen einer vom System zugewiesenen Identität von einem virtuellen Computer

Bei einem virtuellen Computer, für den die vom System zugewiesene Identität nicht mehr benötigt wird:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. 

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie **Identität** aus.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Aus** aus, und klicken Sie dann auf **Speichern**:

   ![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

 In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Azure-Portals eine vom Benutzer zugewiesene Identität einer VM hinzufügen und von dieser entfernen.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Zuweisen einer vom Benutzer zugewiesenen Identität beim Erstellen eines virtuellen Computers

Derzeit unterstützt das Azure-Portal nicht das Zuweisen einer vom System zugewiesenen Identität während der Erstellung einer VM. Lesen Sie stattdessen die folgenden Schnellstartartikel, um zunächst einen virtuellen Computer zu erstellen, und fahren Sie dann mit dem nächsten Abschnitt mit Einzelheiten zum Zuweisen einer vom Benutzer zugewiesenen Identität zum virtuellen Computer fort:

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-identity-to-an-existing-vm"></a>Zuweisen einer vom Benutzer zugewiesenen Identität zu einem vorhandenen virtuellen Computer

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
2. Navigieren Sie zur gewünschten VM, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen** und dann auf **\+Hinzufügen**.

   ![Hinzufügen einer vom Benutzer zugewiesenen Identität zu einer VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Klicken Sie auf die vom Benutzer zugewiesene Identität, die Sie der VM hinzufügen möchten, und dann auf **Hinzufügen**.

    ![Hinzufügen einer vom Benutzer zugewiesenen Identität zu einer VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-vm"></a>Entfernen einer vom Benutzer zugewiesenen Identität von einer VM

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
2. Navigieren Sie zur gewünschten VM, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen**, den Namen der vom Benutzer zugewiesenen Identität, die Sie löschen möchten, und dann auf **Entfernen**. (Klicken Sie im Bestätigungsbereich auf **Ja**.)

   ![Entfernen einer vom Benutzer zugewiesenen Identität von einer VM](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="related-content"></a>Verwandte Inhalte

- Eine Übersicht über die verwaltete Dienstidentität finden Sie in dieser [Übersicht](overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der verwalteten Dienstidentität einer Azure-VM über das Azure-Portal [Zugriff auf eine andere Azure-Ressource](howto-assign-access-portal.md).

