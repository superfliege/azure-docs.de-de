---
title: 'Tutorial: Erstellen eines Azure Stack-Angebots | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie ein Azure Stack-Angebot mit Plänen und Kontingenten erstellen.
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
ms.openlocfilehash: 083b5e20b89f22cb8e523926858fe9ffb1441319
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-offer-azure-stack-iaas-services"></a>Tutorial: Anbieten von Azure Stack-IaaS-Diensten
Als Azure Stack-Cloudadministrator können Sie Angebote erstellen, die von Ihren Benutzern (ggf. auch als Mandanten bezeichnet) abonniert werden können. Mit ihrem Abonnement können Benutzer dann Azure Stack-Dienste nutzen.

In diesem Tutorial erfahren Sie, wie Sie ein Angebot erstellen, das es Benutzern ermöglicht, virtuelle Computer auf der Grundlage des Windows Server 2016 Datacenter-Image aus dem Azure Stack-Marketplace zu erstellen, das Sie im [vorherigen Tutorial](asdk-marketplace-item.md) hochgeladen haben.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen von Angeboten
> * Erstellen von Plänen
> * Festlegen von Kontingenten
> * Konfigurieren des Angebots als öffentliches Angebot

Dienste werden in Azure Stack mithilfe von Abonnements, Angeboten und Plänen für Benutzer bereitgestellt. Die Benutzer können mehrere Angebote abonnieren. Angebote können mehrere Pläne enthalten und die Pläne wiederum mehrere Dienste, wie im folgenden Diagramm zu sehen:

![Abonnements, Angebote und Pläne](media/asdk-offer-services/sop.png)

Als Azure Stack-Betreiber, der Dienste anbietet, werden Sie nacheinander zum Erstellen des Angebots, zum Erstellen eines Plans und zum Erstellen von Kontingenten aufgefordert. Nach der Angebotserstellung können Azure Stack-Benutzer Angebote über das Benutzerportal abonnieren.

## <a name="create-an-offer"></a>Erstellen von Angeboten
**Angebote** sind Gruppen mit mindestens einem Plan, die Azure Stack-Betreiber Benutzern zum Kauf oder als Abonnement anbieten können.

