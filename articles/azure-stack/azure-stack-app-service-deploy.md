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
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: 2dd5fe36105f4013c36dd4dc952424d5672ba91f
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Hinzufügen eines App Service-Ressourcenanbieters zu Azure Stack

Als Azure Stack-Cloudoperator können Sie Ihren Benutzern ermöglichen, Web- und API-Anwendungen zu erstellen. Zu diesem Zweck müssen Sie zunächst Ihrer Azure Stack-Bereitstellung den [App Service-Ressourcenanbieter](azure-stack-app-service-overview.md) hinzufügen, wie in diesem Artikel beschrieben. Nachdem Sie den App Service-Ressourcenanbieter installiert haben, können Sie ihn in Ihre Angebote und Pläne einfügen. Benutzer können ihn dann abonnieren, um den Dienst abzurufen und mit dem Erstellen von Anwendungen zu beginnen.

> [!IMPORTANT]
> Stellen Sie vor dem Ausführen des Installers sicher, dass die Anweisungen unter [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md) befolgt wurden.
> 
>



## <a name="run-the-app-service-resource-provider-installer"></a>Ausführen des Installationsprogramms für den App Service-Ressourcenanbieter

Das Installieren des App Service-Ressourcenanbieters in Ihrer Azure Stack-Umgebung kann bis zu einer Stunde dauern. Während dieses Vorgangs führt das Installationsprogramm Folgendes aus:

* Es erstellt im angegebenen Azure Stack-Speicherkonto einen Blobcontainer.
* Es erstellt eine DNS-Zone und DNS-Einträge für App Service.
* Es registriert den App Service-Ressourcenanbieter.
* Es registriert die App Service-Katalogelemente.

Führen Sie zum Bereitstellen eines App Service-Ressourcenanbieters die folgenden Schritte aus:

1. Führen Sie „appservice.exe“ als Administrator (azurestack\CloudAdmin) aus.

2. Klicken Sie auf **Bereitstellung von App Service in Ihrer Azure Stack-Cloud**.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image01.png)

3. Lesen und akzeptieren Sie die Lizenzbedingungen von Microsoft-Software, und klicken Sie dann auf **Weiter**.

4. Lesen und akzeptieren Sie die Drittanbieter-Lizenzbedingungen, und klicken Sie dann auf **Weiter**.

5. Stellen Sie sicher, dass die App Service-Cloudkonfigurationsinformationen stimmen. Wenn Sie während der Bereitstellung mit dem Azure Stack Development Kit die Standardeinstellungen verwendet haben, können Sie hier die Standardwerte übernehmen. Wenn Sie die Optionen bei der Bereitstellung von Azure Stack jedoch angepasst haben, müssen Sie die Werte in diesem Fenster entsprechend anpassen. Wenn Sie z.B. das Domänensuffix „mycloud.com“ verwenden, muss Ihr Endpunkt in „management.mycloud.com“ geändert werden. Nachdem Sie Ihre Informationen bestätigt haben, klicken Sie auf **Weiter**.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image02.png)

6. Auf der nächsten Seite:
    1. Klicken Sie neben dem Feld **Azure Stack-Abonnements** auf die Schaltfläche **Verbinden**.
        - Wenn Sie Azure Active Directory (Azure AD) verwenden, geben Sie das Azure AD-Administratorkonto, das Sie bei der Bereitstellung von Azure Stack angegeben haben, und das zugehörige Kennwort ein. Klicken Sie auf **Anmelden**.
        - Wenn Sie Active Directory-Verbunddienste (AD FS) verwenden, geben Sie Ihr Administratorkonto an. Beispiel: cloudadmin@azurestack.local. Geben Sie Ihr Kennwort ein, und klicken Sie auf **Anmelden**.
    2. Wählen Sie im Feld **Azure Stack-Abonnements** Ihr Abonnement aus.
    3. Wählen Sie im Feld **Azure Stack-Standorte** den Standort aus, der der Region entspricht, in der die Bereitstellung erfolgen soll. Wählen Sie z.B. **lokal** aus, wenn Ihre Bereitstellung im Azure Stack Development Kit erfolgt.
    4. Geben Sie einen **Ressourcengruppennamen** für Ihre App Service-Bereitstellung ein. Dieser ist standardmäßig auf **APPSERVICE\<REGION\>** festgelegt.
    5. Geben Sie den **Namen des Speicherkontos** ein, das App Service während der Installation erstellen soll. Dieser ist standardmäßig auf **appsvclocalstor** festgelegt.
    6. Klicken Sie auf **Weiter**.


    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image03.png)

