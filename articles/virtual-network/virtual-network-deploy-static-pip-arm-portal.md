---
title: "Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse – Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie über das Azure-Portal einen virtuellen Computer mit einer statischen öffentlichen IP-Adresse erstellen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53e9b55a82e7750393dfb6c4818681028c5c4d2f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse über das Azure-Portal

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Erstellen einer VM mit einer statischen öffentlichen IP-Adresse

Führen Sie die folgenden Schritte aus, um eine VM mit einer statischen öffentlichen IP-Adresse im Azure-Portal zu erstellen:

1. Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie links oben im Portal auf **Ressource erstellen**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. Wählen Sie in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager**, und klicken Sie auf **Erstellen**.
4. Geben Sie im Bereich **Grundlagen** die VM-Informationen wie folgt ein, und klicken Sie dann auf **OK**.
   
    ![Azure-Portal – Grundlagen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. Klicken Sie im Bereich **Größe auswählen** wie folgt auf **A1 Standard**, und klicken Sie dann auf **Auswählen**.
   
    ![Azure-Portal – Größe auswählen](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. Klicken Sie im Bereich **Einstellungen** auf **Öffentliche IP-Adresse** und dann im Bereich **Öffentliche IP-Adresse erstellen** unter **Zuweisung** wie folgt auf **Statisch**. Klicken Sie anschließend auf **OK**.
   
    ![Azure-Portal – Öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. Klicken Sie im Bereich **Einstellungen** auf **OK**.
8. Überprüfen Sie den Bereich**Zusammenfassung** wie folgt, und klicken Sie dann auf **OK**.
   
    ![Azure-Portal – Öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Beachten Sie die neue Kachel in Ihrem Dashboard.
   
    ![Azure-Portal – Öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Nachdem der virtuelle Computer erstellt wurde, wird der Bereich **Einstellungen** folgendermaßen angezeigt:
    
    ![Azure-Portal – Öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

