---
title: Aktivieren der Azure Active Directory-Authentifizierung für die Azure SSIS Integration Runtime | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie die Azure SSIS Integration Runtime konfiguriert wird, um Verbindungen zu aktivieren, die die Azure Active Directory-Authentifizierung verwenden.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: aa06110a6f6fe668388c6aecd98c1ddeeae37edd
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576628"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Aktivieren der Azure Active Directory-Authentifizierung für die Azure SSIS Integration Runtime

In diesem Artikel wird das Erstellen einer Azure SSIS IR mit der Azure Data Factory-Dienstidentität veranschaulicht. Die Azure AD-Authentifizierung (Azure Active Directory) mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für die Azure SSIS Integration Runtime können Sie die Data Factory-MSI anstelle der SQL-Authentifizierung verwenden, um eine Azure SSIS Integration Runtime zu erstellen.

Weitere Informationen zur Data Factory-MSI finden Sie unter [Azure Data Factory-Dienstidentität](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Wenn Sie bereits eine Azure SSIS Integration Runtime mit der SQL-Authentifizierung erstellt haben, können Sie die IR gegenwärtig nicht wieder für die Verwendung der Azure AD-Authentifizierung mit PowerShell konfigurieren.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Erstellen einer Gruppe in Azure AD und Hinzufügen der MSI als Mitglied dieser Gruppe

Sie können eine vorhandene Azure AD-Gruppe verwenden oder mithilfe von Azure AD PowerShell eine neue erstellen.

1.  Installieren Sie das [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)-Modul.

2.  Melden Sie sich mit `Connect-AzureAD` an, und führen Sie den folgenden Befehl aus, um die Gruppe zu erstellen und in einer Variable zu speichern:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Die Ausgabe sieht etwa wie im folgenden Beispiel aus, die auch den Wert der Variable untersucht:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Fügen Sie der Gruppe die Data Factory-MSI hinzu. Sie können die Schritte unter [Azure Data Factory-Dienstidentität](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) durchführen, um die DIENSTPRINZIPALIDENTITÄTS-ID abzurufen (z.B. 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, aber verwenden Sie nicht die DIENSTIDENTITÄTSANWENDUNGS-ID für diesen Zweck).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Sie können auch die Gruppenmitgliedschaft anschließend überprüfen.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Aktivieren von Azure AD in Azure SQL-Datenbank

Azure SQL-Datenbank unterstützt das Erstellen einer Datenbank mit einem Azure AD-Benutzer. Daher können Sie einen Azure AD-Benutzer als Active Directory-Administrator festlegen und sich über den Azure AD-Benutzer bei SSMS anmelden. Anschließend können Sie einen in der Datenbank enthaltenen Benutzer für die Azure AD-Gruppe erstellen, um die IR zum Erstellen des SQL Server Integration Services-Katalogs (SSIS) auf dem Server aktivieren zu können.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Aktivieren der Azure AD-Authentifizierung für Azure SQL-Datenbank

Sie können [die Azure AD-Authentifizierung für SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) mithilfe der folgenden Schritte konfigurieren:

1.  Wählen Sie im Azure-Portal im Navigationsbereich links **Alle Dienste** -> **SQL-Server** aus.

2.  Klicken Sie auf die SQL-Datenbank, für die die Azure AD-Authentifizierung aktiviert werden soll.

3.  Klicken Sie auf dem Blatt im Abschnitt **Einstellungen** auf **Active Directory-Administrator**.

4.  Wählen Sie in der Befehlsleiste **Administrator festlegen** aus.

5.  Wählen Sie ein Azure AD-Benutzerkonto als Administrator des Servers aus, und klicken Sie dann auf **Auswählen**.

6.  Wählen Sie in der Befehlsleiste **Speichern** aus.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Erstellen eines in der Datenbank enthaltenen Benutzers, der die Azure AD-Gruppe darstellt

Für den nächsten Schritt benötigen Sie [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Starten Sie SQL Server Management Studio.

2.  Geben Sie im Dialogfeld **Mit Server verbinden** den Namen Ihres SQL-Servers im Feld **Servername** ein.

3.  Wählen Sie im Feld **Authentifizierung** die Option **Active Directory: universell mit MFA-Unterstützung** aus. (Sie können auch andere zwei Active Directory-Authentifizierungstypen verwenden. Weitere Informationen finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder der verwalteten SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  Geben Sie im Feld **Benutzername** den Namen des Azure AD-Kontos ein, das Sie als Serveradministrator festlegen möchten, z.B. testuser@xxxonline.com.

5.  Wählen Sie **Verbinden** aus. Schließen Sie den Anmeldevorgang ab.

6.  Erweitern Sie im **Objekt-Explorer** unter **Datenbanken** den Ordner „Systemdatenbanken“.

7.  Klicken Sie mit der rechten Maustaste auf die **master**-Datenbank, und klicken Sie auf **Neue Abfrage**.

8.  Geben Sie im Abfragefenster die folgende Zeile ein, und klicken Sie auf der Symbolleiste auf **Ausführen**:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Der Befehl sollte erfolgreich abgeschlossen werden und den enthaltenen Benutzer für die Gruppe erstellen.

9.  Setzen Sie das Abfragefenster zurück, geben Sie die folgende Zeile ein, und klicken Sie auf der Symbolleiste auf **Ausführen**:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    Der Befehl sollte erfolgreich abgeschlossen werden und dem in der Datenbank enthaltenen Benutzer die Möglichkeit gewähren, die Datenbank zu erstellen.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Aktivieren von Azure AD in der verwalteten Azure SQL-Datenbank-Instanz

Die verwaltete Azure SQL-Datenbank-Instanz unterstützt nicht die Erstellung einer Datenbank mit anderen Azure AD-Benutzern als AD-Administrator. Daher müssen Sie die Azure AD-Gruppe als Active Directory-Administrator festlegen. Sie müssen keinen in der Datenbank enthaltenen Benutzer erstellen.

Sie können [die Azure AD-Authentifizierung für den Server mit der verwalteten SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) mithilfe der folgenden Schritte konfigurieren:

7.  Wählen Sie im Azure-Portal im Navigationsbereich links **Alle Dienste** -> **SQL-Server** aus.

8.  Klicken Sie auf den SQL-Server, für den die Azure AD-Authentifizierung aktiviert werden soll.

9.  Klicken Sie auf dem Blatt im Abschnitt **Einstellungen** auf **Active Directory-Administrator**.

10. Wählen Sie in der Befehlsleiste **Administrator festlegen** aus.

11. Suchen Sie nach der Azure AD-Gruppe (z.B. SSISIrGroup), wählen Sie diese aus, und klicken Sie auf **Auswählen**.

12. Wählen Sie in der Befehlsleiste **Speichern** aus.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Bereitstellen der Azure SSIS IR im Portal

Wenn Sie Ihre Azure SSIS IR mit dem Azure-Portal bereitstellen, aktivieren Sie auf der Seite **SQL-Einstellungen** die Option „AAD Authentifizierung mit der ADF-MSI verwenden“. (Im folgenden Screenshot werden die Einstellungen für die IR mit Azure SQL-Datenbank gezeigt. Für die IR mit der verwalteten Instanz ist die Eigenschaft „Katalogdatenbank-Dienstebene“ nicht verfügbar, was auch bei anderen Einstellungen der Fall ist.)

Weitere Informationen zum Erstellen einer Azure SSIS Integration Runtime finden Sie im Artikel [Erstellen einer Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Einstellungen für die Azure SSIS Integration Runtime](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Bereitstellen der Azure SSIS IR mit PowerShell

Um Ihre Azure SSIS IR mit PowerShell bereitzustellen, führen Sie folgende Schritte aus:

1.  Installieren Sie das [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) -Modul.

2.  Legen Sie in Ihrem Skript nicht den *CatalogAdminCredential*-Parameter fest. Beispiel: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Type Managed `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -Description $AzureSSISDescription `
                                               -Edition $AzureSSISEdition `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
