---
title: Hinzufügen neuer Azure Stack-Mandantenkonten in Azure Active Directory | Microsoft-Dokumentation
description: Sie müssen nach dem Bereitstellen des Microsoft Azure Stack Development Kits mindestens ein Mandantenbenutzerkonto erstellen, damit Sie das Mandantenportal durchsuchen können.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 590426563936c66b1353f769be138759bb53f58c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
ms.locfileid: "29553204"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Hinzufügen neuer Azure Stack-Mandantenkonten in Azure Active Directory
Nach dem [Bereitstellen des Azure Stack Development Kits](azure-stack-run-powershell-script.md) benötigen Sie ein Mandantenbenutzerkonto, damit Sie das Portal durchsuchen und Ihre Angebote und Pläne testen können. Sie können ein Mandantenkonto mithilfe des [Azure-Portals](#create-an-azure-stack-tenant-account-using-the-azure-portal) oder mithilfe von [PowerShell](#create-an-azure-stack-tenant-account-using-powershell) erstellen.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Erstellen eines Azure Stack-Mandantenkontos mithilfe des Azure-Portals
Sie benötigen ein Azure-Abonnement, um das Azure-Portal verwenden zu können.

1. Melden Sie sich bei [Azure](https://portal.azure.com)an.
2. Klicken Sie in Microsoft Azure auf der Navigationsleiste links auf **Active Directory**.
3. Klicken Sie in der Verzeichnisliste auf das Verzeichnis, das Sie für Azure Stack verwenden möchten, oder erstellen Sie ein neues.
4. Klicken Sie auf dieser Verzeichnisseite auf **Benutzer**.
5. Klicken Sie auf **Benutzer hinzufügen**.
6. Wählen Sie im Assistenten zum **Hinzufügen von Benutzern** in der Liste **Art des Benutzers** die Option **Neuer Benutzer in Ihrem Unternehmen**.
7. Geben Sie im Feld **Benutzername** einen Namen für den Benutzer ein.
8. Wählen Sie im **@** den entsprechenden Eintrag aus.
9. Klicken Sie auf den Weiter-Pfeil.
10. Geben Sie auf der Seite **Benutzerprofil** des Assistenten **Vorname**, **Nachname** und **Anzeigename** ein.
11. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
12. Klicken Sie auf den Weiter-Pfeil.
13. Klicken Sie auf der Seite **Temporäres Kennwort abrufen** auf **Erstellen**.
14. Kopieren Sie das **neue Kennwort**.
15. Melden Sie sich bei Microsoft Azure mit dem neuen Konto an. Ändern Sie das Kennwort, wenn Sie dazu aufgefordert werden.
16. Melden Sie sich bei `https://portal.local.azurestack.external` mit dem neuen Konto an, um das Mandantenportal anzuzeigen.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Erstellen eines Azure Stack-Mandantenkontos mithilfe von PowerShell
Falls Sie kein Azure-Abonnement besitzen, können Sie ein Mandantenbenutzerkonto nicht mithilfe des Azure-Portals hinzufügen. In diesem Fall können Sie stattdessen das Azure Active Directory-Modul für Windows PowerShell verwenden.

> [!NOTE]
> Wenn Sie ein Microsoft-Konto (Live ID) zum Bereitstellen des Azure Stack Development Kits verwenden, können Sie ein Mandantenkonto nicht mithilfe von AAD PowerShell erstellen. 
> 
> 

1. Installieren Sie den [Microsoft Online Services-Anmelde-Assistenten für IT-Experten RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Installieren Sie das [Azure Active Directory-Modul für Windows PowerShell (64-Bit-Version)](http://go.microsoft.com/fwlink/p/?linkid=236297), und öffnen Sie es.
3. Führen Sie die folgenden Cmdlets aus:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Melden Sie sich mit dem neuen Konto bei Microsoft Azure an. Ändern Sie das Kennwort, wenn Sie dazu aufgefordert werden.
2. Melden Sie sich bei `https://portal.local.azurestack.external` mit dem neuen Konto an, um das Mandantenportal anzuzeigen.