1. Melden Sie sich beim [Azure Stack-Portal](https://adminportal.local.azurestack.external) als Cloudadministrator an.

2. Klicken Sie auf **Neu** > **Angebote + Pläne** > **Angebot**.

   ![Neues Angebot](media/asdk-offer-services/new-offer.png)

2. Geben Sie im Abschnitt **Neues Angebot** die Werte für **Anzeigename** und **Ressourcenname** ein, und wählen Sie anschließend eine neue oder vorhandene **Ressourcengruppe** aus. Der Anzeigename ist der öffentliche Name des Angebots, der den Benutzern angezeigt wird. Der Ressourcenname ist nur für den Cloudbetreiber sichtbar und wird von Administratoren verwendet, um mit dem Angebot als Azure Resource Manager-Ressource zu arbeiten.

   ![Anzeigename](media/asdk-offer-services/offer-display-name.png)


## <a name="create-a-plan"></a>Erstellen von Plänen
Nach Eingabe der grundlegenden Angebotsinformationen müssen Sie dem Angebot mindestens einen Basisplan hinzufügen. 

Mithilfe von **Plänen** können Azure Stack-Betreiber Dienste und die dazugehörigen Kontingente gruppieren, um sie Benutzern anzubieten.

1. Klicken Sie auf **Basispläne** und im Abschnitt **Plan** auf **Hinzufügen**, um dem Angebot einen neuen Plan hinzuzufügen.

   ![Hinzufügen eines Plans](media/asdk-offer-services/new-plan.png)

2. Geben Sie im Abschnitt **Neuer Plan** Werte für die Optionen **Anzeigename** und **Ressourcenname** ein. Der Anzeigename ist der verständliche Name des Plans, der Benutzern angezeigt wird. Der Ressourcenname ist nur für den Cloudbetreiber sichtbar und wird von Cloudbetreibern verwendet, um mit dem Plan als Azure Resource Manager-Ressource zu arbeiten.

   ![Anzeigename des Plans](media/asdk-offer-services/plan-display-name.png)

3. Wählen Sie als Nächstes die Dienste aus, die in den Plan aufgenommen werden sollen. Wenn Sie IaaS-Dienste anbieten möchten, klicken Sie auf **Dienste**, wählen Sie **Microsoft.Compute**, **Microsoft.Network** und **Microsoft.Storage** aus, und klicken Sie anschließend auf **Auswählen**.

   ![Dienste des Plans](media/asdk-offer-services/select-services.png)


## <a name="set-quotas"></a>Festlegen von Kontingenten
Nachdem das Angebot nun über einen Plan mit Diensten verfügt, müssen Sie als Nächstes Kontingente für die einzelnen Dienste festlegen. 

**Kontingente** bestimmen die Menge von Ressourcen, die einem Benutzer für die einzelnen Dienste zur Verfügung stehen, die im angebotenen Plan enthalten sind.

1. Klicken Sie auf **Kontingente**, und wählen Sie dann den Dienst aus, für den Sie ein Kontingent erstellen möchten. 

   Erstellen Sie für den Anfang ein Kontingent für den Computedienst. Wählen Sie in der Liste mit den Namespaces den Namespace **Microsoft.Compute** aus, und klicken Sie anschließend auf **Neues Kontingent erstellen**.
   
   ![Erstellen eines neuen Kontingents](media/asdk-offer-services/create-quota.png)

2. Geben Sie im Abschnitt **Kontingent erstellen** einen Namen für das Kontingent ein, legen Sie die gewünschten Parameter für das Kontingent fest, und klicken Sie anschließend auf **OK**.

   ![Namen des Kontingents](media/asdk-offer-services/quota-properties.png)

3. Wählen Sie das Kontingent aus, das Sie für den Dienst **Microsoft.Compute** erstellt haben.

   ![Auswählen des Kontingents](media/asdk-offer-services/set-quota.png)

4. Wiederholen Sie die Schritte 1 bis 3, um Kontingente für den Netzwerk- und den Speicherdienst festzulegen, und klicken Sie dann im Abschnitt „Kontingente“ auf „OK“. Klicken Sie dann im Abschnitt **Neuer Plan** auf **OK**, um die Planeinrichtung abzuschließen. 

   ![Alle Kontingente festgelegt](media/asdk-offer-services/all-quotas-set.png)

5. Klicken Sie im Abschnitt „Plan“ auf **Erstellen**, um die Angebotserstellung abzuschließen. Nach erfolgreicher Angebotserstellung erhalten Sie eine entsprechende Benachrichtigung, und das Angebot wird als verfügbares Angebot aufgeführt.

   ![Angebot erstellt](media/asdk-offer-services/offer-complete.png)

## <a name="set-offer-to-public"></a>Konfigurieren des Angebots als öffentliches Angebot
Angebote müssen öffentlich sein, damit sie Benutzern angezeigt werden, die ein Angebot zum Abonnieren auswählen möchten. 

Angebote können Folgendes sein:
- **Öffentlich:** Für alle Benutzer sichtbar.
- **Privat:** Nur für die Cloudadministratoren sichtbar. Dies ist hilfreich beim Entwerfen des Plans oder Angebots oder wenn der Cloudadministrator jedes Abonnement für Benutzer erstellen möchte.
- **Außer Betrieb:**für neue Abonnenten geschlossen. Der Cloudadministrator kann mit dem Status „Außer Betrieb“ zukünftige Abonnements verhindern und derzeitige Abonnenten unverändert lassen.

> [!TIP]
> Änderungen am Angebot sind für Benutzer nicht sofort sichtbar. Um die Änderungen zu sehen, müssen sich Benutzer möglicherweise abmelden und erneut beim [Benutzerportal](https://portal.local.azurestack.external) anmelden, damit das neue Angebot angezeigt wird.

So konfigurieren Sie das neue Angebot als öffentliches Angebot: 

1. Klicken Sie im Dashboardmenü auf **Angebote** und dann auf das von Ihnen erstellte Angebot.

2. Klicken Sie auf **Status ändern** und dann auf **Öffentlich**.

   ![Status „Öffentlich“](media/asdk-offer-services/set-public.png)

3. Das Angebot ist nun im Azure Stack-Benutzerportal verfügbar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen von Angeboten
> * Erstellen von Plänen
> * Festlegen von Kontingenten
> * Konfigurieren des Angebots als öffentliches Angebot

Im nächsten Tutorial erfahren Sie, wie das soeben erstellte Angebot als Azure Stack-Benutzer abonnieren.

> [!div class="nextstepaction"]
> [Abonnieren von Angeboten](asdk-subscribe-services.md)