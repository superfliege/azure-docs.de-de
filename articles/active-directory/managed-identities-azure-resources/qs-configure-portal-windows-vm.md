---
title: Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer über das Azure-Portal
description: Ausführliche Anweisungen zum Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einer Azure-VM über das Azure-Portal
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
ms.openlocfilehash: b73a79676be559ad491bd7bb16691369dd8fa271
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158630"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer über das Azure-Portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie eine vom System und vom Benutzer zugewiesene verwaltete Identität für einen virtuellen Azure-Computer (VM) über das Azure-Portal aktivieren und deaktivieren. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Zuweisungen der rollenbasierten Azure-Zugriffssteuerung:

    > [!NOTE]
    > Es sind keine weiteren Azure AD-Verzeichnisrollenzuweisungen erforderlich.

    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) zum Aktivieren und Entfernen einer vom System zugewiesenen verwalteten Identität auf einer Azure-VM

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

In diesem Abschnitt erfahren Sie, wie Sie eine vom System zugewiesene verwaltete Identität für einen virtuellen Computer mit dem Azure-Portal aktivieren und deaktivieren.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Aktivieren einer vom System zugewiesenen verwalteten Identität beim Erstellen eines virtuellen Computers

Wenn Sie die systemseitig zugewiesene verwaltete Identität während der Erstellung eines virtuellen Computers aktivieren möchten, legen Sie auf der Registerkarte **Verwaltung** im Abschnitt **Identität** die Option **Verwaltete Dienstidentität** auf **Ein** fest.  

![Aktivieren einer systemseitig zugewiesenen Identität beim Erstellen eines virtuellen Computers](./media/msi-qs-configure-portal-windows-vm/enable-system-assigned-identity-vm-creation.png)

Informationen zum Erstellen eines virtuellen Computers finden Sie in den folgenden Schnellstartanleitungen: 

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) 
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)


### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Aktivieren einer vom System zugewiesenen verwalteten Identität auf einem vorhandenen virtuellen Computer

So aktivieren Sie die vom System zugewiesene verwaltete Identität auf einem virtuellen Computer, der ursprünglich ohne sie bereitgestellt wurde

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie **Identität** aus.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Ein** aus, und klicken Sie dann auf **Speichern**:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Entfernen einer vom System zugewiesenen verwalteten Identität von einem virtuellen Computer

Bei einem virtuellen Computer, für den die vom System zugewiesene verwaltete Identität nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. 

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie **Identität** aus.

3. Wählen Sie unter **Vom System zugewiesen**, **Status** die Option **Aus** aus, und klicken Sie dann auf **Speichern**:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

 In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Azure-Portals eine vom Benutzer zugewiesene verwaltete Identität einer VM hinzufügen und von dieser entfernen.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Zuweisen einer vom Benutzer zugewiesenen Identität beim Erstellen eines virtuellen Computers

Derzeit unterstützt das Azure-Portal nicht das Zuweisen einer vom System zugewiesenen verwalteten Identität während der Erstellung einer VM. Lesen Sie stattdessen die folgenden Schnellstartartikel, um zunächst einen virtuellen Computer zu erstellen, und fahren Sie dann mit dem nächsten Abschnitt mit Einzelheiten zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zum virtuellen Computer fort:

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einem vorhandenen virtuellen Computer

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
2. Navigieren Sie zur gewünschten VM, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen** und dann auf **\+Hinzufügen**.

   ![Hinzufügen einer vom Benutzer zugewiesenen verwalteten Identität zu einer VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Klicken Sie auf die vom Benutzer zugewiesene Identität, die Sie der VM hinzufügen möchten, und dann auf **Hinzufügen**.

    ![Hinzufügen einer vom Benutzer zugewiesenen verwalteten Identität zu einer VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer VM

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
2. Navigieren Sie zur gewünschten VM, und klicken Sie auf **Identität**, **Vom Benutzer zugewiesen**, den Namen der vom Benutzer zugewiesenen verwalteten Identität, die Sie löschen möchten, und dann auf **Entfernen**. (Klicken Sie im Bestätigungsbereich auf **Ja**.)

   ![Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer VM](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der verwalteten Identität einer Azure-VM über das Azure-Portal [Zugriff auf eine andere Azure-Ressource](howto-assign-access-portal.md).

