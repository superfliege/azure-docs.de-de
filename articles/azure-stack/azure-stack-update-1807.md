---
title: Azure Stack-Update 1807 | Microsoft-Dokumentation
description: Erfahren Sie, welche Neuerungen im Update 1807 für integrierte Azure Stack-Systeme enthalten sind, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: fe37a62d364d53d03a232b6fdc41e38a9ae2e30f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227674"
---
# <a name="azure-stack-1807-update"></a>Azure Stack-Update 1807

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel beschreibt den Inhalt des Updatepakets 1807. Dieses Update enthält Verbesserungen, Fehlerbehebungen und bekannten Probleme für diese Version von Azure Stack sowie Informationen darüber, wo das Update heruntergeladen werden kann. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]        
> Dieses Updatepaket gilt nur für integrierte Azure Stack-Systeme. Wenden Sie dieses Updatepaket nicht auf das Azure Stack Development Kit an.

## <a name="build-reference"></a>Buildreferenz    
Die Buildnummer des Azure Stack-Updates 1807 ist **1.1807.XX.X**.  


### <a name="new-features"></a>Neue Funktionen
Dieses Update enthält die folgenden Verbesserungen für Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Starten von Sicherungen anhand eines vordefinierten Zeitplans**: Als Appliance kann Azure Stack nun automatisch Infrastruktursicherungen regelmäßig auslösen, um Benutzereingriffe zu eliminieren. Zudem bereinigt Azure Stack auch automatisch die externe Freigabe für Sicherungen, die älter als die definierte Aufbewahrungsdauer sind. 

- <!-- 2496385 | ASDK, IS --> **Zur Gesamtsicherungszeit wurde die Datenübertragungzeit hinzugefügt.**

-   <!-- 1702130 | ASDK, IS --> **Die externe Kapazität für Sicherungen zeigt jetzt die richtige Kapazität der externen Freigabe an.** (Zuvor war diese hardcodiert und zeigte 10 GB an.)

- <!-- 2508488 |  IS   --> Die Kapazität lässt sich durch [Hinzufügen zusätzlicher Knoten von Skalierungseinheiten](azure-stack-add-scale-node.md) erweitern.



### <a name="fixed-issues"></a>Behobene Probleme

- <!-- 448955 | IS ASDK --> Sie können jetzt erfolgreich Aktivitätsprotokolle für Systeme abfragen, die in einer UTC+N-Zeitzone bereitgestellt sind.    

- <!-- 2319627 |  ASDK, IS --> Die Vorprüfung der Sicherungskonfigurationsparameter (Pfad/Benutzername/Kennwort/Verschlüsselungsschlüssel) legt keine falschen Einstellungen mehr in der Sicherungskonfiguration fest. (Früher wurden für die Sicherung falsche Einstellungen festgelegt, wodurch beim Auslösen der Sicherung ein Fehler aufgetreten ist.)

- <!-- 2215948 |  ASDK, IS --> Die Sicherungsliste wird nun aktualisiert, wenn Sie die Sicherung manuell von der externen Freigabe löschen.

- <!-- 2332636 - IS --> Das Update auf diese Version setzt den Standardbesitzer des Standardanbieterabonnements nicht mehr auf den integrierten CloudAdmin-Benutzer zurück, wenn es mit AD FS bereitgestellt wird.

