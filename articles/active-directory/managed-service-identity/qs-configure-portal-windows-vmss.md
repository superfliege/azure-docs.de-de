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
ms.openlocfilehash: 93c532cf2864db28b580303ecefec8b6dbed65f6
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257758"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Konfigurieren einer verwalteten Dienstidentität für eine VM-Skalierungsgruppe im Azure-Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie die vom System zugewiesene Identität für eine VM-Skalierungsgruppe über das Azure-Portal aktivieren und deaktivieren. Das Zuweisen und Entfernen von vom Benutzer zugewiesenen Identitäten in einer Azure-VM-Skalierungsgruppe wird derzeit über das Azure-Portal nicht unterstützt.

> [!NOTE]
> Derzeit werden keine Vorgänge für vom Benutzer zugewiesene Identitäten über das Azure-Portal unterstützt. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgende Rollenzuweisung:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) zum Aktivieren und Entfernen einer vom System zugewiesenen verwalteten Identität auf einer VM-Skalierungsgruppe.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Verwaltete Dienstidentität während der Erstellung einer Azure-VM-Skalierungsgruppe

Derzeit werden bei der Erstellung von virtuellen Computern über das Azure-Portal keine Vorgänge zur verwalteten Dienstidentität unterstützt. Sehen Sie sich stattdessen die folgende Schnellstartanleitung zum Erstellen von Azure-VM-Skalierungsgruppen an, um zuerst eine Azure-VM-Skalierungsgruppe zu erstellen:

- [Erstellen einer VM-Skalierungsgruppe im Azure-Portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

Fahren Sie anschließend mit dem nächsten Abschnitt fort, um ausführlichere Informationen zur Aktivierung der verwalteten Dienstidentität auf der VM-Skalierungsgruppe zu erhalten.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Aktivieren der verwalteten Dienstidentität für eine vorhandene Azure-VM-Skalierungsgruppe

So aktivieren Sie die vom System zugewiesene Identität auf einem virtuellen Computer, der ursprünglich ohne sie bereitgestellt wurde

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Aktivieren Sie die vom System zugewiesene Identität auf dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidentität“ die Option „Ja“ auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   [![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Entfernen der verwalteten Dienstidentität aus einer Azure-VM-Skalierungsgruppe

Wenn die verwaltete Dienstidentität in einer VM-Skalierungsgruppe nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen für die VM-Skalierungsgruppe erteilt.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Deaktivieren Sie die vom System zugewiesene Identität auf dem virtuellen Computer, indem Sie unter „Verwaltete Dienstidentität“ die Option „Nein“ auswählen, und klicken Sie dann auf „Speichern“. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Verwandte Inhalte

- Eine Übersicht über die verwaltete Dienstidentität finden Sie in dieser [Übersicht](overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Gewähren Sie der verwalteten Dienstidentität einer Azure-VM-Skalierungsgruppe über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](howto-assign-access-portal.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
