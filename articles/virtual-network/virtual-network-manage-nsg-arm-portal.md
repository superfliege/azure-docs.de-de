---
title: NSGs mit der Azure-Portal verwalten | Microsoft Docs
description: "Erfahren Sie, wie vorhandene NSGs über das Azure Portal verwalten."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-nsgs-using-the-portal"></a>NSGs über das Portal verwalten

> [!div class="op_single_selector"]
> * [Verwaltungsportal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure-CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure gibt es zwei verschiedene Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcen-Manager und klassischen](../resource-manager-deployment-model.md). Dieser Artikel behandelt das Bereitstellungsmodell Ressourcen-Manager empfiehlt sich für die meisten neuen Bereitstellungen statt dem klassischen Bereitstellungsmodell verwenden.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Abrufen von Informationen
Sie können Ihre vorhandenen Netzwerksicherheitsgruppen anzeigen, Abrufen von Regeln für einem vorhandenen NSG und herausfinden, welche Ressourcen eine NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Zeigen Sie vorhandene Netzwerksicherheitsgruppen an.

Um alle vorhandenen NSGs in einem Abonnement anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie in einem Browser zum http://portal.azure.com und ggf. zur Registrierung mit Ihrem Azure-Konto.

2. Klicken Sie auf **Durchsuchen >** > **Netzwerksicherheitsgruppen**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Überprüfen Sie die Liste der Netzwerksicherheitsgruppen in der **Netzwerksicherheitsgruppen** Blatt.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Anzeigen von Netzwerksicherheitsgruppen in einer Ressourcengruppe

Zum Anzeigen der Liste der Netzwerksicherheitsgruppen in der **RG NSG** Ressource zu gruppieren, die folgenden Schritte ausführen:

1. Click **Resource groups >** > **RG-NSG** > **...** .

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. In der Liste der Ressourcen, Suchen nach Elementen, die der NSG-Symbol angezeigt, entsprechend der **Ressourcen** Blatt unten.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Liste aller Regeln für eine NSG

Zum Anzeigen der Regeln einer NSG, die mit dem Namen **NSG-Front-End-**, führen die folgenden Schritte aus:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.

2. In der **Einstellungen** auf **eingehende Sicherheitsregeln**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Die **eingehende Sicherheitsregeln** Blatt wird angezeigt, wie unten dargestellt.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. In der **Einstellungen** auf **ausgangssicherheitsregeln** der ausgehenden Regeln.

    > [!NOTE]
    > Standardregeln anzeigen möchten, klicken Sie auf die **Standard Regeln** Symbol am oberen Rand der Blade, in dem die Regeln angezeigt.
    >

### <a name="view-nsgs-associations"></a>NSGs Zuordnungen anzeigen

Welche Ressourcen anzeigen der **NSG-Front-End-** NSG zugeordnet ist, führen die folgenden Schritte aus:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.

2. In der **Einstellungen** auf **Subnetze** anzeigen, welche Subnetze, die der NSG zugeordnet sind.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. In der **Einstellungen** auf **Netzwerkschnittstellen** anzeigen, welche NICs, die der NSG zugeordnet sind.

## <a name="manage-rules"></a>Verwalten von Regeln
Sie können einer vorhandenen NSG Regeln hinzuzufügen, vorhandene Regeln bearbeiten und Entfernen von Regeln.

### <a name="add-a-rule"></a>Fügen Sie eine Regel hinzu.
Hinzufügen einer Regel zum gewähren **eingehende** -Datenverkehr an Port **443** auf jedem Computer, auf die **NSG-Front-End-** NSG, die folgenden Schritte ausführen:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.
2. In der **Einstellungen** auf **eingehende Sicherheitsregeln**.
3. In der **eingehende Sicherheitsregeln** Blatt, klicken Sie auf **hinzufügen**. Klicken Sie auf die **hinzuzfügen hinzufügen** Blade, füllen Sie die Werte, wie unten dargestellt, und klicken Sie dann auf **OK**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Beachten Sie nach ein paar Sekunden, die neue Regel in der **eingehende Sicherheitsregeln** Blatt.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Ändern Sie eine Regel
So ändern Sie die Regel zum Zulassen von eingehendem Datenverkehr von oben erstellten der **Internet** nur die folgenden Schritte ausführen:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.
2. In der **Einstellungen** Registerkarte, klicken Sie auf die oben erstellte Regel.
3. In der **zulassen Https** Blatt Änderung der **Quelle** Eigenschaft wie unten dargestellt, und klicken Sie dann auf **speichern**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Löschen einer Regel