- <!-- 2360715 |  ASDK, IS --> Wenn Sie die Rechenzentrumsintegration einrichten, greifen Sie nicht mehr von einer Freigabe auf die AD FS-Metadatendatei zu. Weitere Informationen finden Sie unter [Einrichten der AD FS-Integration durch Bereitstellen einer Verbundmetadatendatei](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Es wurde ein Problem behoben, das verhinderte, dass Benutzer eine vorhandene öffentliche IP-Adresse, die zuvor einer Netzwerkschnittstelle oder einem Load Balancer zugewiesen war, einer neuen Netzwerkschnittstelle oder einem neuen Load Balancer zuweisen.  

-   <!--2292271 | ASDK, IS --> Es wurde ein Problem behoben, bei dem ein geänderter Kontingentgrenzwert nicht für bestehende Abonnements galt.  Wenn Sie nun einen Kontingentgrenzwert für eine Netzwerkressource erhöhen, die Teil eines mit einem Mandantenabonnement verknüpften Angebots und Plans ist, gilt der neue Grenzwert sowohl für die bereits bestehenden als auch für neue Abonnements.

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.


<!-- ### Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Voraussetzungen    

### <a name="prerequisites"></a>Voraussetzungen
- Installieren Sie das Azure Stack-[Update 1805](azure-stack-update-1805.md), bevor Sie das Azure Stack-Update 1807 anwenden.  Es gab kein Update 1806.  

- Führen Sie vor Beginn der Installation von Update 1807 [Test-AzureStack](azure-stack-diagnostic-test.md) aus, um den Status Ihrer Azure Stack-Instanz zu überprüfen und ggf. Betriebsprobleme zu beheben. Überprüfen Sie auch aktive Warnungen, und lösen Sie solche auf, die eine Aktion erfordern.

### <a name="known-issues-with-the-update-process"></a>Bekannte Probleme mit dem Updateprozess   
- <!-- 2468613 - IS --> Während der Installation des Updates werden möglicherweise Warnungen mit folgender Meldung angezeigt: *Fehler: Die Vorlage für FaultType UserAccounts.New fehlt.*  Sie können diese Warnungen ignorieren. Diese Warnungen werden automatisch nach der Installation dieses Updates geschlossen.   

- <!-- 2489559 - IS --> Versuchen Sie nicht, während der Installation dieses Updates virtuelle Computer zu erstellen. Weitere Informationen zum Verwalten von Updates finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Schritte nach dem Update
*Für Update 1807 sind keine weiteren Schritte nach dem Update erforderlich.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)
Im Folgenden werden bekannte Probleme nach der Installation zu dieser Buildversion vorgestellt.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Wenn Sie im Verwaltungs- oder Benutzerportal **Übersicht** für ein Speicherkonto auswählen, werden die Informationen aus dem Bereich *Zusammenfassung* nicht angezeigt.  Im Bereich „Zusammenfassung“ werden Informationen über das Konto angezeigt, z.B. *Ressourcengruppe*, *Speicherort*, und *Abonnement-ID*.  Weitere Optionen für die Übersicht sind verfügbar, z.B. *Dienste* und *Überwachung* sowie Optionen zum *Öffnen im Explorer* oder zum *Löschen eines Speicherkontos*.

  Nicht verfügbare Informationen lassen sich mit dem PowerShell-Cmdlet [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) anzeigen.

- <!-- 2551834 - IS, ASDK --> Wenn Sie im Verwaltungs- oder Benutzerportal **Tags** für ein Speicherkonto auswählen, werden die Informationen nicht geladen und nicht angezeigt.  

  Nicht verfügbare Informationen lassen sich mit dem PowerShell-Cmdlet [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) anzeigen. 