7. Geben Sie die Informationen für die Dateifreigabe ein, und klicken Sie dann auf **Weiter**. In der Adresse der Dateifreigabe muss der vollqualifizierte Domänenname des Dateiservers (z.B. „\\\appservicefileserver.local.cloudapp.azurestack.external\websites“) oder die IP-Adresse (z.B. „\\\10.0.0.1\websites“) verwendet werden.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image04.png)

8. Auf der nächsten Seite:
    1. Geben Sie im Feld **Identitätsanwendungs-ID** die GUID der Anwendung, die Sie für die Identität verwenden, ein.
    2. Geben Sie im Feld **Identitätsanwendungs-Zertifikatsdatei** den Speicherort der Zertifikatsdatei ein (oder navigieren Sie zu diesem).
    3. Geben Sie im Feld **Kennwort für das Identitätsanwendungszertifikat** das Kennwort für das Zertifikat ein. Dieses Kennwort haben Sie erstellt, als Sie das Skript zum Erstellen des Zertifikats verwendet haben.
    4. Geben Sie im Feld **Azure Resource Manager-Stammzertifikatsdatei** den Speicherort der Zertifikatsdatei ein (oder navigieren Sie zu diesem).
    5. Klicken Sie auf **Weiter**.


    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image05.png)

9. Klicken Sie für jede der drei Zertifikatsdateien auf **Durchsuchen**, und navigieren Sie zu der entsprechenden Zertifikatsdatei. Sie müssen das Kennwort für jedes Zertifikat angeben. Diese Zertifikate haben Sie im Schritt [Erstellen der erforderlichen Zertifikate](azure-stack-app-service-deploy.md#create-the-required-certificates) erstellt. Klicken Sie auf **Weiter**, nachdem Sie alle Informationen eingegeben haben.

    | Box | Beispielname für eine Zertifikatsdatei |
    | --- | --- |
    | **Standard-SSL-Zertifikatsdatei für App Service** | \_.appservice.local.AzureStack.external.pfx |
    | **SSL-Zertifikatsdatei für App Service-API** | api.appservice.local.AzureStack.external.pfx |
    | **SSL-Zertifikatsdatei für App Service-Herausgeber** | ftp.appservice.local.AzureStack.external.pfx |

    Wenn Sie beim Erstellen der Zertifikate ein anderes Domänensuffix verwendet haben, verwenden die Zertifikatsdateinamen nicht *local.AzureStack.external*. Verwenden Sie stattdessen Ihre benutzerdefinierten Domäneninformationen.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image06.png)    

10. Geben Sie die SQL Server-Informationen für die Serverinstanz ein, auf der die Datenbanken des App Service-Ressourcenanbieters gehostet werden sollen, und klicken Sie dann auf **Weiter**. Das Installationsprogramm überprüft die SQL-Verbindungseigenschaften.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image07.png)    

11. Überprüfen Sie die Optionen für Rolleninstanz und SKU. Als Standardwerte werden die Mindestanzahl der Instanz und die Mindest-SKU für jede Rolle in einer ASDK-Bereitstellung verwendet. Eine Übersicht über die Kern- und Arbeitsspeichervoraussetzungen wird angezeigt, um Sie bei der Bereitstellung zu unterstützen. Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Weiter**.

    > [!NOTE]
    > Befolgen Sie bei Produktionsbereitstellungen die Anweisungen unter [Kapazitätsplanung für Azure App Service-Serverrollen in Azure Stack](azure-stack-app-service-capacity-planning.md).
    > 
    >

    | Rolle | Mindestanzahl der Instanzen | Mindest-SKU | Hinweise |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A1 – (1 Kern, 1.792 MB) | Verwaltet und wartet die Integrität der App Service-Cloud |
    | Verwaltung | 1 | Standard_A2 – (2 Kerne, 3.584 MB) | Verwaltet die Azure Resource Manager- und API-Endpunkte, die Portalerweiterungen (Administrator-, Mandanten, Functions-Portal) und den Datendienst von App Service. Zur Unterstützung eines Failovers erhöhen Sie die empfohlenen Instanzen auf 2. |
    | Herausgeber | 1 | Standard_A1 – (1 Kern, 1.792 MB) | Veröffentlicht Inhalte per FTP oder Webbereitstellung |
    | FrontEnd | 1 | Standard_A1 – (1 Kern, 1.792 MB) | Leitet Anforderungen an App Service-Anwendungen weiter |
    | Freigegebener Worker | 1 | Standard_A1 – (1 Kern, 1.792 MB) | Hostet Web oder -API-Anwendungen und Azure Functions-Apps. Sie können ggf. weitere Instanzen hinzufügen. Als Operator können Sie Ihr Angebot definieren und eine SKU-Ebene auswählen. Die Ebenen müssen mindestens einen Kern aufweisen. |

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Windows Server 2016 Core ist kein unterstütztes Plattformimage für die Verwendung mit Azure App Service für Azure Stack**.

