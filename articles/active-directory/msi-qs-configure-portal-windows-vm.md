---
title: "Konfigurieren von MSI auf einem virtuellen Azure-Computer über das Azure-Portal"
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer über das Azure-Portal."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: bryanla
ms.openlocfilehash: 169417530da21e0c8c58cbf770fd1d26660387f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) über das Azure-Portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Computer über das Azure-Portal aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

Zum Zeitpunkt der Erstellung dieses Dokuments wird das Aktivieren von MSI während der Erstellung eines virtuellen Computers im Azure-Portal nicht unterstützt. Verwenden Sie stattdessen einen der folgenden Schnellstartartikel zur Erstellung virtueller Computer, um erstmals einen virtuellen Computer zu erstellen:

- [Erstellen eines virtuellen Windows-Computers mit dem Azure-Portal](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Fahren Sie dann mit dem nächsten Abschnitt fort, um weitere Details zum Aktivieren von MSI auf dem virtuellen Computer zu erhalten.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

Gehen Sie für einen virtuellen Computer, der ursprünglich ohne MSI bereitgestellt wurde, wie folgt vor:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“.

2. Navigieren Sie zum gewünschten virtuellen Computer.

2. Klicken Sie auf die Seite „Konfiguration“, aktivieren Sie MSI für den virtuellen Computer, indem Sie unter „Verwaltete Dienstidenität“ die Option „Ja“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen auf dem virtuellen Computer erteilt, z. B. „Mitwirkender für virtuelle Computer“.

2. Navigieren Sie zum gewünschten virtuellen Computer.

3. Klicken Sie auf die Seite „Konfiguration“, entfernen Sie MSI von dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidenität“ die Option „Nein“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der MSI eines virtuellen Azure-Computers über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](msi-howto-assign-access-portal.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
