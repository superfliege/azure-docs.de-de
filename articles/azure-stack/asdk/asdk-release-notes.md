---
title: Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation
description: Verbesserungen, Fehlerbehebungen und bekannte Probleme in Azure Stack Development Kit
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2018
ms.author: sethm
ms.reviewer: misainat
ms.openlocfilehash: d1ff154c42709f0c672b30f7ec51a436fb44ce13
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724741"
---
# <a name="asdk-release-notes"></a>Versionshinweise zum ASDK 
 
Dieser Artikel bietet Informationen zu Verbesserungen, Fehlerbehebungen und bekannten Problemen im Azure Stack Development Kit (ASDK). Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](../azure-stack-updates.md#determine-the-current-version).

> Abonnieren Sie den [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)-[Feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#), um in Bezug auf Neuerungen im ASDK auf dem Laufenden zu bleiben.

## <a name="build-118110101"></a>Build 1.1811.0.101

### <a name="new-features"></a>Neue Funktionen

Dieser Build enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack:  

- Es gibt einen Satz von neuen Mindestanforderungen und Empfehlungen für Hardware und Software für das ASDK. Diese neuen empfohlenen Spezifikationen sind in [Überlegungen zur Planung der Azure Stack-Bereitstellung](asdk-deploy-considerations.md) dokumentiert. Da sich die Azure Stack-Plattform weiterentwickelt hat, sind jetzt mehr Dienste verfügbar, wodurch möglicherweise auch mehr Ressourcen erforderlich werden. Die gestiegenen Spezifikationen spiegeln diese überarbeiteten Empfehlungen wider.

### <a name="fixed-issues"></a>Behobene Probleme

<!-- TBD - IS ASDK --> 
- Ein Problem wurde behoben, bei dem die Ansicht für die Nutzungsdaten öffentlicher IP-Adressen denselben **EventDateTime**-Wert für jeden Datensatz anzeigte, statt den **TimeDate**-Stempel, der anzeigt, wann der Datensatz jeweils erstellt wurde. Sie können jetzt diese Daten nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

<!-- 3099544 – IS, ASDK --> 
- Ein Problem wurde behoben, das auftrat, wenn ein neuer virtueller Computer (VM) mithilfe des Azure Stack-Portals erstellt wurde. Durch die Auswahl der VM-Größe wurde in der Spalte „USD/Monat“ die Meldung **Nicht verfügbar** angezeigt. Diese Spalte wird nicht mehr angezeigt. Die Anzeige der VM-Preisspalte wird in Azure Stack nicht unterstützt.

<!-- 2930718 - IS ASDK --> 
- Ein Problem wurde behoben, bei dem im Administratorportal beim Zugriff auf die Details eines Benutzerabonnements nach dem Schließen des Blatts und dem Klicken auf **Aktuell** der Name des Benutzerabonnements nicht angezeigt wurde. Der Name des Benutzerabonnements wird jetzt angezeigt.

<!-- 3060156 - IS ASDK --> 
- Ein Problem wurde behoben, bei dem sowohl im Administrator- als auch im Benutzerportal das Klicken auf die Portaleinstellungen und das Auswählen von **Alle Einstellungen und private Dashboards löschen** nicht wie erwartet funktionierte. Es wurde auch keine Fehlerbenachrichtigung angezeigt. 

<!-- 2930799 - IS ASDK --> 
- Ein Problem wurde behoben, bei dem in den Administrator- und Benutzerportalen unter **Alle Dienste** die Ressource **DDoS Protection-Pläne** fehlerhaft aufgeführt wurde, obwohl sie in Azure Stack nicht verfügbar war.
 
<!--2760466 – IS  ASDK --> 
- Ein Problem wurde behoben, bei dem bei der Installation einer neuen Azure Stack-Umgebung, die Warnung, dass eine **Aktivierung erforderlich** ist, nicht angezeigt wurde. Sie wird nun korrekt angezeigt.

<!--1236441 – IS  ASDK --> 
- Ein Problem wurde behoben, das die Anwendung von RBAC-Richtlinien auf eine Benutzergruppe verhinderte, wenn ADFS verwendet wurde.

<!--3463840 - IS, ASDK --> 
- Ein Problem wurde behoben, bei dem Infrastruktursicherungen fehlschlugen, weil vom öffentlichen VIP-Netzwerk nicht auf den Dateiserver zugegriffen werden konnte. Diese Korrektur verschiebt den Dienst für die Infrastruktursicherung zurück in das öffentliche Infrastrukturnetzwerk. Wenn Sie den neueste [Azure Stack-Hotfix für 1809](#azure-stack-hotfixes) angewendet haben, der dieses Problem behebt, nimmt das 1811-Update keine weiteren Änderungen vor. 

<!-- 2967387 – IS, ASDK --> 
- Ein Problem wurde behoben, bei dem das Konto, mit dem Sie sich beim Administrator- oder Benutzerportal von Azure Stack angemeldet hatten, als **Unbekannter Benutzer** angezeigt wurde. Diese Meldung wurde angezeigt, wenn für das Konto kein *Vorname* oder *Nachname* angegeben war.   

<!--  2873083 - IS ASDK --> 
- Ein Problem wurde behoben, bei dem durch die Verwendung des Portals zum Erstellen einer VM-Skalierungsgruppe das Dropdownmenü *Instanzgröße* bei Verwendung von Internet Explorer nicht ordnungsgemäß geladen wurde. Dieser Browser funktioniert jetzt ordnungsgemäß.  

<!-- 3190553 - IS ASDK -->
- Ein Problem wurde behoben, bei dem falsch positive Warnungen generiert wurden, die anzeigten, dass eine Infrastrukturrolleninstanz nicht verfügbar ist oder dass der Knoten „Skalierungseinheit“ offline ist.

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="portal"></a>Portal  

<!-- 2930820 - IS ASDK --> 
- Wenn Sie im Administrator- oder im Benutzerportal nach „Docker“ suchen, wird das Element nicht ordnungsgemäß zurückgegeben. Sie ist in Azure Stack nicht verfügbar. Wenn Sie versuchen, sie zu erstellen, wird ein Blatt mit einem Fehlerhinweis angezeigt. 

<!-- 2931230 – IS  ASDK --> 
- Pläne, die einem Benutzerabonnement als Add-On-Plan hinzugefügt wurden, können nicht gelöscht werden, auch wenn Sie den Plan aus dem Benutzerabonnement entfernen. Der Plan ist so lange vorhanden, bis die Abonnements gelöscht werden, die auf den Add-On-Plan verweisen. 

<!-- TBD - IS ASDK --> 
- Die zwei administrativen Abonnementtypen, die in Version 1804 eingeführt wurden, sollten nicht verwendet werden. Die Abonnementtypen sind **Messungsabonnement** und **Verbrauchsabonnement**. Diese Abonnementtypen sind in neuen Azure Stack-Umgebungen ab Version 1804 sichtbar, aber noch nicht zur Verwendung bereit. Sie sollten den Abonnementtyp **Standardanbieter** weiterhin verwenden.

<!-- TBD - IS ASDK --> 
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

<!-- TBD - IS ASDK --> 
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.

#### <a name="compute"></a>Compute 

<!-- 3235634 – IS, ASDK -->
- Um VMs mit Größen bereitzustellen, die das Suffix **v2** enthalten – z.B. **Standard_A2_v2** –, geben Sie das Suffix mit klein geschriebenem „v“ an: **Standard_A2_v2**. Verwenden Sie nicht **Standard_A2_V2** mit groß geschriebenem „V“. Dies funktioniert in der globalen Azure-Umgebung und ist eine Inkonsistenz in Azure Stack.

<!-- 2869209 – IS, ASDK --> 
- Bei Verwendung des [**Add-AzsPlatformImage**-Cmdlets](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0) müssen Sie den **-OsUri**-Parameter als Speicherkonto-URI beim Hochladen des Datenträgers verwenden. Wenn Sie den lokalen Pfad des Datenträgers verwenden, schlägt das Cmdlet mit der folgenden Fehlermeldung fehl: *Fehler beim Vorgang mit langer Ausführungszeit mit dem Status ‚Fehler‘*. 

<!--  2795678 – IS, ASDK --> 
- Wenn Sie das Portal zum Erstellen virtueller Computer in einer Premium-VM-Größe (DS, Ds_v2, FS, FSv2) verwenden, wird der virtuelle Computer in einem Standardspeicherkonto erstellt. Die Erstellung in einem Standardspeicherkonto wirkt sich nicht auf die Funktionalität, IOPs oder die Abrechnung aus. Sie können die folgende Warnung problemlos ignorieren: **Sie haben sich dafür entschieden, einen Standarddatenträger mit einer Größe zu verwenden, die Premium-Datenträger unterstützt. Dies kann sich auf die Leistung des Betriebssystems auswirken und wird nicht empfohlen. Erwägen Sie stattdessen die Verwendung von Storage Premium (SSD).**

<!-- 2967447 - IS, ASDK --> 
- Die Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen bietet „CentOS 7.2-basiert“ als Option für die Bereitstellung an. Da dieses Image in Azure Stack nicht verfügbar ist, wählen Sie entweder ein anderes Betriebssystem für Ihre Bereitstellung aus, oder verwenden Sie eine Azure Resource Manager-Vorlage mit einem anderen CentOS-Image, das vor der Bereitstellung vom Operator aus dem Marketplace heruntergeladen wurde.  

<!-- TBD - IS ASDK --> 
- Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

<!-- 1662991 IS ASDK --> 
- Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren.  

<!-- 2724961- IS ASDK --> 
- Wenn Sie den **Microsoft.Insight**-Ressourcenanbieter in den Abonnementeinstellungen registrieren und einen virtuellen Windows-Computer mit aktivierter Gastbetriebssystemdiagnose erstellen, werden im Diagramm mit der CPU-Nutzung in Prozent auf der Übersichtsseite des virtuellen Computers keine Metrikdaten angezeigt.

   Navigieren Sie zum Anzeigen von Metrikdaten (etwa das Diagramm „CPU-Prozentsatz“ für den virtuellen Computer) zum Fenster „Metriken“, und zeigen Sie alle unterstützten Metriken für Windows-VM-Gäste an.

<!-- 3507629 - IS, ASDK --> 
- Managed Disks erstellt zwei neue [Computekontingenttypen](../azure-stack-quota-types.md#compute-quota-types) zur Begrenzung der maximalen Kapazität von verwalteten Datenträgern, die bereitgestellt werden können. Standardmäßig werden für jeden Kontingenttyp von verwalteten Datenträgern 2.048 GiB zugeordnet. Allerdings können die folgenden Probleme auftreten:

   - Bei Kontingenten, die vor dem Update 1808 erstellt wurden, werden im Administratorportal für das "Managed Disks"-Kontingent 0 Werte angezeigt, obwohl 2.048 GiB zugeordnet wurden. Sie können den Wert auf der Grundlage Ihrer tatsächlichen Anforderungen erhöhen oder verringern. Dann setzt der neu festgelegte Kontingentwert den Standardwert 2.048 GiB außer Kraft.
   - Wenn Sie den Kontingentwert auf „0“ aktualisieren, entspricht dies dem Standardwert 2.048 GiB. Als Problemumgehung können Sie den Kontingentwert auf „1“ festlegen.

<!-- TBD - IS ASDK --> 
- Nach dem Anwenden des Updates 1811 können beim Bereitstellen von virtuellen Computern mit Managed Disks die folgenden Probleme auftreten:

   1. Wenn das Abonnement vor dem Update 1808 erstellt wurde, schlägt die Bereitstellung eines virtuellen Computers mit Managed Disks möglicherweise mit einer internen Fehlermeldung fehl. Um den Fehler zu beheben, führen Sie die folgenden Schritte für jedes Abonnement aus:
      1. Navigieren Sie im Mandantenportal zu **Abonnements**, und suchen Sie nach dem Abonnement. Klicken Sie auf **Ressourcenanbieter**, klicken Sie dann auf **Microsoft.Compute**, und klicken Sie anschließend auf **Erneut registrieren**.
      2. Navigieren Sie unter dem gleichen Abonnement zu **Zugriffssteuerung (IAM)**, und überprüfen Sie, ob **Azure Stack – Verwalteter Datenträger** aufgeführt wird.
   2. Wenn Sie eine Umgebung mit mehreren Mandanten konfiguriert haben, schlägt die Bereitstellung von virtuellen Computern in einem Abonnement, dem ein Gastverzeichnis zugeordnet ist, möglicherweise mit einer internen Fehlermeldung fehl. Zum Beheben des Fehlers führen Sie die in [diesem Artikel](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) beschriebenen Schritte aus, um alle Gastverzeichnisse neu zu konfigurieren.

#### <a name="networking"></a>Netzwerk

<!-- 1766332 - IS ASDK --> 
- Wenn Sie unter **Netzwerk** auf **VPN Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

<!-- 1902460 - IS ASDK --> 
- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

<!-- 16309153 - IS ASDK --> 
- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.

<!-- 2529607 - IS ASDK --> 
- Während der *Geheimnisrotation* von Azure Stack sind öffentliche IP-Adressen für einen Zeitraum von zwei bis fünf Minuten nicht erreichbar.

<!-- 2664148 - IS ASDK --> 
-   In Szenarien, in denen der Mandant über einen S2S-VPN-Tunnel auf seine VMs zugreift, kann es passieren, dass bei Verbindungsversuchen Fehler auftreten, wenn das lokale Subnetz dem lokalen Netzwerkgateway erst nach der Erstellung des Gateways hinzugefügt wurde. 

#### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

<!-- #### Usage -->  

<!-- #### Identity -->

## <a name="build-11809090"></a>Build 1.1809.0.90

### <a name="new-features"></a>Neue Funktionen
Dieser Build enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack.  

<!--  2712869   | IS  ASDK -->  
- **Azure Stack-Syslog-Client (Allgemeine Verfügbarkeit)**: Dieser Client ermöglicht die Weiterleitung von Überwachungen, Warnungen und Sicherheitsprotokollen der Azure Stack-Infrastruktur an einen Syslog-Server oder an SIEM-Software (Security Information and Event Management) außerhalb von Azure Stack. Der Syslog-Client unterstützt jetzt die Angabe des Ports, an dem der Syslog-Server lauscht.

Mit diesem Release ist der Syslog-Client allgemein verfügbar und kann in Produktionsumgebungen verwendet werden.

Weitere Informationen finden Sie unter [Azure Stack-Syslog-Weiterleitung](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Behobene Probleme

<!-- TBD - IS ASDK -->
- Im Portal sind die Angaben zur freien/verwendeten Kapazität im Speicherdiagramm jetzt exakt. Sie können jetzt zuverlässiger prognostizieren, wie viele VMs Sie erstellen können.

<!-- TBD - IS ASDK --> 
- Folgendes Problem wurde behoben: Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigte das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

- <!-- 2702741 -  IS, ASDK --> Ein Problem mit öffentlichen IP-Adressen, die mithilfe der dynamischen Zuordnungsmethode bereitgestellt wurden und nach der Ausgabe eines Befehls zum Beenden und Aufheben der Zuordnung nicht unbedingt beibehalten werden, wurde behoben. Sie werden jetzt beibehalten.

- <!-- 3078022 - IS, ASDK --> Wenn eine VM vor Version 1808 beendet und ihre Zuordnung aufgehoben wurde, konnte sie nach dem Update auf 1808 nicht neu zugeordnet werden.  Dieses Problem wurde in Version 1809 behoben. Instanzen, die sich in diesem Zustand befanden und nicht gestartet werden konnten, können in Version 1809 mit dieser Lösung gestartet werden. Die Lösung verhindert außerdem, dass dieses Problem erneut auftritt.

<!-- TBD -  IS ASDK --> 
- Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie erneut versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, herunterzuladen.

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.

### <a name="changes"></a>Änderungen

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="portal"></a>Portal  

<!-- 2515955   | IS ,ASDK--> 
- *Alle Dienste* ersetzt *Weitere Dienste* in den Azure Stack-Administrator- und -Benutzerportalen. Sie können nun *Alle Dienste* als Alternative zum Navigieren in den Azure Stack-Portalen wie in den Azure-Portalen verwenden.

<!--  TBD – IS, ASDK --> 
- Die VM-Größen *Basic A* stehen für das [Erstellen von VM-Skalierungsgruppen](../azure-stack-compute-add-scalesets.md) (VMSS) über das Portal nicht mehr zur Verfügung. Verwenden Sie PowerShell oder eine Vorlage, um eine VMSS mit dieser Größe zu erstellen.

#### <a name="health-and-monitoring"></a>Integrität und Überwachung

<!-- 1264761 - IS ASDK -->  
- Möglicherweise werden Warnungen für die *Health Controller*-Komponente mit folgenden Details angezeigt:  

   Warnung 1:
   - NAME:  Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Heartbeat Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.  

  Warnung 2:
   - NAME:  Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Fault Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.

  Beide Alarme können ignoriert werden und werden nach einer gewissen Zeit automatisch geschlossen.  

<!-- 2368581 - IS. ASDK --> 
- Wenn Sie als Azure Stack-Operator eine Fehlermeldung über unzureichenden Speicher erhalten und virtuelle Computer der Mandanten nicht mit einem *Fabric-VM-Erstellungsfehler* bereitgestellt werden können, verfügt der Azure Stack-Stempel möglicherweise nicht über genügend Arbeitsspeicher. Mit dem [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) können Sie die verfügbare Kapazität für Ihre Workloads besser bestimmen.


#### <a name="compute"></a>Compute 

<!-- 3164607 – IS, ASDK -->
- Das erneute Anfügen eines abgetrennten Datenträgers an denselben virtuellen Computer mit dem gleichen Namen und der gleichen LUN führt zu einem Fehler wie **Datenträger „Datenträger“ kann nicht an VM „vm1“ angefügt werden**. Der Fehler tritt auf, da der Datenträger derzeit getrennt wird oder beim letzten Trennvorgang ein Fehler aufgetreten ist. Warten Sie, bis der Datenträger vollständig getrennt wurde, und versuchen Sie es dann erneut, oder löschen/trennen Sie den Datenträger explizit erneut. Die Problemumgehung besteht darin, ihn unter einem anderen Namen oder mit einer anderen LUN erneut anzufügen. 

<!-- 3235634 – IS, ASDK -->
- Um VMs mit Größen bereitzustellen, die das Suffix **v2** enthalten – z.B. **Standard_A2_v2** –, geben Sie das Suffix mit klein geschriebenem „v“ an: **Standard_A2_v2**. Verwenden Sie nicht **Standard_A2_V2** mit groß geschriebenem „V“. Dies funktioniert in der globalen Azure-Umgebung und ist eine Inkonsistenz in Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Wenn Sie einen neuen virtuellen Computer (VM) über das Azure Stack-Portal erstellen und die VM-Größe auswählen, wird die Spalte „USD/Monat“ mit der Meldung **Nicht verfügbar** angezeigt. Diese Spalte sollte nicht angezeigt werden. Die Anzeige der VM-Preisspalte wird in Azure Stack nicht unterstützt.

<!-- 2869209 – IS, ASDK --> 
- Bei Verwendung des [**Add-AzsPlatformImage**-Cmdlets](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0) müssen Sie den **-OsUri**-Parameter als Speicherkonto-URI beim Hochladen des Datenträgers verwenden. Wenn Sie den lokalen Pfad des Datenträgers verwenden, schlägt das Cmdlet mit der folgenden Fehlermeldung fehl: *Fehler beim Vorgang mit langer Ausführungszeit mit dem Status ‚Fehler‘*. 

<!--  2966665 – IS, ASDK --> 
- Das Anfügen von SSD-Datenträgern an virtuelle Computer mit verwalteten Datenträgern von Premiumgröße (DS, DSv2, Fs, Fs_V2) schlägt mit der folgenden Fehlermeldung fehl:  *Fehler beim Aktualisieren von Datenträgern für den virtuellen Computer ‚vmName‘: Der angeforderte Vorgang kann nicht ausgeführt werden, weil der Speicherkontotyp ‚Premium_LRS‘ für die VM-Größe (‘Standard_DS/Ds_V2/FS/Fs_v2)‘ nicht unterstützt wird*

   Um dieses Problem zu umgehen, verwenden Sie Datenträger vom Typ *Standard_LRS* anstelle von *Premium_LRS*. Das Verwenden von Datenträgern des Typs *Standard_LRS* ändert IOPs oder die Abrechnungskosten nicht.  

<!--  2795678 – IS, ASDK --> 
- Wenn Sie das Portal zum Erstellen virtueller Computer in einer Premium-VM-Größe (DS, Ds_v2, FS, FSv2) verwenden, wird der virtuelle Computer in einem Standardspeicherkonto erstellt. Die Erstellung in einem Standardspeicherkonto wirkt sich nicht auf die Funktionalität, IOPs oder die Abrechnung aus. 

   Sie können die folgende Warnung problemlos ignorieren: *Sie haben sich dafür entschieden, einen Standarddatenträger mit einer Größe zu verwenden, die Premium-Datenträger unterstützt. Dies kann sich auf die Leistung des Betriebssystems auswirken und wird nicht empfohlen. Erwägen Sie stattdessen die Verwendung von Storage Premium (SSD).*

<!-- 2967447 - IS, ASDK --> 
- Die Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen (VMSS) bietet als Option für die Bereitstellung eine CentOS 7.2-basierte Bereitstellung an. Da dieses Image in Azure Stack nicht verfügbar ist, wählen Sie entweder ein anderes Betriebssystem für Ihre Bereitstellung aus, oder verwenden Sie eine Azure Resource Manager-Vorlage mit einem anderen CentOS-Image, das vor der Bereitstellung vom Operator aus dem Marketplace heruntergeladen wurde.

<!-- TBD -  IS ASDK --> 
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.



<!-- TBD -  IS ASDK --> 
- Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

<!-- 1662991 - IS ASDK --> 
- Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren.

<!-- 2724961- IS ASDK --> 
- Wenn Sie den Ressourcenanbieter **Microsoft.Insight** in den Abonnementeinstellungen registrieren und eine Windows-VM mit aktivierter Gastbetriebssystemdiagnose erstellen, können im Diagramm „CPU-Prozentsatz“ auf der Übersichtsseite der VM keine Metrikdaten angezeigt werden.
 
  Navigieren Sie zum Anzeigen des Diagramms „CPU-Prozentsatz“ für die VM zum Blatt **Metriken**, und zeigen Sie alle unterstützten Gastmetriken für Windows-VMs an.

 

#### <a name="networking"></a>Netzwerk
<!-- 1766332 - IS, ASDK --> 
- Wenn Sie unter **Netzwerk** auf **VPN Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

<!-- 16309153 -  IS ASDK --> 
- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.

<!-- 2702741 -  IS ASDK --> 
- Öffentliche IP-Adressen, die mithilfe der dynamischen Zuordnungsmethode bereitgestellt wurden, werden nach der Ausgabe eines Befehls zum Beenden und Aufheben der Zuordnung nicht unbedingt beibehalten.

<!-- 2529607 - IS ASDK --> 
- Während der *Geheimnisrotation* von Azure Stack sind öffentliche IP-Adressen für einen Zeitraum von zwei bis fünf Minuten nicht erreichbar.

<!-- 2664148 - IS ASDK --> 
- In Szenarien, in denen der Mandant über einen S2S-VPN-Tunnel auf seine VMs zugreift, kann es passieren, dass bei Verbindungsversuchen Fehler auftreten, wenn das lokale Subnetz dem lokalen Netzwerkgateway erst nach der Erstellung des Gateways hinzugefügt wurde. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

<!-- TBD - IS ASDK --> 
- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.  


#### <a name="usage"></a>Verwendung  
<!-- TBD -  IS ASDK --> 
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

<!-- #### Identity -->

## <a name="build-11808097"></a>Build 1.1808.0.97

### <a name="new-features"></a>Neue Funktionen
Dieser Build enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack.  

<!-- 1658937 | ASDK, IS --> 
- **Starten von Sicherungen anhand eines vordefinierten Zeitplans**: Als Appliance kann Azure Stack nun automatisch Infrastruktursicherungen regelmäßig auslösen, um Benutzereingriffe zu eliminieren. Zudem bereinigt Azure Stack auch automatisch die externe Freigabe für Sicherungen, die älter als die definierte Aufbewahrungsdauer sind. Weitere Informationen finden Sie unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Zur Gesamtsicherungszeit wurde die Datenübertragungszeit addiert.** Weitere Informationen finden Sie unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS --> 
- **Die externe Kapazität für Sicherungen zeigt jetzt die richtige Kapazität der externen Freigabe an.** (Zuvor war diese hardcodiert und zeigte 10 GB an.) Weitere Informationen finden Sie unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](../azure-stack-backup-enable-backup-powershell.md).
 
<!-- 2753130 |  IS, ASDK   -->  
- **Azure Resource Manager-Vorlagen unterstützen jetzt das condition-Element**: Sie können jetzt eine Ressource in einer Azure Resource Manager-Vorlage mit einer Bedingung bereitstellen. Sie können Ihre Vorlage für die Bereitstellung einer Ressource auf Grundlage einer Bedingung (beispielsweise das Vorhandensein eines Parameterwerts) entwerfen. Informationen zur Verwendung einer Vorlage als Bedingung finden Sie unter [Bedingtes Bereitstellen einer Ressource](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) und [Abschnitt „Variablen“ von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) in der Azure-Dokumentation. 

   Sie können Vorlagen auch zum [Bereitstellen von Azure-Ressourcen für mehrere Abonnements oder Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment) verwenden.  

<!--2753073 | IS, ASDK -->  
- **Die Unterstützung der Microsoft.Network-API-Ressourcenversion wurde aktualisiert** und umfasst nun Unterstützung für die API-Version 2017-10-01 von 2015-06-15 für Azure Stack-Netzwerkressourcen. Die Unterstützung für Ressourcenversionen zwischen 2017-10-01 und 2015-06-15 ist nicht in diesem Release enthalten. Weitere Informationen zu den Funktionsunterschieden finden Sie unter [Überlegungen zu Azure Stack-Netzwerken](../user/azure-stack-network-differences.md).

<!-- 2272116 | IS, ASDK   -->  
- **Azure Stack bietet jetzt Unterstützung für Reverse-DNS-Lookups für extern zugängliche Azure Stack-Infrastrukturendpunkte** (d.h. für Portal, Verwaltungsportal, Verwaltung und Administratorverwaltung). Dies ermöglicht die Auflösung der Namen von externen Azure Stack-Endpunkten über eine IP-Adresse.

<!-- 2780899 |  IS, ASDK   --> 
- **Azure Stack unterstützt jetzt das Hinzufügen zusätzlicher Netzwerkschnittstellen zu einer vorhandenen VM.**  Diese Funktion ist über das Portal, PowerShell und die Befehlszeilenschnittstelle verfügbar. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Netzwerkschnittstellen](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) in der Azure-Dokumentation. 

<!-- 2222444 | IS, ASDK   -->  
- **An Netzwerkverbrauchseinheiten wurden Verbesserungen bezüglich der Genauigkeit und Resilienz vorgenommen**. Netzwerkverbrauchseinheiten sind jetzt genauer und berücksichtigen angehaltene Abonnements, Ausfallzeiten und Racebedingungen.

<!-- 2297790 | IS, ASDK -->  
- **Verbesserungen am Azure Stack-Syslog-Client (Vorschaufeature)**. Dieser Client ermöglicht die Weiterleitung von Überwachungen und Protokollen der Azure Stack-Infrastruktur an einen Syslog-Server oder eine SIEM-Software (Security Information & Event Management) außerhalb von Azure Stack. Der Syslog-Client unterstützt jetzt das TCP-Protokoll mit Nur-Text- oder die TLS 1.2-Verschlüsselung (Letzteres ist die Standardkonfiguration). Sie können die TLS-Verbindung nur mit Serverauthentifizierung oder mit gegenseitiger Authentifizierung konfigurieren.

  Verwenden Sie zum Konfigurieren der Kommunikation des Syslog-Clients mit dem Syslog-Server (beispielsweise Protokoll, Verschlüsselung und Authentifizierung) das Cmdlet „Set-SyslogServer“. Dieses Cmdlet ist über den privilegierten Endpunkt (PEP) verfügbar. 

- <!-- ASDK --> **Katalogelemente für Virtual Machine Scale Sets sind jetzt integriert**.  Katalogelemente für VM-Skalierungsgruppen stehen jetzt in den Benutzer- und Administratorportalen zur Verfügung, ohne dass sie heruntergeladen werden müssen. 

- <!-- IS, ASDK --> **Virtual Machine Scale Set-Skalierung**.  Sie können das Portal verwenden, um eine [VMSS](../azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (Virtual Machine Scale Set) zu skalieren.   

- <!-- 2489570 | IS ASDK--> **Unterstützung für benutzerdefinierte IPSec-/IKE-Richtlinienkonfigurationen** für [VPN-Gateways in Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Kubernetes-Marketplace-Element**. Sie können nun Kubernetes-Cluster mithilfe des [Kubernetes-Marketplace-Elements](/azure/azure-stack/azure-stack-solution-template-kubernetes-cluster-add) bereitstellen. Benutzer können das Kubernetes-Element auswählen und einige Parameter eingeben, um einen Kubernetes-Cluster in Azure Stack bereitzustellen. Der Zweck der Vorlagen ist es, es den Benutzern einfach zu machen, in wenigen Schritten dev/test-Kubernetes-Bereitstellungen einzurichten.

<!-- ####### | IS, ASDK -->  
- **Azure Resource Manager enthält den Namen der Region.** In diesem Release enthalten aus der Azure Resource Manager-Vorlage abgerufene Objekte jetzt das Attribut „Regionsname“. Wenn ein vorhandenes PowerShell-Skript das Objekt direkt an ein anderes Cmdlet übergibt, kann bei der Ausführung des Skripts ein Fehler auftreten. Dies ist das mit Azure Resource Manager konforme Verhalten und erfordert, dass der aufrufende Client das Attribut „Region“ subtrahiert. Weitere Informationen zum Azure Resource Manager finden Sie in der [Dokumentation zum Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/).

<!-- TBD | IS, ASDK -->  
- **Verschieben von Abonnements zwischen delegierten Anbietern.** Sie können jetzt Abonnements zwischen neuen oder vorhandenen delegierten Anbieterabonnements verschieben, die zu demselben Verzeichnismandanten gehören. Abonnements, die zum Standardabonnement des Anbieters gehören, können auch in die delegierten Anbieterabonnements im gleichen Verzeichnismandanten verschoben werden. Weitere Informationen finden Sie unter [Delegieren von Angeboten in Azure Stack](../azure-stack-delegated-provider.md).
 
<!-- 2536808 IS ASDK --> 
- **Kürzere VM-Erstellungszeit** für VMs, die mit aus dem Azure Marketplace heruntergeladenen Images erstellt werden.

### <a name="fixed-issues"></a>Behobene Probleme
- <!-- IS ASDK--> Wir haben das Problem bei der Erstellung einer Verfügbarkeitsgruppe im Portal behoben, das dazu führte, dass die Gruppe eine Fehlerdomäne und eine Updatedomäne von 1 hatte.

<!-- TBD | ASDK, IS --> 
- Die Zuverlässigkeit des Updateprozesses wurde durch verschiedene Verbesserungen erhöht. Zudem wurde durch die Behebung von Fehlern in der zugrunde liegenden Infrastruktur der Knotenausgleich verbessert, wodurch die potenzielle Downtime für Workloads während der Aktualisierung minimiert wird.

<!--2292271 | ASDK, IS --> 
- Es wurde ein Problem behoben, bei dem ein geänderter Kontingentgrenzwert nicht für bestehende Abonnements galt.  Wenn Sie nun einen Kontingentgrenzwert für eine Netzwerkressource erhöhen, die Teil eines mit einem Benutzerabonnement verknüpften Angebots und Plans ist, gilt der neue Grenzwert sowohl für die bereits bestehenden als auch für neue Abonnements.

<!-- 2448955 | IS ASDK --> 
- Sie können jetzt erfolgreich Aktivitätsprotokolle für Systeme abfragen, die in einer UTC+N-Zeitzone bereitgestellt sind.    

<!-- 2319627 |  ASDK, IS --> 
- Die Vorprüfung der Sicherungskonfigurationsparameter (Pfad/Benutzername/Kennwort/Verschlüsselungsschlüssel) legt keine falschen Einstellungen mehr in der Sicherungskonfiguration fest. (Früher wurden für die Sicherung falsche Einstellungen festgelegt, wodurch beim Auslösen der Sicherung ein Fehler aufgetreten ist.)

<!-- 2215948 |  ASDK, IS --> 
- Die Sicherungsliste wird nun aktualisiert, wenn Sie die Sicherung manuell von der externen Freigabe löschen.

<!-- 2360715 |  ASDK, IS -->  
- Wenn Sie die Rechenzentrumsintegration einrichten, greifen Sie nicht mehr von einer Freigabe auf die AD FS-Metadatendatei zu. Weitere Informationen finden Sie unter [Einrichten der AD FS-Integration durch Bereitstellen einer Verbundmetadatendatei](../azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

<!-- 2388980 | ASDK, IS --> 
- Es wurde ein Problem behoben, das verhinderte, dass Benutzer eine vorhandene öffentliche IP-Adresse, die zuvor einer Netzwerkschnittstelle oder einem Load Balancer zugewiesen war, einer neuen Netzwerkschnittstelle oder einem neuen Load Balancer zuweisen.  

<!-- 2551834 - IS, ASDK --> 
- Wenn Sie im Verwaltungs- oder Benutzerportal für ein Speicherkonto die Option *Übersicht* auswählen, werden alle erwarteten Informationen richtig im Bereich *Zusammenfassung* angezeigt. 

<!-- 2551834 - IS, ASDK --> 
- Wenn Sie im Verwaltungs- oder Benutzerportal für ein Speicherkonto die Option *Tags* auswählen, werden die Informationen jetzt richtig angezeigt.

<!-- TBD - IS ASDK --> 
- In dieser Version von Azure Stack wurde das Problem behoben, durch das die Anwendung von Treiberupdates aus OEM-Erweiterungspaketen verhindert wurde.

<!-- 2055809- IS ASDK --> 
- Wir haben ein Problem behoben, aufgrund dessen Sie keine VMs auf dem Blatt „Compute“ löschen konnten, wenn beim Erstellen der VM ein Fehler auftrat.  

<!--  2643962 IS ASDK -->  
- Die Warnung für *niedrige Arbeitsspeicherkapazität* wird nicht mehr fälschlicherweise angezeigt.

<!--  TBD ASDK --> 
- Die VM, die den privilegierten Endpunkt (PEP) hostet, wurde auf 4 GB vergrößert. In der ASDK heißt dieser virtueller Computer AzS-ERCS01.

- <!--  TBD – IS, ASDK --> Die VM-Größen *Basic A* stehen für das [Erstellen von VM-Skalierungsgruppen](../azure-stack-compute-add-scalesets.md) (VMSS) über das Portal nicht mehr zur Verfügung. Verwenden Sie PowerShell oder eine Vorlage, um eine VMSS mit dieser Größe zu erstellen. 

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="portal"></a>Portal  
- <!-- 2967387 – IS, ASDK --> Das Konto, mit dem Sie sich am Azure Stack-Administrator- oder -Benutzerportal anmelden, wird als **Unidentifizierter Benutzer** angezeigt. Dieses Verhalten tritt auf, wenn für das Konto kein *Vorname* oder *Nachname* angegeben wurde. Um dieses Problem zu umgehen, bearbeiten Sie das Benutzerkonto so, dass der Vor- oder Nachname angegeben wird. Sie müssen sich dann abmelden und erneut am Portal anmelden. 

-  <!--  2873083 - IS ASDK --> Wenn Sie das Portal verwenden, um eine VM-Skalierungsgruppe (VMSS) zu erstellen, wird das Dropdownmenü *Instanzgröße* nicht ordnungsgemäß geladen, wenn Sie Internet Explorer verwenden. Um dieses Problem zu umgehen, nutzen Sie einen anderen Browser beim Verwenden des Portals zum Erstellen einer VMSS.  

#### <a name="portal"></a>Portal  
<!-- 2931230 – IS  ASDK --> 
- Pläne, die einem Benutzerabonnement als Add-On-Plan hinzugefügt wurden, können nicht gelöscht werden, auch wenn Sie den Plan aus dem Benutzerabonnement entfernen. Der Plan ist so lange vorhanden, bis die Abonnements gelöscht werden, die auf den Add-On-Plan verweisen. 

<!--2760466 – IS  ASDK --> 
- Bei der Installation einer neuen Azure Stack-Umgebung, die diese Version ausführt, wird die Warnung, dass eine *Aktivierung erforderlich* ist, möglicherweise nicht angezeigt. Die [Aktivierung](../azure-stack-registration.md) ist erforderlich, damit Sie die Marketplace-Syndikation verwenden können. 

<!-- TBD - IS ASDK --> 
- Die zwei administrativen Abonnementtypen, die in Version 1804 eingeführt wurden, sollten nicht verwendet werden. Die Abonnementtypen sind **Messungsabonnement** und **Verbrauchsabonnement**. Diese Abonnementtypen sind **Messungsabonnement** und **Verbrauchsabonnement**. Diese Abonnementtypen sind in neuen Azure Stack-Umgebungen ab Version 1804 sichtbar, aber noch nicht zur Verwendung bereit. Sie sollten weiterhin den Typ **Standardabonnement des Anbieters** verwenden.

<!-- 2403291 - IS ASDK --> 
- Möglicherweise haben Sie die horizontale Scrollleiste am unteren Rand des Verwaltungs- und Benutzerportals nicht verwendet. Wenn Sie nicht auf die horizontale Bildlaufleiste zugreifen können, verwenden Sie die Breadcrumb-Leiste, um zu einem vorherigen Blatt im Portal zu navigieren. Wählen Sie hierzu den Namen des Blattes, das Sie anzeigen möchten, aus der Breadcrumb-Liste oben links im Portal aus.
  ![Breadcrumb](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

<!-- TBD -  IS ASDK --> 
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.

<!--  TBD | ASDK -->  
- Die Standardzeitzone für Ihre Azure Stack-Bereitstellung wird jetzt auf „UTC“ festgelegt. Sie können bei der Installation von Azure Stack eine Zeitzone auswählen, diese wird während der Installation jedoch automatisch auf die Standardeinstellung „UTC“ zurückgesetzt.

#### <a name="health-and-monitoring"></a>Integrität und Überwachung
<!-- 1264761 - IS ASDK -->  
- Möglicherweise werden Warnungen für die *Health Controller*-Komponente mit folgenden Details angezeigt:  

   Warnung 1:
   - NAME:  Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Heartbeat Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.  

  Warnung 2:
   - NAME:  Infrastrukturrolle fehlerhaft
   - SCHWEREGRAD: Warnung
   - KOMPONENTE: Health Controller
   - BESCHREIBUNG: Fault Scanner von Health Controller ist nicht verfügbar. Dies kann sich auf Integritätsberichte und Metriken auswirken.

  Beide Alarme können ignoriert werden und werden nach einer gewissen Zeit automatisch geschlossen.  

<!-- 2368581 - IS. ASDK --> 
- Wenn Sie als Azure Stack-Operator eine Fehlermeldung über unzureichenden Speicher erhalten und virtuelle Computer der Mandanten nicht mit einem *Fabric-VM-Erstellungsfehler* bereitgestellt werden können, verfügt der Azure Stack-Stempel möglicherweise nicht über genügend Arbeitsspeicher. Mit dem [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) können Sie die verfügbare Kapazität für Ihre Workloads besser bestimmen.

<!-- TBD - IS. ASDK --> 
- Beim Ausführen des Cmdlets **Test-AzureStack** für den bevorzugten Endpunkt (PEP) generiert der Test **Azure Stack-Infrastrukturrollen-Instanzleistung** eine WARN-Meldung für die ERCS-VM. Sie können diese WARN-Meldung problemlos ignorieren und das ASDK weiterhin verwenden.

#### <a name="compute"></a>Compute
<!-- 2494144 - IS, ASDK --> 
- Bei der Auswahl der Größe des virtuellen Computers für die Bereitstellung eines virtuellen Computers sind einige VM-Größen der Serie F beim Erstellen einer VM nicht als Teil der Größenauswahl sichtbar. Die folgenden VM-Größen werden in der Auswahl nicht angezeigt: *F8s_v2*, *F16s_v2*, *F32s_v2* und *F64s_v2*.  
  Um das Problem zu umgehen, verwenden Sie eine der folgenden Methoden zum Bereitstellen einer VM. Bei jeder Methode müssen Sie die gewünschte VM-Größe angeben.

- <!-- 3099544 – IS, ASDK --> Wenn Sie im Azure Stack-Portal eine neue VM erstellen und die VM-Größe auswählen, wird die Spalte „USD/Monat“ mit der Meldung **Nicht verfügbar** angezeigt. Diese Spalte sollte nicht angezeigt werden. Die Anzeige der VM-Preisspalte wird in Azure Stack nicht unterstützt.

- <!-- 2869209 – IS, ASDK --> Bei Verwendung des [**Add-AzsPlatformImage**-Cmdlets](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0) müssen Sie den **-OsUri**-Parameter als Speicherkonto-URI beim Hochladen des Datenträgers verwenden. Wenn Sie den lokalen Pfad des Datenträgers verwenden, schlägt das Cmdlet mit der folgenden Fehlermeldung fehl: *Fehler beim Vorgang mit langer Ausführungszeit mit dem Status ‚Fehler‘*. 

- <!--  2966665 – IS, ASDK --> Das Anfügen von SSD-Datenträgern an virtuelle Computer mit verwalteten Datenträgern von Premiumgröße (DS, DSv2, Fs, Fs_V2) schlägt mit der folgenden Fehlermeldung fehl:  *Fehler beim Aktualisieren von Datenträgern für den virtuellen Computer ‚vmName‘: Der angeforderte Vorgang kann nicht ausgeführt werden, weil der Speicherkontotyp ‚Premium_LRS‘ für die VM-Größe ‘Standard_DS/Ds_V2/FS/Fs_v2) nicht unterstützt wird.*

   Um dieses Problem zu umgehen, verwenden Sie Datenträger vom Typ *Standard_LRS* anstelle von *Premium_LRS*. Das Verwenden von Datenträgern des Typs *Standard_LRS* ändert IOPs oder die Abrechnungskosten nicht.  

- <!--  2795678 – IS, ASDK --> Wenn Sie das Portal zum Erstellen virtueller Computer in einer Premium-VM-Größe (DS, Ds_v2, FS, FSv2) verwenden, wird der virtuelle Computer in einem Standardspeicherkonto erstellt. Die Erstellung in einem Standardspeicherkonto wirkt sich nicht auf die Funktionalität, IOPs oder die Abrechnung aus. 

   Sie können die folgende Warnung problemlos ignorieren: *Sie haben sich dafür entschieden, einen Standarddatenträger mit einer Größe zu verwenden, die Premium-Datenträger unterstützt. Dies kann sich auf die Leistung des Betriebssystems auswirken und wird nicht empfohlen. Erwägen Sie stattdessen die Verwendung von Storage Premium (SSD).*

- <!-- 2967447 - IS, ASDK --> Die Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen (VMSS) bietet als Option für die Bereitstellung eine CentOS 7.2-basierte Bereitstellung an. Da dieses Image in Azure Stack nicht verfügbar ist, wählen Sie entweder ein anderes Betriebssystem für Ihre Bereitstellung aus, oder verwenden Sie eine ARM-Vorlage mit einem anderen CentOS-Image, das vor der Bereitstellung aus Marketplace durch den Operator heruntergeladen wurde.

<!-- TBD -  IS ASDK --> 
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.

<!-- TBD -  IS ASDK --> 
- Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie D angefügt werden können. Alle unterstützten virtuellen Computer der Serie D können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

<!-- TBD -  IS ASDK --> 
- Falls bei der Erstellung eines VM-Images ein Fehler auftritt, wird dem Blatt „Berechnung der VM-Images“ ggf. ein fehlerhaftes Element hinzugefügt, das nicht gelöscht werden kann.

  Erstellen Sie zur Problemumgehung ein neues VM-Image mit einer Dummy-VHD, die per Hyper-V erstellt werden kann (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Das Problem, mit dem das Löschen des fehlerhaften Elements verhindert wird, sollte durch diesen Vorgang behoben werden. Anschließend können Sie das Element 15 Minuten nach der Erstellung des Dummy-Images erfolgreich löschen.

  Danach können Sie erneut versuchen, das VM-Image, bei dem zuvor ein Fehler aufgetreten ist, herunterzuladen.

<!-- TBD -  IS ASDK --> 
- Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

<!-- 1662991 - IS ASDK --> 
- Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren.

<!-- 2724961- IS ASDK --> 
- Wenn Sie den **Microsoft.Insight**-Ressourcenanbieter in den Abonnementeinstellungen registrieren und einen virtuellen Windows-Computer mit aktivierter Gastbetriebssystemdiagnose erstellen, werden auf der Übersichtsseite des virtuellen Computers keine Metrikdaten angezeigt. 

 

#### <a name="networking"></a>Netzwerk
<!-- 1766332 - IS, ASDK --> 
- Wenn Sie unter **Netzwerk** auf **VPN Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

<!-- 16309153 -  IS ASDK --> 
- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.

<!-- 2702741 -  IS ASDK --> 
- Öffentliche IP-Adressen, die mithilfe der dynamischen Zuordnungsmethode bereitgestellt wurden, werden nach der Ausgabe eines Befehls zum Beenden und Aufheben der Zuordnung nicht unbedingt beibehalten.

<!-- 2529607 - IS ASDK --> 
- Während der *Geheimnisrotation* von Azure Stack sind öffentliche IP-Adressen für einen Zeitraum von zwei bis fünf Minuten nicht erreichbar.

<!-- 2664148 - IS ASDK --> 
- In Szenarien, in denen der Mandant über einen S2S-VPN-Tunnel auf seine VMs zugreift, kann es passieren, dass bei Verbindungsversuchen Fehler auftreten, wenn das lokale Subnetz dem lokalen Netzwerkgateway erst nach der Erstellung des Gateways hinzugefügt wurde. 


#### <a name="sql-and-mysql"></a>SQL und MySQL
<!-- TBD - ASDK --> 
- Die Server für das Hosten von Datenbanken müssen für die Verwendung durch die Ressourcenanbieter und Benutzerworkloads dediziert sein. Sie können keine Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.

<!-- IS, ASDK --> 
- Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden im Namen für die **Familie** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen.

#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

<!-- TBD - IS ASDK --> 
- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.  

<!-- TBD - IS ASDK --> 
- App Service kann zurzeit nur im *Abonnement für Standardanbieter* bereitgestellt werden.

#### <a name="usage"></a>Verwendung  
<!-- TBD -  IS ASDK --> 
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

<!-- #### Identity -->

