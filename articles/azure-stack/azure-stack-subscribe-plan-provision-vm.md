---
title: Abonnieren von Angeboten in Azure Stack | Microsoft-Dokumentation
description: "Erstellen von Abonnements für Angebote in Azure Stack"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Erstellen von Abonnements für Angebote in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Nachdem Sie ein [Angebot erstellt](azure-stack-create-offer.md) haben, benötigen Benutzer ein Abonnement für dieses Angebot, damit sie es nutzen können.   
- Als Cloudbediener können Sie im Verwaltungsportal ein Abonnement für Benutzer erstellen.  Von Ihnen erstellte Abonnements können für öffentliche und private Angebote verwendet werden.
- Als Mandantenbenutzer können Sie über das Benutzerportal ein öffentliches Angebot abonnieren.  

In den folgenden Abschnitten erhalten Cloudbediener Informationen zum Erstellen von Abonnements für Benutzer, und Benutzer erfahren, wie sie ein Angebot abonnieren.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Erstellen eines Abonnements als Cloudbediener
Cloudbediener können im Verwaltungsportal ein Angebotsabonnement für Benutzer erstellen.  Sie können Abonnements für Mitglieder Ihres eigenen Verzeichnismandanten erstellen.  Wenn die [Mehrinstanzenfähigkeit](azure-stack-enable-multitenancy.md) aktiviert ist, können Sie auch Abonnements für Benutzer in zusätzlichen Verzeichnismandanten erstellen.

Sie können Abonnements für öffentliche und private Angebote erstellen.  Wenn Ihre Mandanten keine eigenen Abonnements erstellen sollen, legen Sie alle Ihre Angebote als privat fest, und erstellen Sie anschließend Abonnements für Ihre Mandanten. Dieser Ansatz kommt häufig beim Integrieren von Azure Stack in externe Abrechnungs- oder Dienstkatalogsysteme zum Einsatz.

Nachdem Sie ein Abonnement für einen Benutzer erstellt haben, kann sich dieser Benutzer beim Benutzerportal anmelden. Dort sieht er, dass ein Abonnement für das Angebot konfiguriert wurde.  

### <a name="to-create-the-subscription-for-a-user"></a>So erstellen Sie das Abonnement für einen Benutzer
1.  Navigieren Sie im Verwaltungsportal zu **Benutzerabonnements**.
2.  Klicken Sie auf **Hinzufügen**, um den Bereich **Neues Benutzerabonnement** zu öffnen. Hier geben Sie die folgenden Details an:  

  a. **Anzeigename**: Ein Anzeigename zum Identifizieren des Abonnements, der als *Name des Benutzerabonnements* angezeigt wird

  b. **Benutzer**: Geben Sie einen Benutzer aus einem verfügbaren Verzeichnismandanten für dieses Abonnement an. Der Benutzername wird als *Besitzer* angezeigt.  Das Format des Benutzernamens hängt von Ihrer Identitätslösung ab. Beispiel:    

     -  **Azure AD:** *&lt;user1>@&lt;contoso.onmicrosoft.com>*

     -   **AD FS:** *&lt;user1>@&lt;azurestack.local>*     

  c.    **Verzeichnismandant**: Wählen Sie den Verzeichnismandanten aus, zu dem das Benutzerkonto gehört. Wenn Sie Mehrinstanzenfähigkeit nicht aktiviert haben, ist nur der lokale Verzeichnismandant verfügbar.

3.  Klicken Sie auf **Angebot**, um den Bereich **Angebote** zu öffnen, und wählen Sie dann ein **Angebot** für dieses Abonnement aus. Da Sie ein Abonnement für einen Benutzer erstellen, können Sie ein privates oder ein öffentliches Angebot auswählen.

4.  Wählen Sie **Erstellen**, um das Abonnement zu erstellen. Im Bereich **Benutzerabonnements** wird nun das neue Abonnement angezeigt.  Wenn sich der Benutzer später beim Benutzerportal anmeldet, kann er Details zu diesem Abonnement anzeigen.

### <a name="to-make-an-add-on-plan-available"></a>So stellen Sie einen Add-On-Plan zur Verfügung  
Ein Cloudbediener kann jederzeit einen Add-On-Plan zu einem zuvor erstellten Abonnement hinzufügen:   
1.  Navigieren Sie im Verwaltungsportal zu **Weitere Dienste** > **Benutzerabonnements**, und wählen Sie dann das zu ändernde Abonnement aus.   

2.  Klicken Sie auf **Add-Ons** > **Hinzufügen**, um den Bereich **Plan hinzufügen** zu öffnen.  

3.  Wählen Sie den Plan aus, den Sie als Add-On hinzufügen möchten.  In der Konsole werden dann die dem Abonnement zugeordneten Pläne angezeigt.




## <a name="create-a-subscription-as-a-user"></a>Erstellen eines Abonnements als Benutzer
Als Benutzer können Sie sich beim Benutzerportal anmelden, um öffentliche Angebote und Add-On-Pläne aus dem Verzeichnismandanten (Organisation) zu suchen und zu abonnieren. Wenn die Azure Stack-Umgebung [Mehrinstanzenfähigkeit](azure-stack-enable-multitenancy.md) unterstützt, können Sie Angebote von einem Remoteverzeichnismandanten abonnieren.

### <a name="to-subscribe-to-an-offer"></a>So abonnieren Sie ein Angebot
1. [Melden Sie sich](azure-stack-connect-azure-stack.md) im Azure Stack-Benutzerportal (https://portal.local.azurestack.external) an, und klicken Sie auf **Abonnement erwerben**.

   ![Erwerben eines Abonnements](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. Geben Sie im Feld **Anzeigename** einen Namen für Ihr Abonnement ein, klicken Sie auf **Angebot**, klicken Sie im Bereich **Angebot wählen** auf ein Angebot, und klicken Sie dann auf **Erstellen**.

   ![Erstellen von Angeboten](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Klicken Sie zum Anzeigen des von Ihnen erstellten Abonnements auf **Weitere Dienste** und dann auf **Abonnements** und Ihr neues Abonnement.  

Aktualisieren Sie nach dem Abonnieren eines Angebots das Portal, um anzuzeigen, welche Dienste zum neuen Abonnement gehören.

### <a name="to-subscribe-to-an-add-on-plan"></a>So abonnieren Sie einen Add-On-Plan
Wenn ein Angebot einen Add-On-Plan enthält, können Sie diesen Plan jederzeit zu Ihrem Abonnement hinzufügen.  

1. Klicken Sie im Benutzerportal auf **Weitere Dienste** > **Abonnements**, und wählen Sie dann das zu ändernde Abonnement aus.

2. Klicken Sie auf die Schaltfläche **Plan hinzufügen**, und wählen Sie dann den gewünschten Add-On-Plan aus. Ist **Plan hinzufügen** nicht verfügbar, gibt es keine Add-On-Pläne für das mit diesem Abonnement verknüpfte Angebot.



## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