Um die oben erstellte Regel zu löschen, müssen führen Sie die folgenden Schritte aus:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.
2. In der **Einstellungen** Registerkarte, klicken Sie auf die oben erstellte Regel.
3. In der **zulassen Https** Blatt, klicken Sie auf **löschen**, und klicken Sie dann auf **Ja**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Verwalten von Zuordnungen
Sie können eine NSG Subnetze und NICs zuordnen. Sie können auch eine NSG von anderen Ressourcen trennen, zu der Sie verbunden ist.

### <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG zu einem NIC
Zuordnen der **NSG-Front-End-** NSG zu den **TestNICWeb1** NIC, die folgenden Schritte ausführen:

1. Aus der **Netzwerksicherheitsgruppen** Blatt oder **Ressourcen** Blatt oben, klicken Sie auf **NSG-Front-End-**.
2. In der **Einstellungen** auf **Netzwerkschnittstellen** > **zuordnen** > **TestNICWeb1**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Trennen einer NSG aus einer Netzwerkkarte

Beim Trennen der **NSG-Front-End-** NSG aus der **TestNICWeb1** NIC, die folgenden Schritte ausführen:

1. Klicken Sie auf der Azure-Portal auf **Ressourcengruppen >** > **RG NSG** > **...**  > **TestNICWeb1**.

2. In der **TestNICWeb1** Blatt, klicken Sie auf **Sicherheit ändern...**  > **None**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> Dieses Blatt können auch die NIC an alle vorhandenen NSG zuordnen.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Trennen einer NSG aus einem Subnetz

Beim Trennen der **NSG-Front-End-** NSG aus der **Front-End-** Subnetz führen die folgenden Schritte aus:

1. Klicken Sie auf der Azure-Portal auf **Ressourcengruppen >** > **RG NSG** > **...**  > **TestVNet**.

2. In der **Einstellungen** Blatt, klicken Sie auf **Subnetze** > **Front-End-** > **Netzwerksicherheitsgruppe** > **keine**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. In der **Front-End-** Blatt, klicken Sie auf **speichern**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG zu einem Subnetz

Zuordnen der **NSG-Front-End-** NSG der **FronEnd** Subnetz erneut, die folgenden Schritte ausführen:

1. Klicken Sie auf der Azure-Portal auf **Ressourcengruppen >** > **RG NSG** > **...**  > **TestVNet**.
2. In der **Einstellungen** Blatt, klicken Sie auf **Subnetze** > **Front-End-** > **Netzwerksicherheitsgruppe** > **NSG-Front-End-**.
3. In der **Front-End-** Blatt, klicken Sie auf **speichern**.

> [!NOTE]
> Sie können auch eine NSG einem Subnetz zuordnen, von Thh NSG **Einstellungen** Blatt.
>

## <a name="delete-an-nsg"></a>Löschen einer NSG
Sie können eine NSG nur löschen, wenn diese nicht auf Ressourcen verknüpft ist. Zum Löschen einer NSG führen Sie die folgenden Schritte aus:.

1. Klicken Sie auf der Azure-Portal auf **Ressourcengruppen >** > **RG NSG** > **...**  > **NSG-FrontEnd**.
2. In der **Einstellungen** Blatt, klicken Sie auf **Netzwerkschnittstellen**.
3. Wenn alle NICs aufgeführt sind, klicken Sie auf der NIC, und führen Sie Schritt 2 in [trennen eine NSG aus einer NIC](#Dissociate-an-NSG-from-a-NIC).
4. Wiederholen Sie Schritt 3 für jede Netzwerkkarte.
5. In der **Einstellungen** Blatt, klicken Sie auf **Subnetze**.
6. Wenn keine Subnetze aufgeführt sind, klicken Sie auf das Subnetz, und führen Sie die Schritte 2 und 3 in [trennen eine NSG aus einem Subnetz](#Dissociate-an-NSG-from-a-subnet).
7. Führt einen Bildlauf nach links, um die **NSG-Front-End-** Blatt, klicken Sie dann auf **löschen** > **Ja**.

    ![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren Sie die Protokollierung](virtual-network-nsg-manage-log.md) für Netzwerksicherheitsgruppen.
