---
title: Offlineaktualisieren von Azure App Service | Microsoft-Dokumentation
description: Detaillierter Leitfaden zum Offlineaktualisieren von Azure App Service in Azure Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 332722a1774eea41ab3566051da138f2383fef98
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447666"
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Offlineaktualisieren von Azure App Service in Azure Stack

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!IMPORTANT]
> Wenden Sie Update 1809 oder höher auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack Development Kit vor der Bereitstellung von Azure App Service 1.4 bereit. Überprüfen Sie vor der Aktualisierung des Ressourcenanbieters die Anmerkungen zu dieser Version auf Informationen zu neuen Funktionen, Fehlerbehebungen und bekannten Problemen, die sich auf die Bereitstellung auswirken können.

Mithilfe der Anweisungen in diesem Artikel können Sie den [App Service-Ressourcenanbieter](azure-stack-app-service-overview.md) in einer Azure Stack-Umgebung aktualisieren, für die Folgendes gilt:

* Sie ist nicht mit dem Internet verbunden.
* Sie wird mit Active Directory Federation Services (AD FS) geschützt.

> [!IMPORTANT]
> Stellen Sie vor dem Ausführen des Upgrades sicher, dass Sie die [Bereitstellung von Azure App Service im Azure Stack-Ressourcenanbieter](azure-stack-app-service-deploy-offline.md) bereits abgeschlossen haben.
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Ausführen des Installationsprogramms für den App Service-Ressourcenanbieter

Um den App Service-Ressourcenanbieter in einer Azure Stack-Umgebung zu aktualisieren, müssen Sie diese Aufgaben ausführen:

1. Laden Sie das [App Service-Installationsprogramm](https://aka.ms/appsvcupdate4installer) herunter.
2. Erstellen Sie ein Paket für das Offlineupgrade.
3. Führen Sie das App Service-Installationsprogramm („appservice.exe“) aus, und schließen Sie das Upgrade ab.

Während dieses Vorgangs führt das Upgrade Folgendes aus:

* Ermitteln der alten Bereitstellung von App Service
* Hochladen in Storage
* Aktualisieren aller App Service-Rollen (Controller, Verwaltung, Front-End, Herausgeber und Worker)
* Aktualisieren der App Service-Skalierungsgruppendefinitionen
* Aktualisieren des App Service-Ressourcenanbietermanifests

## <a name="create-an-offline-upgrade-package"></a>Erstellen eines Pakets für das Offlineupgrade

Um App Service in einer nicht verbundenen Umgebung zu aktualisieren, müssen Sie auf einem Computer, der über eine Internetverbindung verfügt, zunächst ein Offlineupgradepaket erstellen.

1. Führen Sie „appservice.exe“ als Administrator aus.

    ![App Service-Installationsprogramm][1]

2. Klicken Sie auf **Erweitert** > **Offlinepaket erstellen**.

    ![App Service-Installationsprogramm – Erweitert][2]

3. Das App Service-Installationsprogramm erstellt ein Offlineupgradepaket und zeigt den Pfad zu diesem an.  Klicken Sie auf **Ordner öffnen**, um den Ordner im Datei-Explorer zu öffnen.

4. Kopieren Sie das Installationsprogramm „AppService.exe“ und das Offlineupgradepaket auf den Azure Stack-Hostcomputer.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Abschließen des Upgrades von App Service in Azure Stack

> [!IMPORTANT]
> Das App Service-Installationsprogramm muss auf einem Computer ausgeführt werden, der den Azure Resource Manager-Endpunkt des Azure Stack-Administrators erreichen kann.
>
>

1. Führen Sie „appservice.exe“ als Administrator aus.

    ![App Service-Installationsprogramm][1]

2. Klicken Sie auf **Erweitert** > **Vollständige Offline-Installation oder -Aktualisierung**.

    ![App Service-Installationsprogramm – Erweitert][2]

3. Navigieren Sie zum Speicherort des zuvor erstellten Offlineupgradepakets, und klicken Sie dann auf **Weiter**.

4. Lesen und akzeptieren Sie die Lizenzbedingungen von Microsoft-Software, und klicken Sie dann auf **Weiter**.

5. Lesen und akzeptieren Sie die Drittanbieter-Lizenzbedingungen, und klicken Sie dann auf **Weiter**.

6. Stellen Sie sicher, dass die Informationen über den Azure Resource Manager-Endpunkt für Azure Stack und den Active Directory-Mandanten richtig sind. Wenn Sie während der Bereitstellung mit dem Azure Stack Development Kit die Standardeinstellungen verwendet haben, können Sie hier die Standardwerte übernehmen. Wenn Sie die Optionen bei der Bereitstellung von Azure Stack jedoch angepasst haben, müssen Sie die Werte in diesem Fenster anpassen. Wenn Sie beispielsweise das Domänensuffix *mycloud.com* verwenden, muss der Azure Resource Manager-Endpunkt für Azure Stack in *management.region.mycloud.com* geändert werden. Nachdem Sie Ihre Informationen bestätigt haben, klicken Sie auf **Weiter**.

    ![Azure Stack-Cloudinformationen][3]

7. Auf der nächsten Seite:

   1. Klicken Sie neben dem Feld **Azure Stack-Abonnements** auf die Schaltfläche **Verbinden**.
        * Wenn Sie Azure Active Directory (Azure AD) verwenden, geben Sie das Azure AD-Administratorkonto, das Sie bei der Bereitstellung von Azure Stack angegeben haben, und das zugehörige Kennwort ein. Klicken Sie auf **Anmelden**.
        * Wenn Sie Active Directory-Verbunddienste (AD FS) verwenden, geben Sie Ihr Administratorkonto an. Beispiel _cloudadmin@azurestack.local_. Geben Sie Ihr Kennwort ein, und klicken Sie auf **Anmelden**.
   2. Wählen Sie im Feld **Azure Stack-Abonnements** das **Standardabonnement des Anbieters** aus.
   3. Wählen Sie im Feld **Azure Stack-Standorte** den Standort aus, der der Region entspricht, in der die Bereitstellung erfolgen soll. Wählen Sie z.B. **lokal** aus, wenn Ihre Bereitstellung im Azure Stack Development Kit erfolgt.
   4. Wenn eine vorhandene App Service-Bereitstellung erkannt wird, werden Ressourcengruppe und Speicherkonto aufgefüllt und ausgegraut.
   5. Klicken Sie auf **Weiter**, um die Zusammenfassung des Upgrades zu überprüfen.

    ![App Service-Installation erkannt][4]

8. Auf der Zusammenfassungsseite:
   1. Überprüfen Sie Ihre Auswahl. Um Änderungen vorzunehmen, verwenden Sie die Schaltfläche **Zurück**, um auf die vorherigen Seiten zu gelangen.
   2. Wenn die Konfigurationen richtig sind, aktivieren Sie das Kontrollkästchen.
   3. Um das Upgrade zu starten, klicken Sie auf **Weiter**.

       ![App Service-Upgradezusammenfassung][5]

9. Upgradestatusseite:
    1. Verfolgen Sie den Upgradestatus. Die Dauer des Upgrades von App Service in Azure Stack variiert abhängig von der Anzahl der bereitgestellten Rolleninstanzen.
    2. Klicken Sie nach erfolgreichem Abschluss des Upgrades auf **Beenden**.

        ![App Service-Upgradestatus][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Nächste Schritte

Sie können auch weitere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren.

* [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md)
