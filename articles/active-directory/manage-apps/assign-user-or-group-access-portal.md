---
title: Assign a user or group to an enterprise app in Azure Active Directory | Microsoft Docs
description: Auswählen einer Unternehmens-App zum Zuweisen eines Benutzers oder einer Gruppe in der Azure Active Directory-Vorschau
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: barbkess
ms.reviewer: luleon
ms.openlocfilehash: 4d02adac41285ec4589c39eb61e7be48e960f92c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477055"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory
Zum Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App benötigen Sie die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App, und Sie müssen globaler Administrator für das Verzeichnis sein.

> [!NOTE]
> Informationen zu Lizenzanforderungen für die in diesem Artikel erläuterten Features finden Sie in der [Preisübersicht für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory).

> [!NOTE]
> Verwenden Sie für Microsoft-Anwendungen (etwa Office 365-Apps) PowerShell, um einer Unternehmens-App Benutzer hinzuzufügen.


## <a name="assign-a-user-to-an-app---portal"></a>Zuweisen eines Benutzers zu einer App – Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Alle Dienste** aus, geben Sie „Azure Active Directory“ in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Unternehmensanwendungen**.

    ![Öffnen von Unternehmens-Apps](./media/assign-user-or-group-access-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Dadurch werden die Apps aufgeführt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Option **Benutzer und Gruppen** aus.

    ![Auswählen des Befehls „Alle Anwendungen“](./media/assign-user-or-group-access-portal/select-app-users.png)
7. Wählen Sie auf dem Blatt ***App-Name*** **– Benutzer- und Gruppenzuweisung** den Befehl **Hinzufügen** aus.
8. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Zuweisen eines Benutzers oder einer Gruppe zur App](./media/assign-user-or-group-access-portal/assign-users.png)
9. Wählen Sie auf dem Blatt **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe aus der Liste aus, und klicken Sie auf die Schaltfläche **Auswählen** im unteren Bereich des Blatts.
10. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option **Rolle** aus. Wählen Sie dann auf dem Blatt **Rolle auswählen** eine Rolle aus, die auf die ausgewählten Benutzer oder Gruppen angewendet werden soll, und klicken Sie anschließend im unteren Bereich des Blatts auf **OK**.
11. Klicken Sie im unteren Bereich des Blatts **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**. Die zugewiesenen Benutzer oder Gruppen erhalten die Berechtigungen, die durch die ausgewählte Rolle für diese Unternehmens-App definiert werden.

## <a name="allow-all-users-to-access-an-app---portal"></a>Ermöglichen des App-Zugriffs für alle Benutzer – Portal
So ermöglichen Sie allen Benutzern den Zugriff auf eine Anwendung:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Alle Dienste** aus, geben Sie „Azure Active Directory“ in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Unternehmensanwendungen**.
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Dadurch werden die Apps aufgeführt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name***die Option **Eigenschaften** aus.
7. Legen Sie auf dem Blatt ***App-Name* – Eigenschaften** die Einstellung **Benutzerzuweisung erforderlich?** auf **Nein** fest. 

Für die Option **Benutzerzuweisung erforderlich?** gilt Folgendes:

- Sie hat keine Auswirkung darauf, ob eine Anwendung im Anwendungszugriffsbereich angezeigt wird. Wenn die Anwendung im Zugriffsbereich angezeigt werden soll, müssen Sie der Anwendung einen geeigneten Benutzer oder eine geeignete Gruppe zuweisen.
- Sie funktioniert nur mit Cloudanwendungen, die für SAML Single Sign-On konfiguriert sind, sowie mit lokalen Anwendungen, die mit einem Anwendungsproxy konfiguriert sind. Weitere Informationen finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](what-is-single-sign-on.md).
- Sie setzt eine Benutzereinwilligung für eine Anwendung voraus. Ein Administrator kann für alle Benutzer einwilligen.  Weitere Informationen finden Sie unter [Konfigurieren der Art der Benutzereinwilligung für eine Anwendung in Azure Active Directory](configure-user-consent.md).


## <a name="assign-a-user-to-an-app---powershell"></a>Zuweisen eines Benutzers zu einer App – PowerShell

1. Öffnen Sie eine Windows PowerShell-Eingabeaufforderung mit erhöhten Rechten.

    >[!NOTE] 
    > Sie müssen das AzureAD-Modul installieren (verwenden Sie den Befehl `Install-Module -Name AzureAD`). Wenn Sie zum Installieren eines NuGet-Moduls oder des neuen Azure Active Directory V2-PowerShell-Moduls aufgefordert werden, geben Sie „J“ ein, und drücken Sie die EINGABETASTE.

2. Führen Sie `Connect-AzureAD` aus, und melden Sie sich mit dem Benutzerkonto eines globalen Administrators an.
3. Weisen Sie mithilfe des folgenden Skripts einer Anwendung einen Benutzer und eine Rolle zu:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"
    
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```     

Weitere Informationen zum Zuweisen eines Benutzers zu einer Anwendungsrolle finden Sie in der Dokumentation zu [New-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

Wenn Sie eine Gruppe einer Unternehmens-App zuweisen möchten, müssen Sie `Get-AzureADUser` durch `Get-AzureADGroup` ersetzen.

### <a name="example"></a>Beispiel

In diesem Beispiel wird die Benutzerin Britta Simon mithilfe von PowerShell der Anwendung [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) zugewiesen.

1. Weisen Sie in PowerShell den Variablen „$username“, „$app_name“ und „$app_role_name“ die entsprechenden Werte zu. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. In diesem Beispiel kennen wir den genauen Namen der Anwendungsrolle nicht, die wir Britta Simon zuweisen möchten. Führen Sie die folgenden Befehle aus, um den Benutzer ($user) und den Dienstprinzipal ($sp) abzurufen. Verwenden Sie dabei die Anzeigenamen für den Benutzer-UPN und den Dienstprinzipal.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Führen Sie den Befehl `$sp.AppRoles` aus, um die für die Workplace Analytics-Anwendung verfügbaren Rollen anzuzeigen. In diesem Beispiel möchten wir Britta Simon die Rolle „Analyst (Limited access)“ (Analyst (Eingeschränkter Zugriff)) zuweisen.
    
    ![Workplace Analytics-Rolle](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

4. Weisen Sie den Rollennamen der Variablen `$app_role_name` zu.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

5. Führen Sie den folgenden Befehl aus, um den Benutzer der App-Rolle zuzuweisen:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](remove-user-or-group-access-portal.md)
* [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](disable-user-sign-in-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](change-name-or-logo-portal.md)
