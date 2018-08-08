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
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 2f732dfdfe9bf4aff2753114c3041f8f646421c2
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344731"
---
# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit
Die Anmerkungen zu dieser Version enthalten Informationen zu Verbesserungen, Fehlerbehebungen und bekannten Problemen in Azure Stack Development Kit. Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](.\.\azure-stack-updates.md#determine-the-current-version).

> Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.

## <a name="build-11805142"></a>Build 1.1805.1.42

<!-- ### New features 
This build includes the following improvements and fixes for Azure Stack.  
-->


### <a name="fixed-issues"></a>Behobene Probleme

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> Sie können mit dieser Version von Azure Stack keine Treiberupdates mit einem OEM-Erweiterungspaket durchführen.  Dafür gibt es keine Problemumgehung.
 
- <!-- TBD - IS ASDK --> Verwenden Sie nicht die neuen administrativen Abonnementtypen *Messungsabonnement* und *Verbrauchsabonnement*. Diese neuen Abonnementtypen wurden mit der Version 1804 eingeführt, sind aber noch nicht zur Verwendung bereit. Sie sollten den Abonnementtyp *Standardanbieter* weiterhin verwenden.  

- <!-- 2403291 - IS ASDK --> Möglicherweise haben Sie die horizontale Bildlaufleiste am unteren Rand des Verwaltungs- und Benutzerportals nicht verwendet. Wenn Sie nicht auf die horizontale Bildlaufleiste zugreifen können, verwenden Sie die Breadcrumb-Leiste, um zu einem vorherigen Blatt im Portal zu navigieren. Wählen Sie hierzu den Namen des Blattes, das Sie anzeigen möchten, aus der Breadcrumb-Liste oben links im Portal aus.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

- <!-- TBD -  IS ASDK --> Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.


#### <a name="health-and-monitoring"></a>Integrität und Überwachung
- <!-- 1264761 - IS ASDK --> Möglicherweise werden Warnungen für die *Health Controller*-Komponente mit folgenden Details angezeigt:  

   Warnung 1:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Heartbeat Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.  

  Warnung 2:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Fault Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.

  Beide Warnungen können ignoriert werden. Sie werden nach einem bestimmten Zeitraum automatisch geschlossen.  

#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- <!-- TBD -  IS ASDK --> Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie D angefügt werden können. Alle unterstützten virtuellen Computer der Serie D können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- <!-- TBD -  IS ASDK --> Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

- <!-- TBD -  IS ASDK --> Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 - IS ASDK --> Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren. 

#### <a name="networking"></a>Netzwerk
- <!-- 1766332 - IS, ASDK --> Wenn Sie unter **Netzwerk** auf **VPN-Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

- <!-- 2388980 -  IS ASDK --> Nachdem Sie eine VM erstellt und einer öffentlichen IP-Adresse zugeordnet haben, können Sie die Zuordnung dieser VM zu dieser IP-Adresse nicht mehr aufheben. Die Aufhebung der Zuordnung war scheinbar erfolgreich, aber die zuvor zugewiesene öffentliche IP-Adresse bleibt der ursprünglichen VM zugeordnet.

  Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer VMs verwenden.

  Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird.

- <!-- 2292271 - IS ASDK --> Wenn Sie eine Kontingentgrenze für eine Netzwerkressource heraufsetzen, die Teil eines Angebots und eines Plans ist, das bzw. der mit einem Mandantenabonnement verbunden ist, wird die neue Grenze nicht auf dieses Abonnement angewendet. Der neue Grenzwert gilt jedoch für neue Abonnements, die nach Heraufsetzen der Kontingentgrenze erstellt werden. 

  Um dieses Problem zu umgehen, verwenden Sie einen Skalierungsplan, um ein Netzwerkkontingent zu erhöhen, wenn der Plan bereits einem Abonnement zugeordnet ist. Weitere Informationen finden Sie in der Beschreibung der Vorgehensweise zum [Bereitstellen eines Skalierungsplans](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Ein Abonnement, dem DNS-Zonen- oder Routingtabellenressourcen zugeordnet wurden, kann nicht gelöscht werden. Um das Abonnement erfolgreich zu löschen, müssen Sie zuerst die DNS-Zonen- und Routingtabellenressourcen aus dem Mandantenabonnement löschen. 


- <!-- 1902460 -  IS ASDK -->Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

- <!-- 16309153 -  IS ASDK --> In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.
 
- <!-- TBD -  IS ASDK --> Azure Stack unterstützt nicht das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer VM-Instanz, nachdem die VM bereitgestellt wurde. Wenn die VM mehr als eine Netzwerkschnittstelle benötigt, müssen diese zum Zeitpunkt der Bereitstellung definiert werden.


#### <a name="sql-and-mysql"></a>SQL und MySQL 
- <!-- TBD - ASDK --> Die Server für das Hosten von Datenbanken müssen für die Verwendung durch die Ressourcenanbieter und Benutzerworkloads dediziert sein. Sie können keine Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.

- <!-- IS, ASDK --> Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- <!-- TBD - IS ASDK --> Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.  

- <!-- TBD - IS ASDK --> App Service kann derzeit nur im *Abonnement für Standardanbieter* bereitgestellt werden. In einem zukünftigen Update wird App Service in das neue *Messungsabonnement* integriert, das in Azure Stack 1804 eingeführt wurde. Wenn Messungen für die Verwendung unterstützt werden, migrieren alle vorhandenen Bereitstellungen zu diesem neuen Abonnementtyp.

#### <a name="usage"></a>Verwendung  
- <!-- TBD -  IS ASDK --> Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

<!-- #### Identity -->



## <a name="build-201805131"></a>Build 20180513.1

### <a name="new-features"></a>Neue Funktionen 
Dieser Build enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack.  

- <!-- 1759172 - IS, ASDK --> **Neue Verwaltungsabonnements**. Mit 1804 gibt es im Portal zwei neue Abonnementtypen. Diese neuen Abonnementtypen sind zusätzlich zum Standardabonnement des Anbieters verfügbar und werden in neuen Azure Stack-Installationen ab Version 1804 angezeigt. *Verwenden Sie diese neuen Abonnementtypen nicht mit dieser Azure Stack-Version*. Die Verfügbarkeit dieser Abonnementtypen wird in einem zukünftigen Update bekanntgegeben. 

  Diese neuen Abonnementtypen werden zwar angezeigt, sind jedoch Bestandteil einer größeren Änderung, um das Abonnement für Standardanbieter zu sichern und die Bereitstellung von freigegebenen Ressourcen wie SQL-Hostserver zu vereinfachen. 

  Derzeit sind drei Abonnementtypen verfügbar:  
  - Abonnement für Standardanbieter: Diesen Abonnementtyp können Sie weiterhin verwenden. 
  - Abonnement für Messungen: *Verwenden Sie diesen Abonnementtyp nicht.*
  - Abonnement für die Nutzung: *Verwenden Sie diesen Abonnementtyp nicht.*

### <a name="fixed-issues"></a>Behobene Probleme
- <!-- IS, ASDK --> Im Verwaltungsportal müssen Sie die Kachel „Aktualisieren“ nicht mehr aktualisieren, bevor diese Informationen anzeigt. 

- <!-- 2050709 - IS, ASDK --> Sie können nun über das Verwaltungsportal Speichermetriken für Blob-Dienst, Tabellenspeicherdienst und Warteschlangendienst bearbeiten.

- <!-- IS, ASDK --> Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, ist **Site-to-site (IPsec)** nun die einzige verfügbare Option. 

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Zusätzliche, zeitlich auf dieses Update abgestimmte Releases  
Die folgenden Updates sind jetzt verfügbar, benötigen jedoch nicht Azure Stack Update 1804.
- **Update für das Microsoft Azure Stack System Center Operations Manager-Überwachungspaket**. Eine neue Version (1.0.3.0) des Microsoft System Center Operations Manager-Überwachungspakets für Azure Stack steht zum [Herunterladen](https://www.microsoft.com/download/details.aspx?id=55184) zur Verfügung. Mit dieser Version können Sie Dienstprinzipale verwenden, wenn Sie eine verbundene Azure Stack-Bereitstellung hinzufügen. Diese Version bietet auch eine Updateverwaltungsoberfläche, mit der Sie die Wartungsaktion direkt in Operations Manager ausführen können. Es gibt auch neue Dashboards, die Ressourcenanbieter, Skalierungseinheiten und Skalierungseinheitenknoten anzeigen.

- **Neue Azure Stack Admin PowerShell Version 1.3.0**.  Azure Stack PowerShell 1.3.0 ist jetzt für die Installation verfügbar. Diese Version stellt Befehle für alle Administratorressourcenanbieter zum Verwalten von Azure Stack zur Verfügung.  Bei diesem Release werden manche Inhalte aus dem GitHub-[Repository](https://github.com/Azure/AzureStack-Tools) für Azure Stack-Tools eingestellt. 

   Um ausführliche Informationen zur Installation zu erhalten, beachten Sie die [Anweisungen](.\.\azure-stack-powershell-install.md) oder den [Hilfeinhalt](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) für das Azure Stack-Modul 1.3.0. 

- **Erstes Release der Azure Stack-REST-API-Referenz**. Die [API-Referenz für alle Azure Stack-Administratorressourcenanbieter](https://docs.microsoft.com/rest/api/azure-stack/) ist jetzt veröffentlicht. 

### <a name="known-issues"></a>Bekannte Probleme
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> Sie können mit dieser Version von Azure Stack keine Treiberupdates mit einem OEM-Erweiterungspaket durchführen.  Dafür gibt es keine Problemumgehung.
 
- <!-- TBD - IS ASDK --> Die Funktion, mit der innerhalb des Administratorportals [eine neue Supportanfrage über die Dropdownliste geöffnet wird](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support), ist nicht verfügbar. Verwenden Sie stattdessen den folgenden Link:     
    - Verwenden Sie https://aka.ms/azurestackforum für das Azure Stack Development Kit.    

- <!-- 2403291 - IS ASDK --> Möglicherweise haben Sie die horizontale Bildlaufleiste am unteren Rand des Verwaltungs- und Benutzerportals nicht verwendet. Wenn Sie nicht auf die horizontale Bildlaufleiste zugreifen können, verwenden Sie die Breadcrumb-Leiste, um zu einem vorherigen Blatt im Portal zu navigieren. Wählen Sie hierzu den Namen des Blattes, das Sie anzeigen möchten, aus der Breadcrumb-Liste oben links im Portal aus.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

- <!-- TBD -  IS ASDK --> Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.

-   <!-- TBD -  IS ASDK -->Im Verwaltungsportal wird möglicherweise eine kritische Warnung für die Komponente „Microsoft.Update.Admin“ angezeigt. Name, Beschreibung und Behebung der Warnung werden wie folgt angezeigt:  
    - *FEHLER: Vorlage für FaultType ResourceProviderTimeout fehlt.*

    Diese Warnung kann ignoriert werden. 

#### <a name="health-and-monitoring"></a>Integrität und Überwachung
- <!-- 1264761 - IS ASDK --> Möglicherweise werden Warnungen für die *Health Controller*-Komponente mit folgenden Details angezeigt:  

   Warnung 1:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Heartbeat Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.  

  Warnung 2:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Fault Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.

  Beide Warnungen können ignoriert werden. Sie werden nach einem bestimmten Zeitraum automatisch geschlossen.  

#### <a name="marketplace"></a>Marketplace
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- <!-- TBD -  IS ASDK --> Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- <!-- TBD -  IS ASDK --> Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

- <!-- TBD -  IS ASDK --> Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 - IS ASDK --> Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren. 

#### <a name="networking"></a>Netzwerk
- <!-- 1766332 - IS, ASDK --> Wenn Sie unter **Netzwerk** auf **VPN-Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

- <!-- 2388980 -  IS ASDK --> Nachdem Sie eine VM erstellt und einer öffentlichen IP-Adresse zugeordnet haben, können Sie die Zuordnung dieser VM zu dieser IP-Adresse nicht mehr aufheben. Die Aufhebung der Zuordnung war scheinbar erfolgreich, aber die zuvor zugewiesene öffentliche IP-Adresse bleibt der ursprünglichen VM zugeordnet.

  Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer VMs verwenden.

  Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird.

- <!-- 2292271 - IS ASDK --> Wenn Sie eine Kontingentgrenze für eine Netzwerkressource heraufsetzen, die Teil eines Angebots und eines Plans ist, das bzw. der mit einem Mandantenabonnement verbunden ist, wird die neue Grenze nicht auf dieses Abonnement angewendet. Der neue Grenzwert gilt jedoch für neue Abonnements, die nach Heraufsetzen der Kontingentgrenze erstellt werden. 

  Um dieses Problem zu umgehen, verwenden Sie einen Skalierungsplan, um ein Netzwerkkontingent zu erhöhen, wenn der Plan bereits einem Abonnement zugeordnet ist. Weitere Informationen finden Sie in der Beschreibung der Vorgehensweise zum [Bereitstellen eines Skalierungsplans](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Ein Abonnement, dem DNS-Zonen- oder Routingtabellenressourcen zugeordnet wurden, kann nicht gelöscht werden. Um das Abonnement erfolgreich zu löschen, müssen Sie zuerst die DNS-Zonen- und Routingtabellenressourcen aus dem Mandantenabonnement löschen. 


- <!-- 1902460 -  IS ASDK -->Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

- <!-- 16309153 -  IS ASDK --> In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.
 
- <!-- TBD -  IS ASDK --> Azure Stack unterstützt nicht das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer VM-Instanz, nachdem die VM bereitgestellt wurde. Wenn die VM mehr als eine Netzwerkschnittstelle benötigt, müssen diese zum Zeitpunkt der Bereitstellung definiert werden.


#### <a name="sql-and-mysql"></a>SQL und MySQL 
- <!-- TBD - ASDK --> Die Server für das Hosten von Datenbanken müssen für die Verwendung durch die Ressourcenanbieter und Benutzerworkloads dediziert sein. Sie können keine Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.

- <!-- IS, ASDK --> Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- <!-- TBD -  IS ASDK --> Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.
 
#### <a name="usage"></a>Verwendung  
- <!-- TBD -  IS ASDK --> Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Herunterladen von Azure Stack-Tools von GitHub
- Beim Herunterladen der Azure Stack-Tools über GitHub mit dem PowerShell-Cmdlet *invoke-webrequest* ist ein Fehler aufgetreten:     
    -  *invoke-webrequest: Die Anforderung wurde abgebrochen: Es konnte kein sicherer SSL/TLS-Kanal erstellt werden.*     

  Dieser Fehler tritt aufgrund der zurzeit veralteten GitHub-Unterstützung für die kryptografischen Standards TLS v1 und TLS v1.1 auf (der Standardwert für PowerShell). Weitere Informationen finden Sie unter [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Hinweis zur Abschaffung unsicherer kryptografischer Standards).

<!-- #### Identity -->



## <a name="build-201803021"></a>Build 20180302.1

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

- <!-- 2050709 --> Es ist nicht möglich, im Verwaltungsportal Speichermetriken für Blob-Dienst, Tabellenspeicherdienst oder Warteschlangendienst zu bearbeiten. Wenn Sie zum Speicher navigieren und dann die Kachel für Blob-Dienst, Tabellenspeicherdienst oder Warteschlangendienst auswählen, wird ein neues Blatt mit dem Metrikdiagramm für diesen Dienst geöffnet. Wenn Sie anschließend oben in der Kachel „Metrikdiagramm“ die Option „Bearbeiten“ auswählen, wird das Blatt "Diagramm bearbeiten" geöffnet, welches jedoch keine Optionen zum Bearbeiten von Metriken anzeigt.  

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
- <!-- 1264761 - IS ASDK --> Möglicherweise werden Warnungen für die *Health Controller*-Komponente mit folgenden Details angezeigt:  

   Warnung 1:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Heartbeat Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.  

  Warnung 2:
   - NAME: Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Fault Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.

  Beide Warnungen können ignoriert werden. Sie werden nach einem bestimmten Zeitraum automatisch geschlossen.  

- Im Azure Stack-Verwaltungsportal wird ggf. eine kritische Warnung der Art **Pending external certificate expiration** (Anstehender Ablauf des externen Zertifikats) angezeigt.  Sie können diese Warnung ignorieren. Sie wirkt sich nicht auf die Vorgänge des Azure Stack Development Kit aus. 


#### <a name="marketplace"></a>Marketplace
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, erneut herunterzuladen.

-  Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 --> Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren. 


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

- <!-- IS, ASDK --> Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** oder **Ebene** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen.

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
