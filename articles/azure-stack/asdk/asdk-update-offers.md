---
title: Tutorial zur Aktualisierung von Azure Stack-Angeboten und -Plänen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie vorhandene Azure Stack-Angebote und -Pläne anzeigen und ändern.
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
ms.openlocfilehash: 3b5d2ab5e924f578f5838d11b0d2058aacf67dec
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-update-offers-and-plans"></a>Tutorial: Aktualisieren von Angeboten und Plänen
Als Azure Stack-Betreiber erstellen Sie Pläne mit den gewünschten Diensten und entsprechenden Kontingenten, die von Ihren Benutzern abonniert werden können. Diese *Basispläne* enthalten die Kerndienste für Ihre Benutzer. Es ist nur ein Basisplan pro Angebot möglich. Wenn Sie Ihr Angebot ändern müssen, können Sie *Add-On-Pläne* verwenden. Hiermit können Sie den Plan ändern, um die Computer-, Speicher- oder Netzwerkkontingente zu erweitern, die im Rahmen des Basisplans ursprünglich angeboten wurden. 

Das Kombinieren aller Komponenten in einem einzelnen Plan kann in einigen Fällen zwar optimal sein, aber es kann ratsam sein, einen Basisplan zu verwenden und über Add-On-Pläne zusätzliche Dienste anzubieten. Beispielsweise können Sie die Entscheidung treffen, IaaS-Dienste im Rahmen eines Basisplans anzubieten und alle PaaS-Dienste als Add-On-Pläne zu behandeln. Außerdem können Pläne verwendet werden, um den Verbrauch von Ressourcen in Ihrer eingeschränkten ASDK-Umgebung zu steuern. Wenn Sie beispielsweise erreichen möchten, dass Ihre Benutzer auf Ihre Ressourcennutzung achten, können Sie einen relativ kleinen Basisplan verwenden (je nach den erforderlichen Diensten). Sobald Benutzer die Kapazitätsgrenze erreichen, werden sie benachrichtigt, dass sie die Zuteilung der Ressourcen basierend auf dem zugewiesenen Plan bereits verbraucht haben. Die Benutzer können dann einen verfügbaren Add-On-Plan für zusätzliche Ressourcen auswählen. 

> [!NOTE]
> Wenn ein Benutzer einen Add-On-Plan einem vorhandenen Angebotsabonnement hinzufügt, kann es bis zu eine Stunde dauern, bis die zusätzlichen Ressourcen angezeigt werden. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Add-On-Plans 
> * Abonnieren des Add-On-Plans

## <a name="create-an-add-on-plan"></a>Erstellen eines Add-On-Plans
**Add-On-Pläne** werden erstellt, indem ein vorhandenes Angebot geändert wird.

1. Melden Sie sich am [Azure Stack-Portal](https://adminportal.local.azurestack.external) als Cloudadministrator an.
2. Führen Sie die gleichen Schritte wie zuvor aus, um einen [Basisplan zu erstellen](asdk-offer-services.md). Hier besteht das Ziel darin, einen neuen Plan mit Diensten zu erstellen, die zuvor noch nicht angeboten wurden. In diesem Beispiel werden Key Vault-Dienste (Microsoft.KeyVault) in den Plan einbezogen.
3. Klicken Sie im Administratorportal auf **Angebote**, und wählen Sie anschließend das Angebot aus, das mit einem Add-On-Plan aktualisiert werden soll.

   ![](media/asdk-update-offers/1.PNG)

4.  Scrollen Sie ans Ende der Angebotseigenschaften, und wählen Sie die Option **Add-On-Pläne**. Klicken Sie auf **Hinzufügen**.
   
    ![](media/asdk-update-offers/2.PNG)

5. Wählen Sie den hinzuzufügenden Plan aus. In diesem Beispiel hat der Plan den Namen **Key vault plan** (Key Vault-Plan). Klicken Sie auf **Auswählen**, um den Plan dem Angebot hinzuzufügen. Sie sollten eine Benachrichtigung erhalten haben, dass der Plan dem Angebot erfolgreich hinzugefügt wurde.
   
    ![](media/asdk-update-offers/3.PNG)

6. Sehen Sie sich die Liste mit den Add-On-Plänen an, die im Angebot enthalten sind, um sicherzustellen, dass der neue Add-On-Plan aufgeführt ist.
   
    ![](media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>Abonnieren des Add-On-Plans
Sie müssen sich am Azure Stack-Benutzerportal anmelden, um ein vorhandenes Azure Stack-Abonnement mit einem Add-On-Plan zu erweitern.

Führen Sie die folgenden Schritte aus, um zusätzliche Ressourcen zu ermitteln, die vom Azure Stack-Betreiber angeboten wurden, und fügen Sie einem Angebot, das Sie zuvor abonniert haben, einen Add-On-Plan hinzu.

1. Melden Sie sich am [Benutzerportal](https://portal.local.azurestack.external) an.
2. Klicken Sie zum Zugreifen auf das Abonnement, das mit dem Add-On-Plan erweitert werden soll, auf **Weitere Dienste** und dann auf **Abonnements**, und wählen Sie Ihr Abonnement aus.
   
    ![](media/asdk-update-offers/5.PNG)

3.  Klicken Sie in der Abonnementübersicht auf **Plan hinzufügen**.
   
    ![](media/asdk-update-offers/6.PNG)

4. Wählen Sie auf dem Blatt „Plan hinzufügen“ den Add-On-Plan aus, der dem Abonnement hinzugefügt werden soll. In diesem Beispiel wurde **Key vault plan** ausgewählt. Sie sollten eine Benachrichtigung erhalten, dass der Add-On-Plan erfolgreich beschafft wurde und dass Sie das Portal aktualisieren müssen, um auf das aktualisierte Abonnement zuzugreifen.
   
    ![](media/asdk-update-offers/7.PNG)

5. Prüfen Sie abschließend die Add-On-Pläne, die dem Abonnement zugeordnet sind, um sicherzustellen, dass das Hinzufügen des Add-On-Plans erfolgreich war.
   
    ![](media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Add-On-Plans 
> * Abonnieren des Add-On-Plans

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Azure-Computers über eine Vorlage](asdk-create-vm-template.md)

