---
title: Erstellen eines Azure AD-Benutzerkontos
description: In diesem Artikel wird beschrieben, wie Sie Anmeldeinformationen für Azure AD-Benutzerkonten für Runbooks in Azure Automation zum Authentifizieren in Azure erstellen.
keywords: Azure Active Directory-Benutzer, Azure Service Management, Azure AD-Benutzerkonto
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 43490b8ec2139b5e9f62def614dc67e4274304c1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Authentifizieren von Runbooks mit klassischer Azure-Bereitstellung und Resource Manager-Bereitstellung
In diesem Artikel werden die Schritte beschrieben, die Sie zum Konfigurieren eines Azure AD-Benutzerkontos für Azure Automation-Runbooks ausführen müssen, die für Ressourcen des klassischen Azure-Bereitstellungsmodells oder Azure Resource Manager-Bereitstellungsmodells ausgeführt werden. Diese Authentifizierungsidentität wird für Azure Resource Manager-basierte Runbooks zwar weiterhin unterstützt, aber die empfohlene Methode ist die Verwendung eines ausführenden Azure-Kontos.       

## <a name="create-a-new-azure-active-directory-user"></a>Erstellen eines neuen Azure Active Directory-Benutzers
1. Melden Sie sich im Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer** > **Neuer Benutzer**.
3. Geben Sie Details zum Benutzer ein, beispielsweise **Name** und **Benutzername**.  
4. Notieren Sie sich den vollständigen Namen des Benutzers und das vorläufige Kennwort.
5. Klicken Sie auf **Verzeichnisrolle**.
6. Weisen Sie die Rolle „Globaler oder eingeschränkter Administrator“ zu.
7. Melden Sie sich bei Azure ab, und melden Sie sich anschließend mit den soeben erstellten Anmeldeinformationen wieder an. Sie werden aufgefordert, das Kennwort des Benutzers zu ändern.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Erstellen eines Automation-Kontos im Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um ein Azure Automation-Konto im Azure-Portal zu erstellen, das für Ihre Runbooks zum Verwalten von Ressourcen im Azure Resource Manager-Modus verwendet wird.  

1. Melden Sie sich im Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie die Option **Automation-Konten**.
3. Wählen Sie **Hinzufügen**.<br><br>![Automation-Konto hinzufügen](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement für das neue Konto sowie eine neue oder vorhandene **Ressourcengruppe** und den **Standort** eines Azure-Rechenzentrums an.
6. Wählen Sie für die Option **Ausführendes Azure-Konto erstellen** den Wert **Ja** aus, und klicken Sie auf die Schaltfläche **Erstellen**.  
   
    > [!NOTE]
    > Wenn Sie **Nein** auswählen und das ausführende Konto nicht erstellen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt. Das Konto wird zwar erstellt und der Rolle **Mitwirkender** des Abonnements zugewiesen, es verfügt aber über keine entsprechende Authentifizierungsidentität in Ihrem Abonnementverzeichnisdienst und somit auch nicht über Zugriffsressourcen in Ihrem Abonnement. So wird verhindert, dass Runbooks, die auf dieses Konto verweisen, sich authentifizieren und Aufgaben für Azure Resource Manager-Ressourcen durchführen können.
    > 
    >

    <br>![Automation-Konto hinzufügen – Warnung](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

Nachdem die Erstellung der Anmeldeinformationen abgeschlossen ist, müssen Sie ein Anmeldeinformationsobjekt erstellen, um das Automation-Konto dem zuvor erstellten AD-Benutzerkonto zuzuordnen. Beachten Sie, dass Sie das Automation-Konto bisher nur erstellt und noch keiner Authentifizierungsidentität zugeordnet haben. Führen Sie die Schritte im Artikel [Anmeldeinformationsobjekte in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) aus, und geben Sie den Wert für **Benutzername** im Format **Domäne\Benutzer** ein.

## <a name="use-the-credential-in-a-runbook"></a>Verwenden der Anmeldeinformationen in einem Runbook
Sie können die Anmeldeinformationen in einem Runbook mithilfe der Aktivität [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) abrufen und dann diese Informationen verwenden, um mit [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) eine Verbindung mit Ihrem Azure-Abonnement herzustellen. Wenn die Anmeldeinformationen mit einem Administrator für mehrere Azure-Abonnements verknüpft sind, sollten Sie mithilfe von [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) das richtige Abonnement angeben. Dies wird im folgenden PowerShell-Beispiel gezeigt, das typischerweise im oberen Bereich der meisten Azure Automation-Runbooks angezeigt wird.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Wiederholen Sie diese Zeilen nach jedem [Prüfpunkt](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) in Ihrem Runbook. Wenn das Runbook angehalten und später von einem anderen Benutzer fortgesetzt wird, muss die Authentifizierung erneut durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich im Artikel [Azure Automation-Runbooktypen](automation-runbook-types.md) über die unterschiedlichen Runbooktypen und die Schritte zum Erstellen Ihrer eigenen Runbooks.