12. Wählen Sie im Feld **Plattformimage auswählen** aus den VM-Images, die im Computeressourcenanbieter für die App Service-Cloud verfügbar sind, Ihr VM-Bereitstellungsimage für Windows Server 2016 aus. Klicken Sie auf **Weiter**.


13. Auf der nächsten Seite:
     1. Geben Sie den Administratorbenutzernamen für den virtuellen Computer der Workerrolle und das zugehörige Kennwort ein.
     2. Geben Sie den Administratorbenutzernamen für den virtuellen Computer der anderen Rollen und das zugehörige Kennwort ein.
     3. Klicken Sie auf **Weiter**.


    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image09.png)    

14. Auf der Zusammenfassungsseite:
    1. Überprüfen Sie Ihre Auswahl. Um Änderungen vorzunehmen, verwenden Sie die Schaltfläche **Zurück**, um auf die vorherigen Seiten zu gelangen.
    2. Wenn die Konfigurationen richtig sind, aktivieren Sie das Kontrollkästchen.
    3. Um die Bereitstellung zu starten, klicken Sie auf **Weiter**.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image10.png)    

15. Auf der nächsten Seite:
    1. Verfolgen Sie den Installationsstatus nach. Bei Auswahl der Standardoptionen dauert die Bereitstellung von App Service in Azure Stack ca. 60 Minuten.
    2. Klicken Sie nach erfolgreichem Abschluss des Installationsprogramms auf **Beenden**.

    ![App Service-Installationsprogramm](media/azure-stack-app-service-deploy/image11.png)    


## <a name="validate-the-app-service-on-azure-stack-installation"></a>Überprüfen der Installation von App Service in Azure Stack

1. Navigieren Sie im Azure Stack-Verwaltungsportal zu **Verwaltung – App Service**.

2. Überprüfen Sie in der Übersicht unter „Status“, ob für **Status** die Option **Alle Rollen sind bereit** angezeigt wird.

    ![App Service-Verwaltung](media/azure-stack-app-service-deploy/image12.png)    

## <a name="test-drive-app-service-on-azure-stack"></a>Testen von App Service in Azure Stack

Nachdem Sie den App Service-Ressourcenanbieter bereitgestellt und registriert haben, testen Sie ihn, um sicherzustellen, dass Benutzer Web- und API-Apps bereitstellen können.

> [!NOTE]
> Sie müssen ein Angebot erstellen, das den Namespace „Microsoft.Web“ im Plan enthält. Danach benötigen Sie ein Mandantenabonnement, das dieses Angebot abonniert. Weitere Informationen finden Sie unter [Erstellen eines Angebots](azure-stack-create-offer.md) und [Erstellen eines Plans](azure-stack-create-plan.md).
>
Sie *müssen* über ein Mandantenabonnement verfügen, um Anwendungen zu erstellen, die App Service in Azure Stack verwenden. Ein Dienstadministrator kann nur solche Funktionen im Administratorportal ausführen, die mit der Ressourcenanbieterverwaltung von App Service zu tun haben. Hierzu gehören das Hinzufügen von Kapazitäten, das Konfigurieren von Bereitstellungsquellen und das Hinzufügen von Workerebenen und SKUs.
>
Seit der dritten Technical Preview müssen Sie das Mandantenportal verwenden und über ein Mandantenabonnement verfügen, um Web-, API- und Azure Functions-Apps zu erstellen.

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

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
