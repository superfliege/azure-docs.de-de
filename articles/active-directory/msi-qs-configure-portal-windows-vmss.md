---
title: Konfigurieren einer MSI in einer Azure-VM-Skalierungsgruppe mit dem Azure-Portal
description: "Schritt-für-Schritt-Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) für eine Azure-VMSS über das Azure-Portal."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 4808165aa760be7e397bd3601e958419780e0004
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="configure-an-azure-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Konfigurieren einer MSI (Managed Service Identity, verwaltete Dienstidentität) für eine Azure-VM-Skalierungsgruppe über das Azure-Portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie eine MSI für eine Azure-VM-Skalierungsgruppe über das Azure-Portal aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-azure-virtual-machine-scale-set"></a>Aktivieren der MSI während der Erstellung einer Azure-VM-Skalierungsgruppe

Zum Zeitpunkt der Erstellung dieses Dokuments wird das Aktivieren der MSI während der Erstellung einer VM-Skalierungsgruppe im Azure-Portal nicht unterstützt. Sehen Sie sich stattdessen die folgende Schnellstartanleitung zum Erstellen von Azure-VM-Skalierungsgruppen an, um zuerst eine Azure-VM-Skalierungsgruppe zu erstellen:

- [Erstellen einer VM-Skalierungsgruppe im Azure-Portal](../virtual-machine-scale-sets/virtual-machine-scale-sets-create-portal.md)  

Fahren Sie dann mit dem nächsten Abschnitt fort, um weitere Details zum Aktivieren der MSI in der VM-Skalierungsgruppe zu erhalten.

## <a name="enable-msi-on-an-existing-azure-vmms"></a>Aktivieren der MSI in einer vorhandenen Azure-VMMS

Gehen Sie für eine VM-Skalierungsgruppe, die ursprünglich ohne MSI bereitgestellt wurde, wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Klicken Sie auf die Seite **Konfiguration**, aktivieren Sie die MSI für die VM-Skalierungsgruppe, indem Sie unter „Verwaltete Dienstidentität“ die Option **Ja** auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Entfernen einer MSI aus einer Azure-VM-Skalierungsgruppe

Wenn die MSI in einer VM-Skalierungsgruppe nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, die Ihnen Schreibberechtigungen für die VM-Skalierungsgruppe erteilt.

2. Navigieren Sie zur gewünschten VM-Skalierungsgruppe.

3. Klicken Sie auf die Seite **Konfiguration**, entfernen Sie die MSI aus der VM-Skalierungsgruppe, indem Sie unter **Verwaltete Dienstidentität** die Option **Nein** auswählen, und klicken Sie dann auf **Speichern**. Dieser Vorgang kann 60 Sekunden oder länger dauern:

   ![Screenshot der Konfigurationsseite](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Gewähren Sie der MSI einer Azure-VM-Skalierungsgruppe über das Azure-Portal den [Zugriff auf eine andere Azure-Ressource](msi-howto-assign-access-portal.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.
