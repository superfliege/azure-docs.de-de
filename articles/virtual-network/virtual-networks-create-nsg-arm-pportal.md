---
title: Erstellen einer Netzwerksicherheitsgruppe – Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Netzwerksicherheitsgruppen über das Azure-Portal erstellen und bereitstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a66de0b0239fef12168733eca7af85c8b08f82
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-network-security-group-using-the-azure-portal"></a>Erstellen einer Netzwerksicherheitsgruppe (NSG) mithilfe des Azure-Portals

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie haben auch die Möglichkeit, [NSGs im klassischen Bereitstellungsmodell zu erstellen](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]


## <a name="create-the-nsg-frontend-nsg"></a>Erstellen der Netzwerksicherheitsgruppe „NSG-FrontEnd“
Führen Sie die nachstehenden Schritte aus, um die Netzwerksicherheitsgruppe **NSG-FrontEnd** wie im Szenario gezeigt zu erstellen:

1. Navigieren Sie in einem Browser zu https://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **+ Ressource erstellen >** > **Netzwerksicherheitsgruppen**.
   
    ![Azure-Portal – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. Klicken Sie unter **Netzwerksicherheitsgruppen** auf **Hinzufügen**.
   
    ![Azure-Portal – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. Erstellen Sie unter **Netzwerksicherheitsgruppe erstellen** in der Ressourcengruppe *RG-NSG* eine NSG mit dem Namen *NSG-FrontEnd*, und klicken Sie dann auf **Erstellen**.
   
    ![Azure-Portal – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Erstellen von Regeln in einer vorhandenen NSG
Führen Sie die folgenden Schritte aus, um im Azure-Portal Regeln in einer vorhandenen NSG zu erstellen:

1. Klicken Sie auf **Alle Dienste**, und suchen Sie dann nach **Netzwerksicherheitsgruppen**. Wenn **Netzwerksicherheitsgruppen** angezeigt wird, wählen Sie den Eintrag aus.
2. Klicken Sie in der Liste der NSGs auf **NSG-FrontEnd** > **Eingangssicherheitsregeln**.
   
    ![Azure-Portal – NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. Klicken Sie in der Liste **Eingangssicherheitsregeln** auf **Hinzufügen**.
   
    ![Azure-Portal – Regel hinzufügen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. Erstellen Sie unter **Eingangssicherheitsregel hinzufügen** eine Regel mit dem Namen *web-rule* und der Priorität *200*, und lassen Sie den Zugriff über *TCP* an Port *80* auf alle virtuellen Computer von allen Quellen zu. Klicken Sie dann auf **OK**. Beachten Sie, dass die meisten dieser Einstellungen bereits Standardwerte sind.
   
    ![Azure-Portal – Regeleinstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Nach einigen Sekunden wird die neue Regel in der NSG angezeigt.
   
    ![Azure-Portal – Neue Regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Wiederholen Sie die Schritte bis Schritt 6, um eine eingehende Regel namens *RDP-Regel* mit einer Priorität von *250* zu erstellen, und erlauben Sie damit den Zugriff auf einen beliebigen virtuellen Computer per *TCP* auf Port *3389* von einer beliebigen Quelle aus.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Zuordnen der NSG zum FrontEnd-Subnetz

1. Klicken Sie auf **Alle Dienste >**, geben Sie **Ressourcengruppen** ein, und wählen Sie **Ressourcengruppen** aus, wenn das Ergebnis angezeigt wird. Klicken Sie dann auf **RG-NSG**.
2. Klicken Sie unter **RG-NSG** auf **...** > **TestVNet**.
   
    ![Azure-Portal – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. Klicken Sie unter **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerksicherheitsgruppe** > **NSG-FrontEnd**.
   
    ![Azure-Portal – Subnetzeinstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. Klicken Sie auf dem Blatt **FrontEnd** auf **Speichern**.
   
    ![Azure-Portal – Subnetzeinstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Erstellen der Netzwerksicherheitsgruppe „NSG-BackEnd“
Führen Sie die folgenden Schritte aus, um die Netzwerksicherheitsgruppe **NSG-BackEnd** zu erstellen und sie dem Subnetz **BackEnd** zuzuordnen:

1. Wiederholen Sie die Schritte unter *Erstellen der Netzwerksicherheitsgruppe „NSG-FrontEnd“*, um eine NSG namens [NSG-BackEnd](#Create-the-NSG-FrontEnd-NSG) zu erstellen.
2. Wiederholen Sie die Schritte unter **Erstellen von Regeln in einer vorhandenen NSG**, um die [Regeln für eingehenden Datenverkehr](#Create-rules-in-an-existing-NSG) in der folgenden Tabelle zu erstellen.
   
   | Eingehende Regel | Ausgehende Regel |
   | --- | --- |
   | ![Azure-Portal – Eingehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure-Portal – Ausgehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Wiederholen Sie die Schritte unter **Zuordnen der NSG zum FrontEnd-Subnetz**, um die NSG **NSG-BackEnd** dem Subnetz [BackEnd](#Associate-the-NSG-to-the-FrontEnd-subnet) zuzuordnen.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [vorhandene NSGs verwalten](manage-network-security-group.md)
* [Aktivieren Sie die Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.