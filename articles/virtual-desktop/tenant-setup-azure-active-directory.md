---
title: Erstellen eines Mandanten in Windows Virtual Desktop (Vorschauversion) – Azure
description: Es wird beschrieben, wie Sie in Azure Active Directory Windows Virtual Desktop-Mandanten (Vorschauversion) einrichten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 1c66b3de9e18cb74c43f20499e4065c7ec7ae5ca
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58801678"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Tutorial: Erstellen eines Mandanten in Windows Virtual Desktop (Vorschauversion)

Die Erstellung eines Mandanten in Windows Virtual Desktop (Vorschauversion) ist der erste Schritt zur Entwicklung Ihrer Lösung für die Desktopvirtualisierung. Ein Mandant umfasst eine Gruppe mit mindestens einem Hostpool. Jeder Hostpool besteht aus mehreren Sitzungshosts, die als virtuelle Computer in Azure ausgeführt und für den Windows Virtual Desktop-Dienst registriert werden. Darüber hinaus umfasst jeder Hostpool mindestens eine App-Gruppe, die verwendet wird, um Remotedesktop- und Remoteanwendungsressourcen für Benutzer zu veröffentlichen. Mit einem Mandanten können Sie Hostpools und App-Gruppen erstellen, Benutzer zuweisen und Verbindungen über den Dienst herstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Gewähren von Azure Active Directory-Berechtigungen für den Windows Virtual Desktop-Dienst
> * Zuweisen der Anwendungsrolle „TenantCreator“ für einen Benutzer in Ihrem Azure Active Directory-Mandanten
> * Erstellen eines Windows Virtual Desktop-Mandanten

Sie benötigen Folgendes, um Ihren Windows Virtual Desktop-Mandanten einzurichten:

