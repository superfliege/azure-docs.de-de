---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer über das Azure-Portal
description: Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer über das Azure-Portal.
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
ms.openlocfilehash: 9d9c18d576f3975d4c8272efff7161366f3f53fe
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901356"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie die vom System zugewiesene Identität für eine Azure-VM über das Azure-Portal aktivieren und deaktivieren. Das Zuweisen und Entfernen von vom Benutzer zugewiesenen Identitäten für eine Azure-VM wird derzeit über das Azure-Portal nicht unterstützt.

> [!NOTE]
> Derzeit werden keine Vorgänge für vom Benutzer zugewiesene Identitäten über das Azure-Portal unterstützt. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Verwaltete Dienstidentität während der Erstellung einer Azure-VM

Derzeit werden bei der Erstellung von virtuellen Computern über das Azure-Portal keine Vorgänge zur verwalteten Dienstidentität unterstützt. Verwenden Sie stattdessen einen der folgenden Schnellstartartikel zur Erstellung virtueller Computer, um erstmals einen virtuellen Computer zu erstellen:

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Fahren Sie anschließend mit dem nächsten Abschnitt fort, um ausführlichere Informationen zur Aktivierung der verwalteten Dienstidentität auf dem virtuellen Computer zu erhalten.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Aktivieren der verwalteten Dienstidentität für eine vorhandene Azure-VM

So aktivieren Sie die vom System zugewiesene Identität auf einem virtuellen Computer, der ursprünglich ohne sie bereitgestellt wurde

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“.

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie die Seite „Konfiguration“.

3. Aktivieren Sie die vom System zugewiesene Identität auf dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidentität“ die Option „Ja“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

    > [!NOTE]
    > Das Hinzufügen einer vom Benutzer zugewiesenen Identität zu einem virtuellen Computer über das Azure-Portal wird derzeit nicht unterstützt.

   ![Screenshot der Konfigurationsseite](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Entfernen der verwalteten Dienstidentität von einer Azure-VM

Bei einem virtuellen Computer, für den die vom System zugewiesene Identität nicht mehr benötigt wird:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“.

2. Navigieren Sie zum gewünschten virtuellen Computer, und wählen Sie die Seite „Konfiguration“.

3. Deaktivieren Sie die vom System zugewiesene Identität auf dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidentität“ die Option „Nein“ auswählen, und klicken Sie dann auf „Speichern“. Dieser Vorgang kann 60 Sekunden oder länger dauern:

    > [!NOTE]
    > Das Hinzufügen einer vom Benutzer zugewiesenen Identität zu einem virtuellen Computer über das Azure-Portal wird derzeit nicht unterstützt.

   ![Screenshot der Konfigurationsseite](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Verwandte Inhalte

- Eine Übersicht über die verwaltete Dienstidentität finden Sie in dieser [Übersicht](overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](howto-assign-access-portal.md).

