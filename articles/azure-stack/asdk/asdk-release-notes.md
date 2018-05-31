---
title: Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation
description: Verbesserungen, Fehlerbehebungen und bekannte Probleme in Azure Stack Development Kit
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 9a396c51e8eb6262726b7590498500bfb4528924
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258403"
---
# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit
Die Anmerkungen zu dieser Version enthalten Informationen zu Verbesserungen, Fehlerbehebungen und bekannten Problemen in Azure Stack Development Kit. Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](.\.\azure-stack-updates.md#determine-the-current-version).

> Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.


## <a name="build-201805131"></a>Build 20180513.1

### <a name="new-features"></a>Neue Funktionen 
Dieser Build enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack.  

- <!-- 1759172 - IS, ASDK --> **More granular administrative subscriptions**. With version 1804 and later, the Default Provider subscription is now complemented with two additional subscriptions. The additions facilitate separating the management of core infrastructure, additional resource providers, and workloads. The following three subscriptions are available:
  - *Standardabonnement des Anbieters*. Verwenden Sie dieses Abonnement nur für Kerninfrastruktur. Stellen Sie keine Ressourcen oder Ressourcenanbieter für dieses Abonnement bereit.
  - *Messungsabonnement*. Verwenden Sie dieses Abonnement zur Ressourcenanbieterbereitstellung. Für dieses Abonnement bereitgestellte Ressourcen werden nicht in Rechnung gestellt.
  - *Verbrauchsabonnement*. Verwenden Sie dieses Abonnement für alle anderen Workloads, die Sie bereitstellen möchten. Für hier bereitgestellte Ressourcen werden normale Auslastungspreise berechnet.


### <a name="fixed-issues"></a>Behobene Probleme
- <!-- IS, ASDK -->  In the admin portal, you no longer have to refresh the Update tile before it displays information. 

- <!-- 2050709 - IS, ASDK -->  You can now use the admin portal to edit storage metrics for Blob service, Table service, and Queue service.

- <!-- IS, ASDK --> Under **Networking**, when you click **Connection** to set up a VPN connection, **Site-to-site (IPsec)** is now the only available option. 

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Zusätzliche, zeitlich auf dieses Update abgestimmte Releases  
Die folgenden sind jetzt verfügbar, benötigen jedoch nicht Azure Stack Update 1804.
- **Aktualisieren Sie auf das Microsoft Azure Stack System Center Operations Manager-Überwachungspaket**. Eine neue Version (1.0.3.0) des Microsoft System Center Operations Manager-Überwachungspakets für Azure Stack steht zum [Herunterladen](https://www.microsoft.com/download/details.aspx?id=55184) zur Verfügung. Mit dieser Version können Sie Dienstprinzipale verwenden, wenn Sie eine verbundene Azure Stack-Bereitstellung hinzufügen. Diese Version bietet auch eine Updateverwaltungsoberfläche, über die Sie direkt im Operations Manager Wartungsaktionen ausführen können. Es gibt auch neue Dashboards, die Ressourcenanbieter, Skalierungseinheiten und Skalierungseinheitenknoten anzeigen.

- **Neue Azure Stack Admin PowerShell Version 1.3.0**.  Azure Stack PowerShell 1.3.0 ist jetzt für die Installation verfügbar. Diese Version bietet allen Administratorressourcenanbietern Befehle zum Verwalten von Azure Stack.  Aufgrund dieses Releases sind manche Inhalte aus dem GitHub-[Repository](https://github.com/Azure/AzureStack-Tools) zu Azure Stack-Tools veraltet. 

   Um ausführliche Informationen zur Installation zu erhalten, beachten Sie die [Anweisungen](.\.\azure-stack-powershell-install.md) oder den [Hilfeinhalt](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) für das Azure Stack-Modul 1.3.0. 

- **Erstes Release der Azure Stack-REST-API-Referenz**. Die [API-Referenz für alle Azure Stack-Administratorressourcenanbieter](https://docs.microsoft.com/rest/api/azure-stack/) ist jetzt veröffentlicht. 

### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> The ability [to open a new support request from the dropdown](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) from within the administrator portal isn’t available. Instead, use the following link:     
    - Verwenden Sie https://aka.ms/azurestackforum für das Azure Stack Development Kit.    

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD -  IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.

-   <!-- TBD -  IS ASDK --> In the admin portal, you might see a critical alert for the Microsoft.Update.Admin component. The Alert name, description, and remediation all display as:  
    - *FEHLER: Vorlage für FaultType ResourceProviderTimeout fehlt.*

    Diese Warnung kann ignoriert werden. 

#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a DS series VM. DS series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD -  IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

- <!-- TBD -  IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 - IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 

#### <a name="networking"></a>Netzwerk
- <!-- 1766332 - IS, ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 -  IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer VMs verwenden.

  Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased. 

  Um dieses Problem zu umgehen, verwenden Sie einen Add-On-Plan, um ein Netzwerkkontingent zu erhöhen, wenn der Plan bereits einem Abonnement zugeordnet ist. Weitere Informationen finden Sie in der Beschreibung der Vorgehensweise zum [Bereitstellen eines Add-on-Plans](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription. 


- <!-- 1902460 -  IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 -  IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.
 
- <!-- TBD -  IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.


#### <a name="sql-and-mysql"></a>SQL und MySQL 
- <!-- TBD - ASDK --> The database hosting servers must be dedicated for use by the resource provider and user workloads. You cannot use an instance that is being used by any other consumer, including App Services.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- TBD -  IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.
 
#### <a name="usage"></a>Verwendung  
- <!-- TBD -  IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.

<!-- #### Identity -->






## <a name="build-201803291"></a>Build 20180329.1

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen
Die für integrierte Azure Stack-Systeme Version 1803 veröffentlichten neuen Features und Fixes gelten für das Azure Stack Development Kit. Weitere Informationen finden Sie in den Abschnitten [Neue Features](.\.\azure-stack-update-1803.md#new-features) und [Fehlerbehebungen](.\.\azure-stack-update-1803.md#fixed-issues) der Versionshinweise zum Azure Stack 1803-Update.  
> [!IMPORTANT]    
> Einige der Elemente, die in den Abschnitten **Neue Features** und **Fehlerbehebungen** aufgeführt sind, sind nur für integrierte Azure Stack-Systeme relevant.

### <a name="changes"></a>Änderungen
- Die Methode der Änderung des Zustands eines neu erstellten Angebots von *Privat* in *Öffentlich* oder *Außer Betrieb* hat sich geändert. Weitere Informationen finden Sie unter [Erstellen von Angeboten in Azure Stack](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- Die Funktion, mit der innerhalb des Administratorportals [eine neue Supportanfrage über die Dropdownliste geöffnet wird](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support), ist nicht verfügbar. Verwenden Sie stattdessen den folgenden Link:     
    - Verwenden Sie https://aka.ms/azurestackforum für das Azure Stack Development Kit.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Ihnen wird die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.

- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.

- Im Dashboard des Verwaltungsportals tritt beim Anzeigen von Informationen zu Updates über die Kachel „Aktualisieren“ ein Fehler auf. Klicken Sie zur Behebung dieses Problems auf die Kachel, um sie zu aktualisieren.

-   Im Verwaltungsportal wird unter Umständen eine kritische Warnung für die Komponente „Microsoft.Update.Admin“ angezeigt. Name, Beschreibung und Behebung der Warnung werden wie folgt angezeigt:  
    - *FEHLER: Vorlage für FaultType ResourceProviderTimeout fehlt.*

    Diese Warnung kann ignoriert werden. 



#### <a name="marketplace"></a>Marketplace
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

-  Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Netzwerk
- Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, wird **VNET-zu-VNET** als möglicher Verbindungstyp aufgeführt. Wählen Sie diese Option nicht aus. Derzeit wird nur die Option **Standort-zu-Standort (IPsec)** unterstützt.

- Nachdem Sie eine VM erstellt und einer öffentlichen IP-Adresse zugeordnet haben, können Sie die Zuordnung dieser VM zu dieser IP-Adresse nicht mehr aufheben. Die Aufhebung der Zuordnung war scheinbar erfolgreich, aber die zuvor zugewiesene öffentliche IP-Adresse bleibt der ursprünglichen VM zugeordnet.

  Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer VMs verwenden.

  Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird.



- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.
 
- Azure Stack unterstützt nicht das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer VM-Instanz, nachdem die VM bereitgestellt wurde. Wenn die VM mehr als eine Netzwerkschnittstelle benötigt, müssen diese zum Zeitpunkt der Bereitstellung definiert werden.



#### <a name="sql-and-mysql"></a>SQL und MySQL 
- Es kann bis zu einer Stunde dauern, bis Benutzer Datenbanken in einer neuen SQL- oder MySQL-SKU erstellen können.

- Die Server für das Hosten von Datenbanken müssen für die Verwendung durch die Ressourcenanbieter und Benutzerworkloads dediziert sein. Sie können keine Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.
 
#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Herunterladen von Azure Stack-Tools von GitHub
- Beim Herunterladen der Azure Stack-Tools über GitHub mit dem PowerShell-Cmdlet *invoke-webrequest* ist ein Fehler aufgetreten:     
    -  *invoke-webrequest: Die Anforderung wurde abgebrochen: Es konnte kein sicherer SSL/TLS-Kanal erstellt werden.*     

  Dieser Fehler tritt aufgrund der zurzeit veralteten GitHub-Unterstützung für die kryptografischen Standards TLS v1 und TLS v1.1 auf (der Standardwert für PowerShell). Weitere Informationen finden Sie unter [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Hinweis zur Abschaffung unsicherer kryptografischer Standards).

  Fügen Sie zur Behebung dieses Problems `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` am Anfang des Skripts hinzu, um bei Downloads aus GitHub-Repositorys die Verwendung von TLS v1.2 in der PowerShell-Konsole zu erzwingen.






## <a name="build-201803021"></a>Build 20180302.1

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen
Weitere Informationen finden Sie im Abschnitt [Neue Features und Fehlerbehebungen](.\.\azure-stack-update-1802.md#new-features-and-fixes) in den Versionshinweisen zum Azure Stack Update 1802 für in Azure Stack integrierte Systeme.

> [!IMPORTANT]    
> Einige der Elemente, die im Abschnitt **Neue Features und Fehlerbehebungen** aufgeführt sind, sind nur für integrierte Azure Stack-Systeme relevant.


### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- Die Funktion, mit der innerhalb des Administratorportals [eine neue Supportanfrage über die Dropdownliste geöffnet wird](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support), ist nicht verfügbar. Verwenden Sie stattdessen den folgenden Link:     
    - Verwenden Sie https://aka.ms/azurestackforum für das Azure Stack Development Kit.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Ihnen wird die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.

- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.

- Im Dashboard des Verwaltungsportals tritt beim Anzeigen von Informationen zu Updates über die Kachel „Aktualisieren“ ein Fehler auf. Klicken Sie zur Behebung dieses Problems auf die Kachel, um sie zu aktualisieren.

-   Im Verwaltungsportal wird unter Umständen eine kritische Warnung für die Komponente „Microsoft.Update.Admin“ angezeigt. Name, Beschreibung und Behebung der Warnung werden wie folgt angezeigt:  
    - *FEHLER: Vorlage für FaultType ResourceProviderTimeout fehlt.*

    Diese Warnung kann ignoriert werden. 

- Sowohl im Administrator- als auch im Benutzerportal wird das Blatt „Übersicht“ nicht geladen, wenn Sie auf das Übersichtsblatt für Speicherkonten klicken, die mit einer älteren API-Version (beispielsweise 2015-06-15) erstellt wurden. 

  Führen Sie zur Umgehung dieses Problems mithilfe von PowerShell das Skript **Start-ResourceSynchronization.ps1** aus, um den Zugriff auf die Speicherkontodetails wiederherzustellen. Das Skript steht auf [GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts) zur Verfügung und muss bei Verwendung des ASDKs mit Dienstadministrator-Anmeldeinformationen auf dem Development Kit-Host ausgeführt werden.  

- Das Blatt **Service Health** wird nicht geladen. Beim Öffnen des Blatts „Service Health“ im Verwaltungs- oder Benutzerportal zeigt Azure Stack eine Fehlermeldung an, und Informationen werden nicht geladen. Dieses Verhalten wird erwartet. Sie können Service Health zwar auswählen und öffnen, dieses Feature ist jedoch noch nicht verfügbar, sondern wird in einer zukünftigen Version von Azure Stack implementiert.

#### <a name="health-and-monitoring"></a>Integrität und Überwachung
Im Azure Stack-Verwaltungsportal wird ggf. eine kritische Warnung der Art **Pending external certificate expiration** (Anstehender Ablauf des externen Zertifikats) angezeigt.  Sie können diese Warnung ignorieren. Sie wirkt sich nicht auf die Vorgänge des Azure Stack Development Kit aus. 


#### <a name="marketplace"></a>Marketplace
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- Azure Stack unterstützt nur die Verwendung von VHDs mit fester Größe. Einige über den Marketplace in Azure Stack angebotenen Images verwenden dynamische VHDs, die aber entfernt wurden. Größenänderungen an virtuellen Computern (VMs), bei denen ein dynamischer Datenträger angefügt ist, führen dazu, dass die VM in den Zustand „Fehlerhaft“ wechselt.

  Löschen Sie in diesem Fall den virtuellen Computer, ohne jedoch seinen Datenträger (ein VHD-Blob in einem Speicherkonto) zu löschen. Konvertieren Sie die VHD anschließend von einem dynamischen Datenträger in einen Datenträger mit fester Größe, und erstellen Sie den virtuellen Computer neu.

- Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

-  Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Netzwerk
- Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, wird **VNET-zu-VNET** als möglicher Verbindungstyp aufgeführt. Wählen Sie diese Option nicht aus. Derzeit wird nur die Option **Standort-zu-Standort (IPsec)** unterstützt.

- Nachdem Sie eine VM erstellt und einer öffentlichen IP-Adresse zugeordnet haben, können Sie die Zuordnung dieser VM zu dieser IP-Adresse nicht mehr aufheben. Die Aufhebung der Zuordnung war scheinbar erfolgreich, aber die zuvor zugewiesene öffentliche IP-Adresse bleibt der ursprünglichen VM zugeordnet.

  Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer VMs verwenden.

  Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird.

-   Das Feature zur IP-Weiterleitung wird im Portal angezeigt, aber die Aktivierung der IP-Weiterleitung hat keine Auswirkungen. Dieses Feature wird noch nicht unterstützt.

- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.
 
- Azure Stack unterstützt nicht das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer VM-Instanz, nachdem die VM bereitgestellt wurde. Wenn die VM mehr als eine Netzwerkschnittstelle benötigt, müssen diese zum Zeitpunkt der Bereitstellung definiert werden.



#### <a name="sql-and-mysql"></a>SQL und MySQL 
- Es kann bis zu einer Stunde dauern, bis Benutzer Datenbanken in einer neuen SQL- oder MySQL-SKU erstellen können.

- Die Server für das Hosten von Datenbanken müssen für die Verwendung durch die Ressourcenanbieter und Benutzerworkloads dediziert sein. Sie können keine Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.
 
#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Herunterladen von Azure Stack-Tools von GitHub
- Beim Herunterladen der Azure Stack-Tools über GitHub mit dem PowerShell-Cmdlet *invoke-webrequest* ist ein Fehler aufgetreten:     
    -  *invoke-webrequest: Die Anforderung wurde abgebrochen: Es konnte kein sicherer SSL/TLS-Kanal erstellt werden.*     

  Dieser Fehler tritt aufgrund der zurzeit veralteten GitHub-Unterstützung für die kryptografischen Standards TLS v1 und TLS v1.1 auf (der Standardwert für PowerShell). Weitere Informationen finden Sie unter [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Hinweis zur Abschaffung unsicherer kryptografischer Standards).

  Fügen Sie zur Behebung dieses Problems `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` am Anfang des Skripts hinzu, um bei Downloads aus GitHub-Repositorys die Verwendung von TLS v1.2 in der PowerShell-Konsole zu erzwingen.

