---
title: 'Bereitstellen von App Service in einer Offlineumgebung: Azure Stack | Microsoft-Dokumentation'
description: Hier finden Sie detaillierte Informationen zum Bereitstellen von App Service in einer getrennten, durch AD FS gesicherten Azure Stack-Umgebung.
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: anwestg
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: b733851d4459ead1ae437604a27ca7720e2a3a87
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Hinzufügen eines App Service-Ressourcenanbieters zu einer getrennten, durch AD FS gesicherten Azure Stack-Umgebung

In folgenden Fällen müssen Sie Ihrer Azure Stack-Bereitstellung einen [Azure App Service-Ressourcenanbieter](azure-stack-app-service-overview.md) hinzufügen:

* Sie führen Azure Stack in einer durch Active Directory Federation Services (AD FS) gesicherten, isolierten Umgebung aus. 
* Sie möchten Ihren Mandanten die Möglichkeit bieten, mit ihrem Azure Stack-Abonnement mobile, Web- und API-Anwendungen sowie Azure Functions-Anwendungen zu erstellen. 

Zu diesem Zweck führen Sie die Schritte in diesem Artikel aus.

## <a name="download-the-required-components"></a>Herunterladen der erforderlichen Komponenten

1. Laden Sie das [Installationsprogramm für die Vorschauversion von App Service in Azure Stack](http://aka.ms/appsvconmasrc1installer) herunter.

2. Laden Sie die [Hilfsskripts für die Bereitstellung von App Service in Azure Stack](http://aka.ms/appsvconmasrc1helper) herunter.

3. Extrahieren Sie die Dateien aus der ZIP-Datei der Hilfsskripts. Die folgende Datei- und Ordnerstruktur wird entpackt:

   - Create-AppServiceCerts.ps1
   - Create-IdentityApp.ps1
   - Module
      - AzureStack.Identity.psm1
      - GraphAPI.psm1

## <a name="create-an-offline-installation-package"></a>Erstellen eines Offlineinstallationspakets

Um App Service in einer isolierten Umgebung bereitzustellen, müssen Sie auf einem Computer, der über eine Internetverbindung verfügt, ein Offlineinstallationspaket erstellen.

1. Führen Sie das Installationsprogramm für die Vorschauversion von App Service in Azure Stack (AppService.exe) auf einem Computer aus, der mit dem Internet verbunden ist.

2. Klicken Sie auf die Registerkarte **Erweitert**, und klicken Sie dann auf **Offlineinstallationspaket erstellen**.

    ![App Service in Azure Stack – Offlineinstallationspaket erstellen][1]

3. Das App Service-Installationsprogramm erstellt ein Offlineinstallationspaket, zeigt den Pfad an und bietet eine Option, den Ordner zu öffnen.

   ![App Service in Azure Stack – Offlineinstallationspaket][2]

4. Kopieren Sie das Installationsprogramm für die Vorschauversion von App Service in Azure Stack (AppService.exe) und das Offlineinstallationspaket auf den Azure Stack-Hostcomputer.

## <a name="create-certificates-required-by-app-service-on-azure-stack"></a>Erstellen der für App Service in Azure Stack erforderlichen Zertifikate

Dieses erste Skript erstellt zusammen mit der Azure Stack-Zertifizierungsstelle drei Zertifikate, die App Service benötigt. Führen Sie das Skript auf dem Azure Stack-Host aus, und stellen Sie sicher, dass Sie PowerShell als azurestack\administrator ausführen.

1. Führen Sie in einer als azurestack\administrator ausgeführten PowerShell-Sitzung das Skript **Create-AppServiceCerts.ps1** an dem Speicherort aus, an dem Sie die Hilfsskripts extrahiert haben.  Das Skript erstellt im selben Ordner, der vom Skript zum Erstellen der von App Service benötigten Zertifikate verwendet wird, drei Zertifikate.

2. Geben Sie ein Kennwort ein, um die PFX-Dateien zu schützen, und notieren Sie das Kennwort. Sie müssen es im Installationsprogramm für App Service in Azure Stack eingeben.

### <a name="create-appservicecertsps1-parameters"></a>Create-AppServiceCerts.ps1 – Parameter

| Parameter | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| pfxPassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| CertificateAuthority | Erforderlich | AzS-CA01.azurestack.local | Endpunkt der Zertifizierungsstelle |

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Abschließen der Offlineinstallation von App Service in Azure Stack

> [!NOTE]
> Sie *müssen* ein Konto mit erhöhten Rechten (lokaler oder Domänenadministrator) verwenden, um das Installationsprogramm auszuführen. Wenn Sie sich als azurestack\azurestackuser anmelden, werden Sie aufgefordert, Anmeldeinformationen für ein Konto mit erhöhten Rechten einzugeben.

1. Führen Sie appservice.exe als azurestack\administrator aus.

2. Klicken Sie auf die Registerkarte **Erweitert**, und klicken Sie dann auf **Offlineinstallation abschließen**.

    ![App Service in Azure Stack – Offlineinstallation abschließen][3]

3. Geben Sie den Speicherort des zuvor erstellten Offlineinstallationspakets an, und klicken Sie auf **Weiter**.

    ![App Service in Azure Stack – Speicherort des Offlineinstallationspakets][4]

4. Lesen und akzeptieren Sie die Lizenzbedingungen für Vorabversionen von Microsoft-Software, und klicken Sie auf **Weiter**.

5. Lesen und akzeptieren Sie die Drittanbieter-Lizenzbedingungen, und klicken Sie auf **Weiter**.

6. Lesen Sie die Informationen zur App Service-Cloudkonfiguration, und klicken Sie auf **Weiter**.

    ![App Service in Azure Stack – Cloudkonfiguration][5]

    > [!NOTE]
    > Das Installationsprogramm für App Service in Azure Stack gibt die Standardwerte für eine Azure Stack-Installation auf einem Knoten an. Wenn Sie bei der Bereitstellung von Azure Stack die Optionen (z.B. das Domänensuffix) angepasst haben, müssen Sie die Werte in diesem Fenster entsprechend bearbeiten. Wenn Sie beispielsweise das Domänensuffix „mycloud.com“ verwenden, muss der Azure Resource Manager-Endpunkt zu „adminmanagement.[region].mycloud.com“ geändert werden.

7. Klicken Sie neben dem Feld **Azure Stack-Abonnements** auf die Schaltfläche **Verbinden**. Geben Sie Ihr Administratorkonto ein, z.B. azurestackadmin@azurestack.local. Geben Sie Ihr Kennwort ein, und klicken Sie auf **Anmelden**.

8. Wählen Sie im Feld **Azure Stack-Abonnements** Ihr Abonnement aus.

9. Wählen Sie im Feld **Azure Stack-Standorte** den Standort aus, der der Region entspricht, in der die Bereitstellung erfolgen soll. Wählen Sie z.B. **lokal** aus. Klicken Sie auf **Weiter**.

    ![App Service in Azure Stack – Abonnementauswahl][6]

10. Geben Sie den **Ressourcengruppennamen** für Ihre App Service-Bereitstellung ein. Dieser ist standardmäßig auf **APPSERVICE-LOCAL** festgelegt.

11. Geben Sie den **Speicherkontonamen** ein, den App Service während der Installation erstellen soll. Dieser ist standardmäßig auf **appsvclocalstor** festgelegt.

12. Geben Sie die SQL Server-Informationen für die Instanz ein, in der die Datenbanken des App Service-Ressourcenanbieters gehostet werden sollen. Klicken Sie auf **Weiter**. Das Installationsprogramm überprüft die SQL-Verbindungseigenschaften.

13. Klicken Sie neben dem Feld **Standard-SSL-Zertifikatdatei für App Service** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **_.appservice.local.AzureStack.external**. Wenn Sie beim Erstellen des Zertifikats einen anderen Speicherort und ein anderes Domänensuffix angegeben haben, wählen Sie das entsprechende Zertifikat aus.

14. Geben Sie das Zertifikatkennwort ein, das Sie beim Erstellen des Zertifikats festgelegt haben.

15. Klicken Sie neben dem Feld **SSL-Zertifikatdatei des Ressourcenanbieters** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **api.appservice.local.AzureStack.external**. Wenn Sie beim Erstellen des Zertifikats einen anderen Speicherort und ein anderes Domänensuffix angegeben haben, wählen Sie das entsprechende Zertifikat aus.

16. Geben Sie das Zertifikatkennwort ein, das Sie beim Erstellen des Zertifikats festgelegt haben.

17. Klicken Sie neben dem Feld **Stammzertifikatdatei des Ressourcenanbieters** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **AzureStackCertificationAuthority**.

18. Klicken Sie auf **Weiter**.
 Das Installationsprogramm überprüft das angegebene Zertifikatkennwort.

    ![App Service in Azure Stack – Zertifikatdetails][8]

19. Überprüfen Sie die App Service-Rollenkonfiguration. Die Standardwerte werden mit den als Minimum empfohlenen Instanz-SKUs für jede Rolle aufgefüllt. Eine Übersicht über die Kern- und Arbeitsspeichervoraussetzungen wird angezeigt, um Sie bei der Bereitstellung zu unterstützen. Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Weiter**.

    - **Controller**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die Controllerrolle ist für die Verwaltung und Wartung der Integrität der App Service-Cloud zuständig.
    - **Verwaltung**: Standardmäßig ist eine Standard-A2-Instanz ausgewählt. Um Failoverfunktionen bereitzustellen, werden zwei Instanzen empfohlen. Die Verwaltungsrolle ist für die Azure Resource Manager- und API-Endpunkte, die Portalerweiterungen (Administrator-, Mandanten, Functions-Portal) und den Datendienst von App Service zuständig.
    - **Herausgeber**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die Herausgeberrolle ist für die Veröffentlichung von Inhalten über FTP- und Webbereitstellungen zuständig.
    - **FrontEnd**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die FrontEnd-Rolle ist für das Routen von Anforderungen an App Service-Anwendungen zuständig.
    - **Freigegebener Worker**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt, es empfiehlt sich jedoch möglicherweise, weitere Worker hinzuzufügen. Als Administrator können Sie Ihr Angebot definieren und eine SKU-Ebene auswählen. Die Ebenen müssen mindestens einen Kern aufweisen. Die Rolle „Freigegebener Worker“ ist für das Hosten von mobilen, Web- oder API-Anwendungen sowie Azure Functions-Apps zuständig.

    ![App Service in Azure Stack – Rollenkonfiguration][9]

    > [!NOTE]
    > In den Technical Previews stellt das Installationsprogramm des App Service-Ressourcenanbieters auch eine Standard-A1-Instanz bereit, die als einfacher Dateiserver zur Unterstützung von Azure Resource Manager fungiert. Dies wird für Konfigurationen mit einem Knoten als zentraler Kontaktpunkt beibehalten. Bei allgemeiner Verfügbarkeit ermöglicht das App Service-Installationsprogramm die Verwendung eines hochverfügbaren Dateiservers für Produktionsworkloads.

20. Wählen Sie aus den VM-Images, die im Computeressourcenanbieter für die App Service-Cloud verfügbar sind, Ihr VM-Bereitstellungsimage für **Windows Server 2016** aus. Klicken Sie auf **Weiter**.
 

    ![App Service in Azure Stack – Auswahl des VM-Images][10]

21. Geben Sie einen Benutzernamen und ein Kennwort für die in der App Service-Cloud konfigurierten Workerrollen ein. Geben Sie einen Benutzernamen und ein Kennwort für alle weiteren App Service-Rollen ein. Klicken Sie auf **Weiter**.


    ![App Service in Azure Stack – Eingabe von Anmeldeinformationen][11]

22. Überprüfen Sie auf dem Bildschirm mit der Zusammenfassung die von Ihnen ausgewählten Optionen. Um Angaben zu ändern, wechseln Sie durch die Bildschirme zurück, und nehmen Sie die entsprechenden Änderungen vor. Wenn die Konfiguration Ihren Wünschen entspricht, aktivieren Sie das Kontrollkästchen. Um die Bereitstellung zu starten, klicken Sie auf **Weiter**. 

    ![App Service in Azure Stack – Zusammenfassung der Auswahl][12]

23. Verfolgen Sie den Installationsstatus nach. Bei Auswahl der Standardoptionen dauert die Bereitstellung von App Service in Azure Stack ca. 45 bis 60 Minuten.

    ![App Service in Azure Stack – Installationsstatus][13]

24. Klicken Sie nach erfolgreichem Abschluss des Installationsprogramms auf **Beenden**.

## <a name="configure-an-ad-fs-service-principal-for-virtual-machine-scale-set-integration-on-worker-tiers-and-sso-for-the-azure-functions-portal-and-advanced-developer-tools"></a>Konfigurieren eines AD FS-Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen sowie von SSO für das Azure Functions-Portal und erweiterte Entwicklertools

>[!NOTE]
> Diese Schritte gelten nur für durch AD FS gesicherte Azure Stack-Umgebungen.

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:

* Konfigurieren eines Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen
* Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
* Aktivieren der Verwendung des Azure Functions-Portals 

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als azurestack\azurestackadmin.

2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](#Download-Required-Components) heruntergeladen und extrahiert wurden.

3. [Installieren](azure-stack-powershell-install.md) und [konfigurieren Sie eine Azure Stack-PowerShell-Umgebung](azure-stack-powershell-configure-admin.md).

4. Führen Sie in der gleichen PowerShell-Sitzung das Skript **CreateIdentityApp.ps1** aus. Wenn Sie zur Eingabe Ihrer Azure Active Directory-Mandanten-ID (Azure AD) aufgefordert werden, geben Sie **ADFS** ein.

5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres AD FS-Diensts ein. Klicken Sie auf **OK**.

6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](# Create certificates to be used by App Service on Azure Stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet sso.appservice.local.azurestack.external.pfx.

7. Das Skript erstellt eine neue Anwendung im Azure AD des Mandanten und generiert ein neues PowerShell-Skript.

8. Kopieren Sie die Zertifikatdatei und das generierte Skript für die Identitäts-App mithilfe einer Remotedesktopsitzung auf den virtuellen Computer **CN0-VM**.

9. Kehren Sie zu **CN0-VM** zurück.

10. Öffnen Sie ein PowerShell-Administratorfenster, und navigieren Sie zu dem Verzeichnis, in das in Schritt 7 die Skriptdatei und das Zertifikat kopiert wurden.

11. Führen Sie die Skriptdatei aus. Diese Skriptdatei gibt die Eigenschaften in die Konfiguration von App Service in Azure Stack ein und initiiert einen Reparaturvorgang auf allen FrontEnd- und Verwaltungsrollen.

| Parameter | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | **ADFS** für die AD FS-Umgebung verwenden |
| TenantAzure Resource ManagerEndpoint | Erforderlich | management.local.azurestack.external | Der Azure Resource Manager-Endpunkt des Mandanten |
| AzureStackCredential | Erforderlich | Null | Das Administratorkonto des AD FS-Diensts |
| CertificateFilePath | Erforderlich | Null | Pfad zur zuvor generierten Zertifikatdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| AdfsMachineName | Optional | Name des AD FS-Computers, z.B.: AzS-ADFS01.azurestack.local |


## <a name="validate-the-app-service-on-azure-stack-installation"></a>Überprüfen der Installation von App Service in Azure Stack

1. Navigieren Sie im Azure Stack-Administratorportal zu der Ressourcengruppe, die vom Installationsprogramm erstellt wurde. Diese Gruppe lautet standardmäßig **APPSERVICE-LOCAL**.

2. Suchen Sie **CN0-VM**. Um eine Verbindung mit dem virtuellen Computer herzustellen, klicken Sie auf dem Blatt **Virtueller Computer** auf **Verbinden**.

3. Doppelklicken Sie auf dem Desktop dieses virtuellen Computers auf **Webcloud-Verwaltungskonsole**.

4. Wechseln Sie zu **Verwaltete Server**.

5. Wenn bei allen Computern für einen oder mehrere Worker der Status **Bereit** angezeigt wird, fahren Sie mit Schritt 6 fort.

6. Schließen Sie den Remotedesktopcomputer, und kehren Sie zu dem Computer zurück, auf dem Sie das App Service-Installationsprogramm ausgeführt haben.

    > [!NOTE]
    > Sie müssen nicht warten, bis für einen oder mehrere Worker der Status **Bereit** angezeigt wird, um die Installation von App Service in Azure Stack abzuschließen. Sie benötigen jedoch mindestens einen Worker, der bereit ist, um eine mobile, Web- oder API-App oder Azure Functions bereitzustellen.
    
    ![App Service in Azure Stack – Status der verwalteten Server][14]

## <a name="test-drive-app-service-on-azure-stack"></a>Testen von App Service in Azure Stack

Nachdem Sie den App Service-Ressourcenanbieter bereitgestellt und registriert haben, testen Sie ihn, um sicherzustellen, dass Mandanten mobile, Web- und API-Apps bereitstellen können.

> [!NOTE]
> Sie müssen ein Angebot erstellen, das den Namespace „Microsoft.Web“ im Plan enthält. Danach benötigen Sie ein Mandantenabonnement, das dieses Angebot abonniert. Weitere Informationen finden Sie unter [Erstellen eines Angebots](azure-stack-create-offer.md) und [Erstellen eines Plans](azure-stack-create-plan.md).
>

Sie *müssen* über ein Mandantenabonnement verfügen, um Anwendungen zu erstellen, die App Service in Azure Stack verwenden. Ein Dienstadministrator kann nur solche Funktionen im Administratorportal ausführen, die mit der Ressourcenanbieterverwaltung von App Service zu tun haben. Hierzu gehören das Hinzufügen von Kapazitäten, das Konfigurieren von Bereitstellungsquellen und das Hinzufügen von Workerebenen und SKUs.

Seit der dritten Technical Preview müssen Sie das Mandantenportal verwenden und über ein Mandantenabonnement verfügen, um mobile, Web- und API-Apps zu erstellen.  

1. Klicken Sie im Azure Stack-Mandantenportal auf **Neu** > **Web und mobil** > **Web-App**.

2. Geben Sie auf dem Blatt **Web-App** einen Namen in das Feld **Web-App** ein.

3. Klicken Sie unter **Ressourcengruppe** auf **Neu**. Geben Sie einen Namen in das Feld **Ressourcengruppe** ein.

4. Klicken Sie auf **App Service-Plan/Standort** > **Neu erstellen**.

5. Geben Sie auf dem Blatt **App Service-Plan** einen Namen in das Feld **App Service-Plan** ein.

6. Klicken Sie auf **Tarif** > **Free-Shared** oder **Shared-Shared** > **Auswählen** > **OK** > **Erstellen**.

7. Nach weniger als einer Minute wird auf dem Dashboard eine Kachel für die neue Web-App angezeigt. Klicken Sie auf die Kachel.

8. Klicken Sie auf dem Blatt **Web-App** auf **Durchsuchen**, um die Standardwebsite für diese App anzuzeigen.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Bereitstellen einer WordPress-, DNN- oder Django-Website (optional)

1. Klicken Sie im Azure Stack-Mandantenportal auf **+**. Wechseln Sie zum Azure Marketplace, stellen Sie eine Django-Website bereit, und warten Sie, bis der Vorgang erfolgreich abgeschlossen ist. Die Django-Webplattform verwendet eine dateisystembasierte Datenbank. Sie erfordert keine zusätzlichen Ressourcenanbieter wie SQL oder MySQL.

2. Wenn Sie auch einen MySQL-Ressourcenanbieter bereitgestellt haben, können Sie über den Marketplace eine WordPress-Website bereitstellen. Wenn Sie zur Angabe der Datenbankparameter aufgefordert werden, geben Sie den Benutzernamen im Format *User1@Server1* ein. Sie können einen Benutzer- und Servernamen Ihrer Wahl eingeben.

3. Wenn Sie auch einen SQL Server-Ressourcenanbieter bereitgestellt haben, können Sie über den Marketplace eine DNN-Website bereitstellen. Wenn Sie zur Angabe der Datenbankparameter aufgefordert werden, wählen Sie auf dem SQL Server-Computer, der mit Ihrem Ressourcenanbieter verbunden ist, eine Datenbank aus.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch weitere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren.

- [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md)
- [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-offline-step1.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-offline-step2.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-offline-step3.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-offline-step4.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-cloud-configuration.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-subscription-location-populated.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-resource-group-SQL.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-certificates.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-role-configuration.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-vm-image-selection.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-default-entries-step-role-credentials.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-selection-summary.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-installation-progress.png
[14]: ./media/azure-stack-app-service-deploy-offline/managed-servers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

