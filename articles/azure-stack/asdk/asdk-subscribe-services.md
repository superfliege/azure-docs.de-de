---
title: 'Tutorial: Abonnieren eines Azure Stack-Angebots | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie ein neues Abonnement für Azure Stack-Dienste erstellen und das Angebot mit einem erstellten virtuellen Testcomputer testen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Tutorial: Erstellen und Testen eines Abonnements
In diesem Tutorial erfahren Sie, wie Sie ein Abonnement mit einem Angebot erstellen und es anschließend testen. Für den Test melden Sie sich beim [Benutzerportal](https://portal.local.azurestack.external) als Cloudadministrator an, abonnieren das Angebot und erstellen dann einen virtuellen Computer.

> [!TIP]
> Für eine komplexere Evaluierung können Sie [ein Abonnement für einen bestimmten Benutzer erstellen](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) und sich dann als dieser Benutzer beim Benutzerportal anmelden. 

In diesem Tutorial wird gezeigt, wie Sie das Angebot aus dem [vorherigen Tutorial](asdk-offer-services.md) abonnieren.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Abonnieren von Angeboten 
> * Testen des Angebots

## <a name="subscribe-to-an-offer"></a>Abonnieren von Angeboten
Wenn Sie als Benutzer ein Angebot abonnieren möchten, müssen Sie sich beim Azure Stack-Benutzerportal anmelden, um die Dienste zu ermitteln, die vom Azure Stack-Betreiber angeboten werden.

1. Melden Sie sich beim [Benutzerportal](https://portal.local.azurestack.external) an, und klicken Sie auf **Abonnement erwerben**.

   ![Erwerben eines Abonnements](media/asdk-subscribe-services/get-subscription.png)

2. Geben Sie im Feld **Anzeigenamen** einen Namen für Ihr Abonnement ein. Klicken Sie auf **Angebot**, wählen Sie im Abschnitt **Angebot wählen** eines der verfügbaren Angebote aus, und klicken Sie anschließend auf **Erstellen**.

   ![Erstellen von Angeboten](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > Aktualisieren Sie nun das Benutzerportal, um mit der Verwendung Ihres Abonnements zu beginnen.

3. Klicken Sie zum Anzeigen des Abonnements, das Sie soeben erstellt haben, auf **Weitere Dienste**, auf **Abonnements** und anschließend auf Ihr neues Abonnement. Aktualisieren Sie nach dem Abonnieren eines Angebots das Portal, um zu sehen, ob das neue Abonnement neue Dienste beinhaltet. In diesem Beispiel wurde **Virtuelle Computer** hinzugefügt.

   ![Anzeigen des Abonnements](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testen des Angebots
Wenn Sie beim [Benutzerportal](https://portal.local.azurestack.external) angemeldet sind, können Sie das Angebot testen, indem Sie einen virtuellen Computer bereitstellen, der die Funktionen des neuen Abonnements verwendet. 

> [!NOTE]
> Für diesen Test wird das Windows Server 2016 Datacenter-VM-Image benötigt, das dem Azure Stack-Marketplace in einem [vorherigen Tutorial](asdk-marketplace-item.md) hinzugefügt wurde. 

1. Melden Sie sich beim [Benutzerportal](https://portal.local.azurestack.external) an.

2. Klicken Sie im Benutzerportal auf **Virtual Machines** > **Hinzufügen** > **Windows Server 2016 Datacenter** und anschließend auf **Erstellen**.

3. Geben Sie im Abschnitt **Grundlagen** einen **Namen**, einen **Benutzernamen** und ein **Kennwort** ein, wählen Sie ein **Abonnement** aus, erstellen Sie eine **Ressourcengruppe** (oder wählen Sie eine vorhandene Ressourcengruppe aus), und klicken Sie dann auf **OK**.

4. Klicken Sie im Abschnitt **Größe auswählen** auf **A1 Standard** und dann auf **Auswählen**.  

5. Behalten Sie auf dem Einstellungsblatt die Standardwerte bei, und klicken Sie auf **OK**.

6. Klicken Sie im Abschnitt **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.  

7. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Virtuelle Computer**, suchen Sie nach dem neuen virtuellen Computer, und klicken Sie auf den Namen.

    ![Alle Ressourcen](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> Die Bereitstellung des virtuellen Computers dauert einige Minuten.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Abonnieren von Angeboten 
> * Testen des Angebots

Im nächsten Tutorial erfahren Sie, wie Sie einen virtuellen Computer auf der Grundlage einer Vorlage erstellen.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Azure-Computers über eine Vorlage](asdk-create-vm-template.md)