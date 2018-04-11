---
title: Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation
description: Verbesserungen, Fehlerbehebungen und bekannte Probleme in Azure Stack Development Kit
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 1a8cbdef8f3d8a5aa4aeab0e51275933160360c2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit
Die Anmerkungen zu dieser Version enthalten Informationen zu Verbesserungen, Fehlerbehebungen und bekannten Problemen in Azure Stack Development Kit. Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](.\.\azure-stack-updates.md#determine-the-current-version).

> Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.

## <a name="build-201803291"></a>Build 20180329.1

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen
Die für integrierte Azure Stack-Systeme Version 1803 veröffentlichten neuen Features und Fixes gelten für das Azure Stack Development Kit. Weitere Informationen finden Sie in den Abschnitten [Neue Features](.\.\azure-stack-update-1803.md#new-features) und [Fehlerbehebungen](.\.\azure-stack-update-1803.md#fixed-issues) der Versionshinweise zum Azure Stack 1803-Update.  
> [!IMPORTANT]    
> Einige der Elemente, die in den Abschnitten **Neue Features** und **Fehlerbehebungen** aufgeführt sind, sind nur für integrierte Azure Stack-Systeme relevant.

### <a name="changes"></a>Änderungen
- Die Methode der Änderung des Zustands eines neu erstellten Angebots von *privat* zu *öffentlich* oder *außer Betrieb* hat sich geändert. Weitere Informationen finden Sie unter [Erstellen von Angeboten in Azure Stack](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- Die Funktion, mit der innerhalb des Administratorportals [eine neue Supportanfrage über die Dropdownliste geöffnet wird](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support), ist nicht verfügbar. Verwenden Sie stattdessen den folgenden Link:     
    - Verwenden Sie https://aka.ms/azurestackforum für das Azure Stack Development Kit.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressource oder Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcen oder Ressourcengruppen zwischen Ressourcengruppen oder Abonnements wird derzeit nicht unterstützt.
 
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

#### <a name="app-service"></a>App Service
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.
 
#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz an statt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.
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

- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressource oder Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcen oder Ressourcengruppen zwischen Ressourcengruppen oder Abonnements wird derzeit nicht unterstützt.
 
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

#### <a name="app-service"></a>App Service
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.
 
#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz an statt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Herunterladen von Azure Stack-Tools von GitHub
- Beim Herunterladen der Azure Stack-Tools über GitHub mit dem PowerShell-Cmdlet *invoke-webrequest* ist ein Fehler aufgetreten:     
    -  *invoke-webrequest: Die Anforderung wurde abgebrochen: Es konnte kein sicherer SSL/TLS-Kanal erstellt werden.*     

  Dieser Fehler tritt aufgrund der zurzeit veralteten GitHub-Unterstützung für die kryptografischen Standards TLS v1 und TLS v1.1 auf (der Standardwert für PowerShell). Weitere Informationen finden Sie unter [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Hinweis zur Abschaffung unsicherer kryptografischer Standards).

  Fügen Sie zur Behebung dieses Problems `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` am Anfang des Skripts hinzu, um bei Downloads aus GitHub-Repositorys die Verwendung von TLS v1.2 in der PowerShell-Konsole zu erzwingen.




## <a name="build-201801032"></a>Build 20180103.2

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen

- Weitere Informationen finden Sie im Abschnitt [Neue Features und Fehlerbehebungen](.\.\azure-stack-update-1712.md#new-features-and-fixes) in den Versionshinweisen zum Azure Stack Update 1712 für in Azure Stack integrierte Systeme.

    > [!IMPORTANT]
    > Einige der Elemente, die im Abschnitt **Neue Features und Fehlerbehebungen** aufgeführt sind, sind nur für integrierte Azure Stack-Systeme relevant.

### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="deployment"></a>Bereitstellung
- Sie müssen während der Bereitstellung einen Zeitserver mithilfe einer IP-Adresse angeben.

#### <a name="infrastructure-management"></a>Infrastrukturverwaltung
- Belassen Sie die Infrastruktursicherung auf dem Blatt **Infrastructure backup** (Infrastruktursicherung) im deaktivierten Zustand.
- Die IP-Adresse und das Modell des Baseboard-Verwaltungscontrollers (Baseboard Management Controller, BMC) werden nicht in den Hauptinformationen eines Skalierungseinheitknotens angezeigt. Dies ist das erwartete Verhalten für das Azure Stack Development Kit.

#### <a name="portal"></a>Portal
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
-  Für alle Workflows, für die Sie in einer Dropdownliste ein Abonnement, eine Ressourcengruppe oder einen Standort auswählen, können unter Umständen folgende Probleme auftreten:

   - Ggf. wird oben in der Liste eine leere Zeile angezeigt. Es sollte trotzdem möglich sein, ein Element auf normale Weise auszuwählen.
   - Wenn die Dropdownliste mit den Elementen kurz ist, ist es ggf. nicht möglich, die Elementnamen anzuzeigen.
   - Wenn Sie über mehrere Benutzerabonnements verfügen, kann die Dropdownliste für die Ressourcengruppen leer sein. 

   Zum Umgehen der beiden letztgenannten Probleme können Sie den Namen des Abonnements oder der Ressourcengruppe eingeben (falls bekannt), oder Sie können stattdessen PowerShell verwenden.

- Ihnen wird die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.
- Wenn der Link **Komponente** von einer beliebigen **Infrastrukturrollen**-Warnung aus angeklickt wird, wird versucht, das resultierende Blatt **Übersicht** zu laden, aber dabei tritt ein Fehler auf. Darüber hinaus wird kein Timeout für das Blatt **Übersicht** überschritten.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
- Das Blatt **Service Health** wird nicht geladen. Beim Öffnen des Blatts „Service Health“ im Verwaltungs- oder Benutzerportal zeigt Azure Stack eine Fehlermeldung an, und Informationen werden nicht geladen. Dieses Verhalten wird erwartet. Sie können Service Health zwar auswählen und öffnen, dieses Feature ist jedoch noch nicht verfügbar, sondern wird in einer zukünftigen Version von Azure Stack implementiert.
#### <a name="marketplace"></a>Marketplace
- Einige Marketplace-Elemente wurden aus dieser Version aus Kompatibilitätsgründen entfernt. Sie werden nach einer weiteren Überprüfung erneut aktiviert.
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Benutzer haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen. Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt. 
- Sie können eine VM-Verfügbarkeitsgruppe nur mit einer Fehlerdomäne und einer Updatedomäne konfigurieren.
- Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden. Sie können eine Skalierungsgruppe erstellen, indem Sie eine Vorlage verwenden.
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

#### <a name="networking"></a>Netzwerk
- Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen. Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
- Beim Erstellen eines Netzwerklastenausgleichs müssen Sie eine NAT-Regel (Network Address Translation, Netzwerkadressübersetzung) erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
- Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, wird **VNET-zu-VNET** als möglicher Verbindungstyp aufgeführt. Wählen Sie diese Option nicht aus. Derzeit wird nur die Option **Standort-zu-Standort (IPsec)** unterstützt.
- Sie können die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer nicht aufheben, nachdem der virtuelle Computer erstellt und dieser IP-Adresse zugeordnet wurde. Die Aufhebung der Zuordnung war scheinbar erfolgreich, die zuvor zugewiesene öffentliche IP-Adresse bleibt dem ursprünglichen virtuellen Computer jedoch zugeordnet. Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird. Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer virtueller Computer verwenden.
- Azure Stack-Benutzer können möglicherweise VNETs oder Netzwerksicherheitsgruppen nicht bereitstellen, löschen oder ändern. Dieses Problem tritt in erster Linie bei nachfolgenden Updateversuchen desselben Pakets auf. Die Ursache ist ein Packproblem beim Update, das derzeit geprüft wird.
- Der interne Lastenausgleich (ILB) behandelt MAC-Adressen für Back-End-VMs nicht korrekt, wodurch bei der Verwendung von Linux-Instanzen an das Back-End-Netzwerk adressierte Pakete verworfen werden.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

#### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
 
#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz an statt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

#### <a name="identity"></a>Identity

In Umgebungen, die in Azure Active Directory-Verbunddienste (AD FS) bereitgestellt wurden, ist das Konto **azurestack\azurestackadmin** nicht mehr der Besitzer des Standardabonnements des Anbieters. Anstelle der Protokollierung in **Administratorportal/Admin-Verwaltungsendpunkt** mit **azurestack\azurestackadmin** können Sie das Konto **azurestack\cloudadmin** verwenden, um das Standardabonnement des Anbieters zu verwalten und zu verwenden.

> [!IMPORTANT]
> Obwohl das Konto **azurestack\cloudadmin** in Umgebungen, die in AD FS bereitgestellt wurden, der Besitzer des Standardabonnements des Anbieters ist, verfügt es nicht über die Berechtigung für RDP-Verbindungen mit dem Host. Verwenden Sie weiterhin das Konto **azurestack\azurestackadmin** oder das lokale Administratorkonto, um sich bei Bedarf auf dem Host anzumelden, auf diesen zuzugreifen oder ihn zu verwalten.




## <a name="build-201711221"></a>Build 20171122.1

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen

- Weitere Informationen finden Sie im Abschnitt [Neue Features und Fehlerbehebungen](.\.\azure-stack-update-1711.md#new-features-and-fixes) in den Versionshinweisen zum Azure Stack Update 1711 für integrierte Azure Stack-Systeme.

    > [!IMPORTANT]
    > Einige der Elemente, die im Abschnitt **Neue Features und Fehlerbehebungen** aufgeführt sind, sind nur für integrierte Azure Stack-Systeme relevant.

### <a name="known-issues"></a>Bekannte Probleme

 
#### <a name="deployment"></a>Bereitstellung
- Sie müssen während der Bereitstellung einen Zeitserver mithilfe einer IP-Adresse angeben.
- Ab Version 1711 ist **CloudAdmin** ein reservierter Kontoname, der bei der Bereitstellung des Development Kits nicht manuell angegeben werden darf. 

#### <a name="infrastructure-management"></a>Infrastrukturverwaltung
- Belassen Sie die Infrastruktursicherung auf dem Blatt **Infrastructure backup** (Infrastruktursicherung) im deaktivierten Zustand.
- Die IP-Adresse und das Modell des Baseboard-Verwaltungscontrollers (Baseboard Management Controller, BMC) werden nicht in den Hauptinformationen eines Skalierungseinheitknotens angezeigt. Dies ist das erwartete Verhalten für das Azure Stack Development Kit.

#### <a name="portal"></a>Portal
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
-  Für alle Workflows, für die Sie in einer Dropdownliste ein Abonnement, eine Ressourcengruppe oder einen Standort auswählen, können unter Umständen folgende Probleme auftreten:

   - Ggf. wird oben in der Liste eine leere Zeile angezeigt. Es sollte trotzdem möglich sein, ein Element auf normale Weise auszuwählen.
   - Wenn die Dropdownliste mit den Elementen kurz ist, ist es ggf. nicht möglich, die Elementnamen anzuzeigen.
   - Wenn Sie über mehrere Benutzerabonnements verfügen, kann die Dropdownliste für die Ressourcengruppen leer sein. 

   Zum Umgehen der beiden letztgenannten Probleme können Sie den Namen des Abonnements oder der Ressourcengruppe eingeben (falls bekannt), oder Sie können stattdessen PowerShell verwenden.

- Ihnen wird die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.
- Wenn der Link **Komponente** von einer beliebigen **Infrastrukturrollen**-Warnung aus angeklickt wird, wird versucht, das resultierende Blatt **Übersicht** zu laden, aber dabei tritt ein Fehler auf. Darüber hinaus wird kein Timeout für das Blatt **Übersicht** überschritten.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
- Das Blatt **Service Health** wird nicht geladen. Beim Öffnen des Blatts „Service Health“ im Verwaltungs- oder Benutzerportal zeigt Azure Stack eine Fehlermeldung an, und Informationen werden nicht geladen. Dieses Verhalten wird erwartet. Sie können Service Health zwar auswählen und öffnen, dieses Feature ist jedoch noch nicht verfügbar, sondern wird in einer zukünftigen Version von Azure Stack implementiert.

#### <a name="marketplace"></a>Marketplace
- Wenn Sie versuchen, dem Azure Stack-Marketplace mit der Option **Add from Azure** (Aus Azure hinzufügen) Elemente hinzuzufügen, werden unter Umständen nicht alle Elemente zum Herunterladen angezeigt.
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Benutzer haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen. Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt. 
- Sie können eine VM-Verfügbarkeitsgruppe nur mit einer Fehlerdomäne und einer Updatedomäne konfigurieren.
- Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden. Sie können eine Skalierungsgruppe erstellen, indem Sie eine Vorlage verwenden.
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

#### <a name="networking"></a>Netzwerk
- Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen. Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
- Beim Erstellen eines Netzwerklastenausgleichs müssen Sie eine NAT-Regel (Network Address Translation, Netzwerkadressübersetzung) erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
- Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, wird **VNET-zu-VNET** als möglicher Verbindungstyp aufgeführt. Wählen Sie diese Option nicht aus. Derzeit wird nur die Option **Standort-zu-Standort (IPsec)** unterstützt.
- Sie können die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer nicht aufheben, nachdem der virtuelle Computer erstellt und dieser IP-Adresse zugeordnet wurde. Die Aufhebung der Zuordnung war scheinbar erfolgreich, die zuvor zugewiesene öffentliche IP-Adresse bleibt dem ursprünglichen virtuellen Computer jedoch zugeordnet. Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird. Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer virtueller Computer verwenden.
- Azure Stack-Benutzer können möglicherweise VNETs oder Netzwerksicherheitsgruppen nicht bereitstellen, löschen oder ändern. Dieses Problem tritt in erster Linie bei nachfolgenden Updateversuchen desselben Pakets auf. Die Ursache ist ein Packproblem beim Update, das derzeit geprüft wird.
- Der interne Lastenausgleich (ILB) behandelt MAC-Adressen für Back-End-VMs nicht korrekt, wodurch bei der Verwendung von Linux-Instanzen an das Back-End-Netzwerk adressierte Pakete verworfen werden.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

    > [!NOTE]
    > Informationen zur Versionskompatibilität finden Sie in den einzelnen Artikeln zur Einrichtung von [SQL](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy) und [MySQL](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy).

#### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

#### <a name="usage"></a>Verwendung  
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz an statt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

#### <a name="identity"></a>Identity

In Umgebungen, die in Azure Active Directory-Verbunddienste (AD FS) bereitgestellt wurden, ist das Konto **azurestack\azurestackadmin** nicht mehr der Besitzer des Standardabonnements des Anbieters. Anstelle der Protokollierung in **Administratorportal/Admin-Verwaltungsendpunkt** mit **azurestack\azurestackadmin** können Sie das Konto **azurestack\cloudadmin** verwenden, um das Standardabonnement des Anbieters zu verwalten und zu verwenden.

> [!IMPORTANT]
> Obwohl das Konto **azurestack\cloudadmin** in Umgebungen, die in AD FS bereitgestellt wurden, der Besitzer des Standardabonnements des Anbieters ist, verfügt es nicht über die Berechtigung für RDP-Verbindungen mit dem Host. Verwenden Sie weiterhin das Konto **azurestack\azurestackadmin** oder das lokale Administratorkonto, um sich bei Bedarf auf dem Host anzumelden, auf diesen zuzugreifen oder ihn zu verwalten.

