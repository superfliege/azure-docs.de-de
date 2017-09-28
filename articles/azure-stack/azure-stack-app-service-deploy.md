---
title: 'Bereitstellen von App Service: Azure Stack | Microsoft-Dokumentation'
description: Detaillierte Anleitung zum Bereitstellen von App Service in Azure Stack
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
ms.openlocfilehash: 4b4f978f008dbcd8a7424f285198535cf133d7e2
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Hinzufügen eines App Service-Ressourcenanbieters zu Azure Stack

Wenn Sie es Ihren Mandanten ermöglichen möchten, mit ihren Azure Stack-Abonnements mobile, Web- und API-Anwendungen zu erstellen, müssen Sie Ihrer Azure Stack-Bereitstellung einen [Azure App Service-Ressourcenanbieter](azure-stack-app-service-overview.md) hinzufügen. Führen Sie die Schritte in diesem Artikel aus.

## <a name="download-the-required-components"></a>Herunterladen der erforderlichen Komponenten

1. Laden Sie das [Installationsprogramm für die Vorschauversion von App Service in Azure Stack](http://aka.ms/appsvconmasrc1installer) herunter.

2. Laden Sie die [Hilfsskripts für die Bereitstellung von App Service in Azure Stack](http://aka.ms/appsvconmasrc1helper) herunter.

3. Extrahieren Sie die Dateien aus der ZIP-Datei der Hilfsskripts. Die folgende Datei- und Ordnerstruktur wird entpackt:

   - Create-AppServiceCerts.ps1
   - Create-IdentityApp.ps1
   - Module
      - AzureStack.Identity.psm1
      - GraphAPI.psm1
   
## <a name="create-certificates-required-by-app-service-on-azure-stack"></a>Erstellen der für App Service in Azure Stack erforderlichen Zertifikate

Dieses erste Skript erstellt zusammen mit der Azure Stack-Zertifizierungsstelle drei Zertifikate, die App Service benötigt. Führen Sie das Skript auf dem Azure Stack-Host aus, und stellen Sie sicher, dass Sie PowerShell als azurestack\AzureStackAdmin ausführen.

1. Führen Sie in einer als azurestack\AzureStackAdmin ausgeführten PowerShell-Sitzung das Skript **Create-AppServiceCerts.ps1** in dem Ordner aus, in den Sie die Hilfsskripts extrahiert haben. Das Skript erstellt im selben Ordner, der vom Skript zum Erstellen der von App Service benötigten Zertifikate verwendet wird, drei Zertifikate.

2. Geben Sie ein Kennwort ein, um die PFX-Dateien zu schützen, und notieren Sie das Kennwort. Sie müssen es im Installationsprogramm für App Service in Azure Stack eingeben.

### <a name="create-appservicecertsps1-parameters"></a>Create-AppServiceCerts.ps1 – Parameter

| Parameter | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| pfxPassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| CertificateAuthority | Erforderlich | AzS-CA01.azurestack.local | Endpunkt der Zertifizierungsstelle |

## <a name="use-the-installer-to-download-and-install-app-service-on-azure-stack"></a>Verwenden des Installationsprogramms zum Herunterladen und Installieren von App Service in Azure Stack

Das Installationsprogramm appservice.exe führt folgende Aktionen aus:

* Es fordert Sie auf, die Softwarelizenzbedingungen von Microsoft und Drittanbietern zu akzeptieren.
* Es sammelt Informationen über die Azure Stack-Bereitstellung.
* Es erstellt im angegebenen Azure Stack-Speicherkonto einen Blobcontainer.
* Es lädt die Dateien herunter, die zum Installieren des App Service-Ressourcenanbieters erforderlich sind.
* Es bereitet die Installation auf die Bereitstellung des App Service-Ressourcenanbieters in der Azure Stack-Umgebung vor.
* Es lädt die Dateien in das App Service-Speicherkonto hoch.
* Es stellt den App Service-Ressourcenanbieter bereit.
* Es erstellt eine DNS-Zone und DNS-Einträge für App Service.
* Es registriert den App Service-Ressourcenanbieter.
* Es registriert die App Service-Katalogelemente.

Die folgenden Schritte leiten Sie durch die Installationsphasen.

> [!NOTE]
> Sie *müssen* ein Konto mit erhöhten Rechten (lokaler oder Domänenadministrator) verwenden, um das Installationsprogramm auszuführen. Wenn Sie sich als azurestack\azurestackuser anmelden, werden Sie aufgefordert, Anmeldeinformationen für ein Konto mit erhöhten Rechten einzugeben.

1. Führen Sie appservice.exe als azurestack\AzureStackAdmin aus.

2. Klicken Sie auf **Bereitstellung von App Service in Ihrer Azure Stack-Cloud**.

    ![App Service in Azure Stack – Installationsprogramm][1]

3. Lesen und akzeptieren Sie die Lizenzbedingungen für Vorabversionen von Microsoft-Software, und klicken Sie auf **Weiter**.

4. Lesen und akzeptieren Sie die Drittanbieter-Lizenzbedingungen, und klicken Sie auf **Weiter**.

5. Lesen Sie die Informationen zur App Service-Cloudkonfiguration, und klicken Sie auf **Weiter**.

    ![App Service in Azure Stack – App Service-Cloudkonfiguration][2]

    > [!NOTE]
    > Das Installationsprogramm für App Service in Azure Stack gibt die Standardwerte für eine Azure Stack-Installation auf einem Knoten an. Wenn Sie bei der Bereitstellung von Azure Stack die Optionen (z.B. das Domänensuffix) angepasst haben, müssen Sie die Werte in diesem Fenster entsprechend bearbeiten. Wenn Sie beispielsweise das Domänensuffix „mycloud.com“ verwenden, muss der Azure Resource Manager-Endpunkt zu „adminmanagement.[region].mycloud.com“ geändert werden.

6. Klicken Sie neben dem Feld **Azure Stack-Abonnements** auf die Schaltfläche **Verbinden**.

   - Wenn Sie Azure Active Directory (Azure AD) verwenden, müssen Sie Ihr Azure AD-Administratorkonto und das zugehörige Kennwort eingeben. Klicken Sie auf **Anmelden**. Sie *müssen* das Azure AD-Konto eingeben, das Sie bei der Bereitstellung von Azure Stack angegeben haben.
   - Wenn Sie Active Directory Federation Services (AD FS) verwenden, müssen Sie Ihr Administratorkonto angeben, z.B. azurestackadmin@azurestack.local. Geben Sie Ihr Kennwort ein, und klicken Sie auf **Anmelden**.

7. Wählen Sie im Feld **Azure Stack-Abonnements** Ihr Abonnement aus.

8. Wählen Sie im Feld **Azure Stack-Standorte** den Standort aus, der der Region entspricht, in der die Bereitstellung erfolgen soll. Wählen Sie z.B. **lokal** aus. Klicken Sie auf **Weiter**.


    ![App Service in Azure Stack – Abonnementauswahl][3]

9. Geben Sie den **Ressourcengruppennamen** für Ihre App Service-Bereitstellung ein. Dieser ist standardmäßig auf **APPSERVICE-LOCAL** festgelegt.

10. Geben Sie den **Speicherkontonamen** ein, den App Service während der Installation erstellen soll. Dieser ist standardmäßig auf **appsvclocalstor** festgelegt.

11. Geben Sie die SQL Server-Informationen für die Instanz ein, in der die Datenbanken des App Service-Ressourcenanbieters gehostet werden sollen. Klicken Sie auf **Weiter**. Das Installationsprogramm überprüft die SQL-Verbindungseigenschaften.

    ![App Service in Azure Stack – Informationen zu Ressourcengruppe, Speicher und SQL Server][4]

12. Klicken Sie neben dem Feld **Standard-SSL-Zertifikatdatei für App Service** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **_.appservice.local.AzureStack.external**. Wenn Sie beim Erstellen des Zertifikats einen anderen Speicherort und ein anderes Domänensuffix angegeben haben, wählen Sie das entsprechende Zertifikat aus.

13. Geben Sie das Zertifikatkennwort ein, das Sie beim Erstellen des Zertifikats festgelegt haben.

14. Klicken Sie neben dem Feld **SSL-Zertifikatdatei des Ressourcenanbieters** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **api.appservice.local.AzureStack.external**. Wenn Sie beim Erstellen der Zertifikate einen anderen Speicherort und ein anderes Domänensuffix angegeben haben, wählen Sie das entsprechende Zertifikat aus.

15. Geben Sie das Zertifikatkennwort ein, das Sie beim Erstellen des Zertifikats festgelegt haben.

16. Klicken Sie neben dem Feld **Stammzertifikatdatei des Ressourcenanbieters** auf die Schaltfläche **Durchsuchen**. Wechseln Sie zum [zuvor erstellten](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps) Zertifikat **AzureStackCertificationAuthority**.

17. Klicken Sie auf **Weiter**.
 Das Installationsprogramm überprüft das angegebene Zertifikatkennwort.

    ![App Service in Azure Stack – Zertifikatdetails][5]

18. Überprüfen Sie die App Service-Rollenkonfiguration. Die Standardwerte werden mit den als Minimum empfohlenen Instanz-SKUs für jede Rolle aufgefüllt. Eine Übersicht über die Kern- und Arbeitsspeichervoraussetzungen wird angezeigt, um Sie bei der Bereitstellung zu unterstützen. Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Weiter**.

    - **Controller**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die Controllerrolle ist für die Verwaltung und Wartung der Integrität der App Service-Cloud zuständig.
    - **Verwaltung**: Standardmäßig ist eine Standard-A2-Instanz ausgewählt. Um Failoverfunktionen bereitzustellen, werden zwei Instanzen empfohlen. Die Verwaltungsrolle ist für die Azure Resource Manager- und API-Endpunkte, die Portalerweiterungen (Administrator-, Mandanten, Functions-Portal) und den Datendienst von App Service zuständig.
    - **Herausgeber**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die Herausgeberrolle ist für die Veröffentlichung von Inhalten über FTP- und Webbereitstellungen zuständig.
    - **FrontEnd**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt. Dies ist das empfohlene Minimum. Die FrontEnd-Rolle ist für das Routen von Anforderungen an App Service-Anwendungen zuständig.
    - **Freigegebener Worker**: Standardmäßig ist eine Standard-A1-Instanz ausgewählt, es empfiehlt sich jedoch möglicherweise, weitere Worker hinzuzufügen. Als Administrator können Sie Ihr Angebot definieren und eine SKU-Ebene auswählen. Die Ebenen müssen mindestens einen Kern aufweisen. Die Rolle „Freigegebener Worker“ ist für das Hosten von mobilen, Web- oder API-Anwendungen sowie Azure Functions-Apps zuständig.

    ![App Service in Azure Stack – Rollenkonfiguration][6]

    > [!NOTE]
    > In den Technical Previews stellt das Installationsprogramm des App Service-Ressourcenanbieters auch eine Standard-A1-Instanz bereit, die als einfacher Dateiserver zur Unterstützung von Azure Resource Manager fungiert. Dies wird in SDKs mit einem Knoten beibehalten. Bei allgemeiner Verfügbarkeit ermöglicht das App Service-Installationsprogramm die Verwendung eines hochverfügbaren Dateiservers für Produktionsworkloads.

19. Wählen Sie aus den VM-Images, die im Computeressourcenanbieter für die App Service-Cloud verfügbar sind, Ihr VM-Bereitstellungsimage für **Windows Server 2016** aus. Klicken Sie auf **Weiter**.


    ![App Service in Azure Stack – Auswahl des VM-Images][7]

20. Geben Sie einen Benutzernamen und ein Kennwort für die in der App Service-Cloud konfigurierten Workerrollen ein. Geben Sie einen Benutzernamen und ein Kennwort für alle weiteren App Service-Rollen ein. Klicken Sie auf **Weiter**.


    ![App Service in Azure Stack – Eingabe von Anmeldeinformationen][8]

21. Überprüfen Sie auf dem Bildschirm mit der Zusammenfassung die von Ihnen ausgewählten Optionen. Um Angaben zu ändern, wechseln Sie durch die Bildschirme zurück, und nehmen Sie die entsprechenden Änderungen vor. Wenn die Konfiguration Ihren Wünschen entspricht, aktivieren Sie das Kontrollkästchen. Um die Bereitstellung zu starten, klicken Sie auf **Weiter**.

    ![App Service in Azure Stack – Zusammenfassung der Auswahl][9]

22. Verfolgen Sie den Installationsstatus nach. Bei Auswahl der Standardoptionen dauert die Bereitstellung von App Service in Azure Stack ca. 45 bis 60 Minuten.

    ![App Service in Azure Stack – Installationsstatus][10]

23. Klicken Sie nach erfolgreichem Abschluss des Installationsprogramms auf **Beenden**.

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Überprüfen der Installation von App Service in Azure Stack

1. Navigieren Sie im Azure Stack-Administratorportal zu der Ressourcengruppe, die vom Installationsprogramm erstellt wurde. Diese Gruppe lautet standardmäßig **APPSERVICE-LOCAL**.

2. Suchen Sie **CN0-VM**. Um eine Verbindung mit dem virtuellen Computer herzustellen, klicken Sie auf dem Blatt **Virtueller Computer** auf **Verbinden**.

3. Doppelklicken Sie auf dem Desktop dieses virtuellen Computers auf **Webcloud-Verwaltungskonsole**.

4. Wechseln Sie zu **Verwaltete Server**.

5. Wenn bei allen Computern für einen oder mehrere Worker der Status **Bereit** angezeigt wird, fahren Sie mit Schritt 6 fort.

6. Schließen Sie den Remotedesktopcomputer, und kehren Sie zu dem Computer zurück, auf dem Sie das App Service-Installationsprogramm ausgeführt haben.

    > [!NOTE]
    > Sie müssen nicht warten, bis für einen oder mehrere Worker der Status **Bereit** angezeigt wird, um die Installation von App Service in Azure Stack abzuschließen. Sie benötigen jedoch mindestens einen Worker, der bereit ist, um eine mobile, Web- oder API-App oder Azure Functions bereitzustellen.

    ![App Service in Azure Stack – Status der verwalteten Server][11]

## <a name="configure-an-azure-ad-service-principal-for-virtual-machine-scale-set-integration-on-worker-tiers-and-sso-for-the-azure-functions-portal-and-advanced-developer-tools"></a>Konfigurieren eines Azure AD-Dienstprinzipals für die Integration von VM-Skalierungsgruppen auf Workerebenen sowie von SSO für das Azure Functions-Portal und erweiterte Entwicklertools

>[!NOTE]
> Diese Schritte gelten nur für durch Azure AD gesicherte Azure Stack-Umgebungen.

Administratoren müssen SSO zu folgenden Zwecken konfigurieren:

* Aktivieren der erweiterten Entwicklertools in App Service (Kudu)
* Aktivieren der Verwendung des Azure Functions-Portals

Folgen Sie diesen Schritten:

1. Öffnen Sie eine PowerShell-Instanz als azurestack\azurestackadmin.

2. Wechseln Sie zum Speicherort der Skripts, die im [Vorbereitungsschritt](#Download-Required-Components) heruntergeladen und extrahiert wurden.

3. [Installieren](azure-stack-powershell-install.md) und [konfigurieren Sie eine Azure Stack-PowerShell-Umgebung](azure-stack-powershell-configure-admin.md).

4. Führen Sie in der gleichen PowerShell-Sitzung das Skript **CreateIdentityApp.ps1** aus. Wenn Sie zur Eingabe Ihrer Azure AD-Mandanten-ID aufgefordert werden, geben Sie die Azure AD-Mandanten-ID ein, die Sie für Ihre Azure Stack-Bereitstellung verwenden, z.B.: myazurestack.onmicrosoft.com.

5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres Azure AD-Diensts ein. Klicken Sie auf **OK**.

6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](# Create certificates to be used by App Service on Azure Stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet sso.appservice.local.azurestack.external.pfx.

7. Das Skript erstellt eine neue Anwendung im Azure AD des Mandanten und generiert ein neues PowerShell-Skript namens **UpdateConfigOnController.ps1**.

    >[!NOTE]
    > Notieren Sie sich die **Anwendungs-ID**, die in der PowerShell-Ausgabe zurückgegeben wird. Sie benötigen diese Informationen, um in Schritt 12 danach suchen zu können.

8. Kopieren Sie die Zertifikatdatei und das generierte Skript für die Identitäts-App mithilfe einer Remotedesktopsitzung auf den virtuellen Computer **CN0-VM**.

9. Öffnen Sie ein neues Browserfenster, und melden Sie sich als **Azure Active Directory-Dienstadministrator** beim Azure-Portal (portal.azure.com) an.

10. Öffnen Sie den Azure AD-Ressourcenanbieter.

11. Klicken Sie auf **App-Registrierungen**.

12. Suchen Sie nach der **Anwendungs-ID**, die in Schritt 7 zurückgegeben wurde. Es wird eine App Service-Anwendung aufgeführt.

13. Klicken Sie in der Liste auf **Anwendung**, und öffnen Sie das Blatt **Schlüssel**.

14. Fügen Sie einen neuen Schlüssel mit **Beschreibung – Functions-Portal** hinzu, und legen Sie das **Ablaufdatum** auf **Läuft nie ab** fest.

15. Klicken Sie auf **Speichern**, und kopieren Sie dann den Schlüssel, der generiert wurde.

    >[!NOTE]
    > Stellen Sie sicher, dass Sie den Schlüssel kopieren oder sich notieren, nachdem er generiert wurde. Nach dem Speichern können Sie den Schlüssel nicht mehr anzeigen und müssen einen neuen generieren.

    ![App Service in Azure Stack – Anwendungsschlüssel][12]

16. Kehren Sie zur **Anwendungsregistrierung** in Azure AD zurück.

17. Klicken Sie auf **Erforderliche Berechtigungen** > **Berechtigungen erteilen** > **Ja**.

    ![App Service in Azure Stack – SSO gewähren][13]

18. Kehren Sie zu **CN0-VM** zurück, und öffnen Sie noch einmal die **Webcloud-Verwaltungskonsole**.

19. Wählen Sie im linken Bereich den Knoten **Einstellungen** aus, und suchen Sie die Einstellung **ApplicationClientSecret**.

20. Klicken Sie mit der rechten Maustaste, und wählen Sie **Bearbeiten** aus. Fügen Sie den in Schritt 15 generierten Schlüssel ein, und klicken Sie auf **OK**.

    ![App Service in Azure Stack – Anwendungsschlüssel][14]

21. Öffnen Sie ein PowerShell-Administratorfenster. Navigieren Sie zu dem Verzeichnis, in das in Schritt 7 die Skriptdatei und das Zertifikat kopiert wurden.

22. Führen Sie die Skriptdatei aus. Diese Skriptdatei gibt die Eigenschaften in die Konfiguration von App Service in Azure Stack ein und initiiert einen Reparaturvorgang auf allen FrontEnd- und Verwaltungsrollen.

| Parameter | Erforderlich/optional | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| DirectoryTenantName | Erforderlich | Null | Azure AD-Mandanten-ID; GUID oder Zeichenfolge angeben, z.B.: myazureaaddirectory.onmicrosoft.com |
| TenantAzure Resource ManagerEndpoint | Erforderlich | management.local.azurestack.external | Der Azure Resource Manager-Endpunkt des Mandanten |
| AzureStackCredential | Erforderlich | Null | Azure AD-Administrator |
| CertificateFilePath | Erforderlich | Null | Pfad zur zuvor generierten Zertifikatdatei für die Identitätsanwendung |
| CertificatePassword | Erforderlich | Null | Kennwort zum Schutz des privaten Zertifikatschlüssels |
| DomainName | Erforderlich | local.azurestack.external | Azure Stack-Region und Domänensuffix |
| AdfsMachineName | Optional | In einer Azure AD-Bereitstellung ignorierbar, in einer AD FS-Bereitstellung aber erforderlich; Name des AD FS-Computers, z.B.: AzS-ADFS01.azurestack.local |

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

4. Führen Sie in der gleichen PowerShell-Sitzung das Skript **CreateIdentityApp.ps1** aus. Wenn Sie zur Eingabe Ihrer Azure AD-Mandanten-ID aufgefordert werden, geben Sie **ADFS** ein.

5. Geben Sie im Fenster **Anmeldeinformationen** das Administratorkonto und -kennwort Ihres AD FS-Diensts ein. Klicken Sie auf **OK**.

6. Geben Sie den Zertifikatdateipfad und das Zertifikatkennwort für das [zuvor erstellte Zertifikat](# Create certificates to be used by App Service on Azure Stack) ein. Das für diesen Schritt standardmäßig erstellte Zertifikat lautet sso.appservice.local.azurestack.external.pfx.

7. Das Skript erstellt eine neue Anwendung im Azure AD des Mandanten und generiert ein neues PowerShell-Skript namens **UpdateConfigOnController.ps1**.

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

## <a name="test-drive-app-service-on-azure-stack"></a>Testen von App Service in Azure Stack

Nachdem Sie den App Service-Ressourcenanbieter bereitgestellt und registriert haben, testen Sie ihn, um sicherzustellen, dass Mandanten mobile, Web- und API-Apps bereitstellen können.

> [!NOTE]
> Sie müssen ein Angebot erstellen, das den Namespace „Microsoft.Web“ im Plan enthält. Danach benötigen Sie ein Mandantenabonnement, das dieses Angebot abonniert. Weitere Informationen finden Sie unter [Erstellen eines Angebots](azure-stack-create-offer.md) und [Erstellen eines Plans](azure-stack-create-plan.md).
>
Sie *müssen* über ein Mandantenabonnement verfügen, um Anwendungen zu erstellen, die App Service in Azure Stack verwenden. Ein Dienstadministrator kann nur solche Funktionen im Administratorportal ausführen, die mit der Ressourcenanbieterverwaltung von App Service zu tun haben. Hierzu gehören das Hinzufügen von Kapazitäten, das Konfigurieren von Bereitstellungsquellen und das Hinzufügen von Workerebenen und SKUs.
>
Seit der dritten Technical Preview müssen Sie das Mandantenportal verwenden und über ein Mandantenabonnement verfügen, um mobile, Web-, API- und Azure Functions-Apps zu erstellen. 

1. Klicken Sie im Azure Stack-Mandantenportal auf **Neu** > **Web und mobil** > **Web-App**.

2. Geben Sie auf dem Blatt **Web-App** einen Namen in das Feld **Web-App** ein.

3. Klicken Sie unter **Ressourcengruppe** auf **Neu**. Geben Sie einen Namen in das Feld **Ressourcengruppe** ein.

4. Klicken Sie auf **App Service-Plan/Standort** > **Neu erstellen**.

5. Geben Sie auf dem Blatt **App Service-Plan** einen Namen in das Feld **App Service-Plan** ein.

6. Klicken Sie auf **Tarif** > **Free-Shared** oder **Shared-Shared** > **Auswählen** > **OK** > **Erstellen**.

7. Nach weniger als einer Minute wird auf dem Dashboard eine Kachel für die neue Web-App angezeigt. Klicken Sie auf die Kachel.

8. Klicken Sie auf dem Blatt **Web-App** auf **Durchsuchen**, um die Standardwebsite für diese App anzuzeigen.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Bereitstellen einer WordPress-, DNN- oder Django-Website (optional)

1. Klicken Sie im Azure Stack-Mandantenportal auf **+**, wechseln Sie zum Azure Marketplace, stellen Sie eine Django-Website bereit, und warten Sie, bis der Vorgang erfolgreich abgeschlossen ist. Die Django-Webplattform verwendet eine dateisystembasierte Datenbank. Sie erfordert keine zusätzlichen Ressourcenanbieter wie SQL oder MySQL.

2. Wenn Sie auch einen MySQL-Ressourcenanbieter bereitgestellt haben, können Sie über den Marketplace eine WordPress-Website bereitstellen. Wenn Sie zur Angabe der Datenbankparameter aufgefordert werden, geben Sie den Benutzernamen im Format *User1@Server1* ein. Sie können einen Benutzer- und Servernamen Ihrer Wahl eingeben.

3. Wenn Sie auch einen SQL Server-Ressourcenanbieter bereitgestellt haben, können Sie über den Marketplace eine DNN-Website bereitstellen. Wenn Sie zur Angabe der Datenbankparameter aufgefordert werden, wählen Sie auf dem SQL Server-Computer, der mit Ihrem Ressourcenanbieter verbunden ist, eine Datenbank aus.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch weitere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren.

- [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md)
- [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-exe-start.png
[2]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-cloud-configuration.png
[3]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-subscription-location-populated.png
[4]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-resource-group-sql.png
[5]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-certificates.png
[6]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-role-configuration.png
[7]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-vm-image-selection.png
[8]: ./media/azure-stack-app-service-deploy/app-service-exe-default-entries-step-role-credentials.png
[9]: ./media/azure-stack-app-service-deploy/app-service-exe-selection-summary.png
[10]: ./media/azure-stack-app-service-deploy/app-service-exe-installation-progress.png
[11]: ./media/azure-stack-app-service-deploy/managed-servers.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sso-keys.png
[13]: ./media/azure-stack-app-service-deploy/app-service-sso-grant.png
[14]: ./media/azure-stack-app-service-deploy/app-service-application-secret.png

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

