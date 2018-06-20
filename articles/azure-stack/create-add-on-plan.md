---
title: Aktualisieren von Azure Stack-Angeboten und -Plänen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie vorhandene Azure Stack-Angebote und -Pläne anzeigen und ändern.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a84148a3ac31d51ff30cebffab00e5fec8fdaa87
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236951"
---
# <a name="azure-stack-add-on-plans"></a>Add-On-Pläne für Azure Stack
Als Azure Stack-Betreiber erstellen Sie Pläne mit den gewünschten Diensten und entsprechenden Kontingenten, die von Ihren Benutzern abonniert werden können. Diese [*Basispläne*](azure-stack-create-plan.md) enthalten die Kerndienste für Ihre Benutzer. Pro Angebot ist jeweils nur ein Basisplan zulässig. Wenn Sie Ihr Angebot ändern müssen, können Sie *Add-On-Pläne* verwenden. Hiermit können Sie den Plan ändern, um die Computer-, Speicher- oder Netzwerkkontingente zu erweitern, die im Rahmen des Basisplans ursprünglich angeboten wurden. 

Das Kombinieren aller Komponenten in einem einzelnen Plan kann in einigen Fällen zwar optimal sein, aber es kann ratsam sein, einen Basisplan zu verwenden und über Add-On-Pläne zusätzliche Dienste anzubieten. Beispielsweise können Sie die Entscheidung treffen, IaaS-Dienste im Rahmen eines Basisplans anzubieten und alle PaaS-Dienste als Add-On-Pläne zu behandeln. Außerdem können Pläne verwendet werden, um den Verbrauch von Ressourcen in Ihrer Azure Stack-Umgebung zu steuern. Wenn Sie beispielsweise erreichen möchten, dass Ihre Benutzer auf Ihre Ressourcennutzung achten, können Sie einen relativ kleinen Basisplan verwenden (je nach den erforderlichen Diensten). Sobald Benutzer die Kapazitätsgrenze erreichen, werden sie benachrichtigt, dass sie die Zuteilung der Ressourcen basierend auf dem zugewiesenen Plan bereits verbraucht haben. Die Benutzer können dann einen verfügbaren Add-On-Plan für zusätzliche Ressourcen auswählen. 

> [!NOTE]
> Wenn ein Benutzer einen Add-On-Plan einem vorhandenen Angebotsabonnement hinzufügt, kann es bis zu eine Stunde dauern, bis die zusätzlichen Ressourcen angezeigt werden. 

## <a name="create-an-add-on-plan"></a>Erstellen eines Add-On-Plans
Add-On-Pläne werden durch Ändern eines vorhandenen Angebots erstellt:

1. Melden Sie sich beim Azure Stack-Administratorportal als Cloudadministrator an.
2. Führen Sie die gleichen Schritte wie beim [Erstellen eines neuen Basisplans](azure-stack-create-plan.md) aus, um einen neuen Plan mit Diensten zu erstellen, die zuvor noch nicht angeboten wurden. In diesem Beispiel werden Key Vault-Dienste (Microsoft.KeyVault) in den neuen Plan einbezogen.
3. Klicken Sie im Administratorportal auf **Angebote**, und wählen Sie anschließend das Angebot aus, das mit einem Add-On-Plan aktualisiert werden soll.

   ![](media/create-add-on-plan/1.PNG)

4.  Scrollen Sie ans Ende der Angebotseigenschaften, und wählen Sie die Option **Add-On-Pläne**. Klicken Sie auf **Hinzufügen**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Wählen Sie den hinzuzufügenden Plan aus. In diesem Beispiel hat der Plan den Namen **Key vault plan** (Key Vault-Plan). Klicken Sie auf **Auswählen**, um den Plan dem Angebot hinzuzufügen. Sie sollten eine Benachrichtigung erhalten haben, dass der Plan dem Angebot erfolgreich hinzugefügt wurde.
   
    ![](media/create-add-on-plan/3.PNG)

6. Sehen Sie sich die Liste mit den Add-On-Plänen an, die im Angebot enthalten sind, um sicherzustellen, dass der neue Add-On-Plan aufgeführt ist.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Nächste Schritte
[Erstellen von Angeboten](azure-stack-create-offer.md)