* Die Mandanten-ID von [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für Windows Virtual Desktop-Benutzer.
* Ein globales Administratorkonto im Azure Active Directory-Mandanten.
   * Dies gilt auch für CSP-Organisationen (Cloud Solution Provider), die einen Windows Virtual Desktop-Mandanten für ihre Kunden erstellen. Wenn Sie eine CSP-Organisation sind, müssen Sie sich als globaler Administrator der Azure Active Directory-Instanz des Kunden anmelden können.
   * Das Administratorkonto muss aus dem Azure Active Directory-Mandanten stammen, in dem Sie den Windows Virtual Desktop-Mandanten erstellen möchten. Azure Active Directory B2B-Konten (Gastkonten) werden bei diesem Prozess nicht unterstützt.
   * Das Administratorkonto muss ein Geschäfts-, Schul- oder Unikonto sein.
* Azure-Abonnement-ID

## <a name="grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service"></a>Gewähren von Azure Active Directory-Berechtigungen für den Windows Virtual Desktop-Dienst (Vorschauversion)

Überspringen Sie diesen Abschnitt, falls Sie für diese Azure Active Directory-Instanz bereits Berechtigungen für Windows Virtual Desktop gewährt haben.

Durch das Gewähren von Berechtigungen für den Windows Virtual Desktop-Dienst kann dieser Ihre Azure Active Directory-Instanz zur Erledigung von Verwaltungs- und Endbenutzeraufgaben abfragen.

Gewähren Sie die Dienstberechtigungen wie folgt:

1. Öffnen Sie einen Browser, und stellen Sie eine Verbindung mit der [Windows Virtual Desktop-Seite für die Einwilligung](https://rdweb.wvd.microsoft.com) her.
2. Geben Sie unter **Consent Option** > **Server App** (Einwilligungsoption > Server-App) den Namen des Azure Active Directory-Mandanten oder die Verzeichnis-ID ein, und wählen Sie anschließend **Submit** (Absenden).
        Für Cloud Solution Provider-Kunden ist die ID die Microsoft-ID des Kunden aus dem Partnerportal. Für Enterprise-Kunden befindet sich die ID unter **Azure Active Directory** > **Eigenschaften** > **Verzeichnis-ID**.
3. Melden Sie sich auf der Windows Virtual Desktop-Seite für die Einwilligung mit dem Konto eines globalen Administrators an. Wenn Sie für die Organisation Contoso arbeiten, lautet Ihr Kontoname beispielsweise admin@contoso.com oder admin@contoso.onmicrosoft.com.  
4. Wählen Sie **Akzeptieren** aus.
5. Warten Sie eine Minute.
6. Navigieren Sie zurück zur [Windows Virtual Desktop-Seite für die Einwilligung](https://rdweb.wvd.microsoft.com).
7. Navigieren Sie zu **Consent Option** > **Client App** (Einwilligungsoption > Client-App), geben Sie denselben Azure AD-Mandantennamen bzw. die Verzeichnis-ID ein, und wählen Sie anschließend **Submit** (Absenden).
8. Melden Sie sich (wie in Schritt 3) auf der Seite für die Windows Virtual Desktop-Einwilligung als globaler Administrator an.
9. Wählen Sie **Akzeptieren** aus.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>Zuweisen der Anwendungsrolle „TenantCreator“ für einen Benutzer in Ihrem Azure Active Directory-Mandanten

Wenn die Anwendungsrolle „TenantCreator“ einem Azure Active Directory-Benutzer zugewiesen wird, kann der jeweilige Benutzer einen Windows Virtual Desktop-Mandanten erstellen, der der Azure Active Directory-Instanz zugeordnet ist. Sie müssen Ihr globales Administratorkonto verwenden, um die Rolle „TenantCreator“ zuzuweisen.

Weisen Sie die Anwendungsrolle „TenantCreator“ über Ihr globales Administratorkonto zu:

1. Öffnen Sie einen Browser, und stellen Sie über Ihr globales Administratorkonto eine Verbindung mit dem [Azure Active Directory-Portal](https://aad.portal.azure.com) her.
   - Wenn Sie mehrere Azure AD-Mandanten verwenden, besteht die bewährte Methode darin, eine private Browsersitzung zu öffnen und URLs zu kopieren und in die Adressleiste einzufügen.
2. Wählen Sie **Unternehmensanwendungen**, und suchen Sie nach **Windows Virtual Desktop**. Die beiden Anwendungen, für die Sie im vorherigen Abschnitt die Einwilligung erteilt haben, werden angezeigt. Wählen Sie von diesen beiden Apps **Windows Virtual Desktop** aus.
3. Wählen Sie **Benutzer und Gruppen** und dann **Benutzer hinzufügen**.
4. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option „Benutzer und Gruppen“.
5. Suchen Sie nach einem Benutzerkonto, das zum Erstellen Ihres Windows Virtual Desktop-Mandanten verwendet wird.
   - Der Einfachheit halber kann dies das globale Administratorkonto sein.
6. Wählen Sie das Benutzerkonto aus, klicken Sie auf die Schaltfläche **Auswählen**, und wählen Sie dann **Zuweisen**.

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Erstellen eines Windows Virtual Desktop-Mandanten (Vorschauversion)

Nachdem Sie nun die Windows Virtual Desktop-Dienstberechtigungen zum Abfragen der Azure Active Directory-Instanz gewährt und die Rolle „TenantCreator“ einem Benutzerkonto zugewiesen haben, können Sie einen Windows Virtual Desktop-Mandanten erstellen.

Zunächst müssen Sie das [Windows Virtual Desktop-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Melden Sie sich mit dem TenantCreator-Benutzerkonto an Windows Virtual Desktop an, indem Sie dieses Cmdlet verwenden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Erstellen Sie anschließend einen neuen Windows Virtual Desktop-Mandanten, der dem Azure Active Directory-Mandanten zugeordnet ist:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Die Werte in Klammern sollten durch die Werte ersetzt werden, die für Ihre Organisation und den Mandanten relevant sind. Angenommen, Sie sind der „TenantCreator“ von Windows Virtual Desktop für die Organisation Contoso. Das auszuführende Cmdlet sieht dann wie folgt aus:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren Mandanten erstellt haben, müssen Sie einen Hostpool zusammenstellen. Weitere Informationen zu Hostpools finden Sie im Tutorial zum Erstellen eines Hostpools in Windows Virtual Desktop.

> [!div class="nextstepaction"]
> [Tutorial: Create a host pool with Azure Marketplace](./create-host-pools-azure-marketplace.md) (Tutorial: Erstellen eines Hostpools mit Azure Marketplace)