- <!-- TBD - IS ASDK --> Einige administrative Abonnementtypen sind nicht verfügbar.  Wenn Sie Azure Stack auf diese Version aktualisieren, werden die zwei Abonnementtypen, die [mit Version 1804 eingeführt wurden](azure-stack-update-1804.md#new-features), nicht in der Konsole angezeigt. Dies entspricht dem erwarteten Verhalten. Die nicht verfügbaren Abonnementtypen sind *Messungsabonnement* und *Verbrauchsabonnement*. Diese Abonnementtypen sind in neuen Azure Stack-Umgebungen ab Version 1804 sichtbar, aber noch nicht zur Verwendung bereit. Sie sollten den Abonnementtyp *Standardanbieter* weiterhin verwenden.  

- <!-- 2403291 - IS ASDK --> Möglicherweise haben Sie die horizontale Bildlaufleiste am unteren Rand des Verwaltungs- und Benutzerportals nicht verwendet. Wenn Sie nicht auf die horizontale Bildlaufleiste zugreifen können, verwenden Sie die Breadcrumb-Leiste, um zu einem vorherigen Blatt im Portal zu navigieren. Wählen Sie hierzu den Namen des Blattes, das Sie anzeigen möchten, aus der Breadcrumb-Liste oben links im Portal aus.
  ![Breadcrumb](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Es ist u.U. nicht möglich, Compute- oder Speicherressourcen im Administratorportal anzuzeigen. Grund für dieses Problem ist ein Fehler bei der Installation des Updates, der dazu führt, dass das Update fälschlicherweise als erfolgreich gemeldet wird. Wenn dieses Problem auftritt, wenden Sie sich an Microsoft Customer Support Services, um Unterstützung zu erhalten.

- <!-- TBD - IS --> Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.

- <!-- TBD - IS ASDK --> Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

- <!-- TBD - IS ASDK --> Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.


### <a name="health-and-monitoring"></a>Integrität und Überwachung
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

  Beide Alarme können ignoriert werden und werden nach einer gewissen Zeit automatisch geschlossen.  

- <!-- 2368581 - IS. ASDK --> Wenn Sie als Azure Stack-Bediener eine Fehlermeldung über unzureichenden Speicher erhalten und virtuelle Computer der Mandanten nicht mit einem *Fabric-VM-Erstellungsfehler* bereitgestellt werden können, verfügt der Azure Stack-Stempel möglicherweise nicht über genügend Arbeitsspeicher. Mit dem [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) können Sie die verfügbare Kapazität für Ihre Workloads besser bestimmen.


### <a name="compute"></a>Compute
- <!-- 2724873 - IS --> Wenn Sie die PowerShell-Cmdlets **Start-AzsScaleUnitNode** oder **Stop-AzsScaleunitNode** verwenden, um Skalierungseinheiten zu verwalten, tritt beim ersten Versuch, die Skalierungseinheit zu starten oder zu stoppen, möglicherweise ein Fehler auf. Wenn beim ersten Ausführen des Cmdlets ein Fehler auftritt, führen Sie das Cmdlet ein zweites Mal aus. Bei der zweiten Ausführung sollte der Vorgang erfolgreich sein. 

- <!-- 2494144 - IS, ASDK --> Bei der Auswahl der Größe des virtuellen Computers für die Bereitstellung eines virtuellen Computers sind einige VM-Größen der Serie F beim Erstellen einer VM nicht als Teil der Größenauswahl sichtbar. Folgende VM-Größen werden im Selektor nicht angezeigt: *F8s_v2*, *F16s_v2*, *F32s_v2* und *F64s_v2*.  
  Um das Problem zu umgehen, verwenden Sie eine der folgenden Methoden zum Bereitstellen einer VM. Bei jeder Methode müssen Sie die gewünschte VM-Größe angeben.

  - **Azure Resource Manager-Vorlage**: Wenn Sie eine Vorlage verwenden, legen Sie *vmSize* in der Vorlage entsprechend der gewünschten VM-Größe fest. Der folgende Eintrag z.B. wird zum Bereitstellen einer VM mit der Größe *F32s_v2* verwendet:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI**: Sie können den Befehl [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) verwenden und die VM-Größe als Parameter angeben, ähnlich wie `--size "Standard_F32s_v2"`.

  - **PowerShell**: Mit PowerShell können Sie [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) mit dem Parameter verwenden, der die VM-Größe angibt, ähnlich wie `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

- <!-- TBD - IS --> Wenn Sie im Portal unter **Neu** > **Berechnen** > **Verfügbarkeitsgruppe** eine Verfügbarkeitsgruppe erstellen, können Sie nur eine Verfügbarkeitsgruppe mit einer Fehlerdomäne und einer Updatedomäne erstellen. Erstellen Sie bei der Erstellung eines neuen virtuellen Computers zur Problemumgehung die Verfügbarkeitsgruppe mithilfe von PowerShell, der CLI oder im Portal.

- <!-- TBD - IS ASDK --> Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- <!-- TBD - IS ASDK --> Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie erneut versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, herunterzuladen.

- <!-- TBD - IS ASDK --> Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

- <!-- 1662991 IS ASDK --> Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren.  


### <a name="networking"></a>Netzwerk  

- <!-- 1766332 - IS ASDK --> Wenn Sie unter **Netzwerk** auf **VPN-Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

- <!-- 2304134 IS ASDK --> Ein Abonnement, dem DNS-Zonen- oder Routingtabellenressourcen zugeordnet wurden, kann nicht gelöscht werden. Um das Abonnement erfolgreich zu löschen, müssen Sie zuerst die DNS-Zonen- und Routingtabellenressourcen aus dem Mandantenabonnement löschen.


- <!-- 1902460 - IS ASDK -->Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

- <!-- 16309153 - IS ASDK --> In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.

- <!-- TBD - IS ASDK --> Azure Stack unterstützt nicht das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer VM-Instanz, nachdem die VM bereitgestellt wurde. Wenn die VM mehr als eine Netzwerkschnittstelle benötigt, müssen diese zum Zeitpunkt der Bereitstellung definiert werden.

- <!-- 2096388 IS --> Sie können nicht über das Verwaltungsportal Regeln für eine Netzwerksicherheitsgruppe aktualisieren.

    Problemumgehung für App Service: Wenn Sie eine Remotedesktopverbindung mit den Controllerinstanzen herstellen müssen, ändern Sie mit PowerShell die Sicherheitsregeln innerhalb der Netzwerksicherheitsgruppen.  Im Folgenden werden Beispiele zum *Zulassen* und anschließenden Wiederherstellen der Konfiguration mit *Verweigern* vorgestellt:  

    - *Zulassen*:

      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      ##This doesn’t work. Need to set properties again even in case of edit

      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Verweigern*:

        ```powershell

        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        ##This doesn’t work. Need to set properties again even in case of edit

        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
        ```


### <a name="sql-and-mysql"></a>SQL und MySQL

- <!-- TBD - IS --> Auf Servern, die SQL oder MySQL hosten, kann nur der Ressourcenanbieter Elemente erstellen. Die auf einem Hostserver erstellten Elemente, die nicht vom Ressourcenanbieter erstellt wurden, könnten zu einem Zustand ohne Entsprechung führen.  

- <!-- IS, ASDK --> Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** oder **Ebene** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen.


> [!NOTE]  
> <!-- TBD - IS --> Nachdem Sie ein Update auf diese Version von Azure Stack ausgeführt haben, können Sie die zuvor bereitgestellten Ressourcenanbieter SQL und MySQL weiterhin verwenden.  Es wird empfohlen, SQL und MySQL zu aktualisieren, sobald ein neues Release verfügbar ist. Wenden Sie Updates wie bei Azure Stack sequenziell auf die Ressourcenanbieter SQL und MySQL an. Wenn Sie z.B. Version 1804 verwenden, wenden Sie zuerst Version 1805 an, und aktualisieren Sie sie dann auf 1807.      
>   
> Die Installation dieses Updates wirkt sich nicht auf die aktuelle Verwendung der von Ihren Benutzern verwendeten Ressourcenanbieter SQL oder MySQL aus.
> Unabhängig von der Version der Ressourcenanbieter, die Sie verwenden, bleiben Ihre Benutzerdaten in deren Datenbanken davon unberührt und stehen weiterhin für den Zugriff zur Verfügung.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

- <!-- 2489178 - IS ASDK --> Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.

- <!-- TBD - IS ASDK --> App Service kann derzeit nur im *Abonnement für Standardanbieter* bereitgestellt werden. In einem zukünftigen Update wird App Service in das neue *Messungsabonnement* integriert, das in Azure Stack 1804 eingeführt wurde. Wenn Messungen für die Verwendung unterstützt werden, migrieren alle vorhandenen Bereitstellungen zu diesem neuen Abonnementtyp.


### <a name="usage"></a>Verwendung  
- <!-- TBD - IS ASDK --> Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Herunterladen des Updates
Sie können das Paket für das Azure Stack-Update 1807 [hier](https://aka.ms/azurestackupdatedownload) herunterladen.


## <a name="see-also"></a>Weitere Informationen
- Informationen zum Überwachen und Fortsetzen von Updates mithilfe des privilegierten Endpunkts (PEP) finden Sie unter [Überwachen von Änderungen in Azure Stack mithilfe des privilegierten Endpunkts](azure-stack-monitor-update.md).
- Eine Übersicht über die Updateverwaltung in Azure Stack finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md).
- Informationen zur Anwendung von Updates mit Azure Stack finden Sie unter [Anwenden von Updates in Azure Stack](azure-stack-apply-updates.md).
