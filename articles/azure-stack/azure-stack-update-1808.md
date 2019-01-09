---
title: Azure Stack-Update 1808 | Microsoft Docs
description: Erfahren Sie, welche Neuerungen im Update 1808 für integrierte Azure Stack-Systeme enthalten sind, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 86f4e99401278d13a17f40c4c021060e8bd15f8a
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2018
ms.locfileid: "53754542"
---
# <a name="azure-stack-1808-update"></a>Azure Stack-Update 1808

*Anwendungsbereich: Integrierte Azure Stack-Systeme*

Dieser Artikel beschreibt den Inhalt des Updatepakets 1808. Das Updatepaket enthält Verbesserungen, Fehlerbehebungen und bekannten Probleme für diese Version von Azure Stack. Dieser Artikel enthält auch einen Link, damit Sie das Update herunterladen können. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]  
> Dieses Updatepaket gilt nur für integrierte Azure Stack-Systeme. Wenden Sie dieses Updatepaket nicht auf das Azure Stack Development Kit an.

## <a name="build-reference"></a>Buildreferenz

Die Buildnummer des Azure Stack-Updates 1808 ist **1.1808.0.97**.  

### <a name="new-features"></a>Neue Funktionen

Dieses Update enthält die folgenden Verbesserungen für Azure Stack.

<!--  2682594   | IS  --> 
- **Alle Azure Stack-Umgebungen verwenden nun das Zeitzonenformat Koordinierte Weltzeit (UTC).**  Alle Protokolldaten und zugehörigen Informationen werden nun im UTC-Format angezeigt. Wenn Sie ein Update aus einer früheren Version ausführen, die nicht mit UTC installiert wurde, wird Ihre Umgebung auf UTC aktualisiert. 

<!-- 2437250  | IS  ASDK --> 
- **Managed Disks werden unterstützt.** Sie können jetzt Managed Disks in virtuellen Azure Stack-Computern und VM-Skalierungsgruppen verwenden. Weitere Informationen finden Sie unter [Azure Stack Managed Disks: Unterschiede und Überlegungen](/azure/azure-stack/user/azure-stack-managed-disk-considerations).

<!-- 2563799  | IS  ASDK --> 
- **Azure Monitor**: Wie in Azure stellt Azure Monitor in Azure Stack grundlegende Infrastrukturmetriken und Protokolle für die meisten Dienste bereit. Weitere Informationen finden Sie unter [Azure Monitor in Azure Stack](/azure/azure-stack/user/azure-stack-metrics-azure-data).

<!-- 2487932| IS --> 
- **Vorbereiten auf den Erweiterungshost**. Sie können Azure Stack mit dem Erweiterungshost sicherer machen, indem Sie die Anzahl der erforderlichen TCP/IP-Ports reduzieren. Mit dem Update 1808 können Sie sich vorbereiten und Azure Stack für den Erweiterungshost bereit machen. Weitere Informationen finden Sie unter [Vorbereiten auf den Erweiterungshost für Azure Stack](/azure/azure-stack/azure-stack-extension-host-prepare).

<!-- IS --> 
- **Katalogelemente für Virtual Machine Scale Sets sind jetzt integriert**. Das Virtual Machine Scale Set-Katalogelement steht jetzt in den Benutzer- und Administratorportalen zur Verfügung, ohne dass es heruntergeladen werden muss.  Wenn Sie ein Upgrade auf 1808 ausführen, steht es nach Abschluss des Upgrades zur Verfügung.  

<!-- IS, ASDK --> 
- **Virtual Machine Scale Set-Skalierung**. Sie können das Portal verwenden, um eine [VMSS](azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (Virtual Machine Scale Set) zu skalieren.    

<!-- 2489570 | IS ASDK--> 
- **Unterstützung für benutzerdefinierte IPsec-/IKE-Richtlinienkonfigurationen** für [VPN-Gateways in Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

<!-- | IS ASDK--> 
- **Kubernetes-Marketplace-Element**. Sie können nun Kubernetes-Cluster mithilfe des [Kubernetes-Marketplace-Elements](azure-stack-solution-template-kubernetes-cluster-add.md) bereitstellen. Benutzer können das Kubernetes-Element auswählen und einige Parameter eingeben, um einen Kubernetes-Cluster in Azure Stack bereitzustellen. Mithilfe der Vorlagen wird es den Benutzern einfach gemacht, in wenigen Schritten Dev/Test-Kubernetes-Bereitstellungen einzurichten.

<!-- | IS ASDK--> 
- **Blockchainvorlagen**. Sie können jetzt [Ethereum-Konsortiumbereitstellungen](user/azure-stack-ethereum.md) in Azure Stack ausführen. Sie finden drei neue Vorlagen in den [Azure Stack-Schnellstartvorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates). Sie ermöglichen es dem Benutzer, ein Ethereum-Konsortiumnetzwerk mit mehreren Elementen mit minimalen Azure- und Ethereum-Kenntnissen bereitzustellen und zu konfigurieren. Mithilfe der Vorlagen wird es den Benutzern einfach gemacht, in wenigen Schritten Dev/Test-Blockchainbereitstellungen einzurichten.

<!-- | IS ASDK--> 
- **Das API-Versionsprofil 2017-03-09-profile wurde in 2018-03-01-hybrid aktualisiert**. API-Profile geben den Azure-Ressourcenanbieter und die API-Version für Azure-REST-Endpunkte an. Weitere Informationen zu Profilen finden Sie unter [Verwalten von API-Versionsprofilen in Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles).

### <a name="fixed-issues"></a>Behobene Probleme

<!-- IS ASDK--> 
- Wir haben das Problem bei der Erstellung einer Verfügbarkeitsgruppe im Portal behoben, das dazu führte, dass die Gruppe eine Fehlerdomäne und eine Updatedomäne von 1 hatte. 

<!-- IS ASDK --> 
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind nun im Portal verfügbar.  

<!-- 2494144- IS, ASDK --> 
- Das Problem, das verhindert hat, dass einige VM-Größen der F-Serie bei der Auswahl einer VM-Größe für die Bereitstellung nicht angezeigt wurden, ist nun behoben. 

<!-- IS, ASDK --> 
- Verbesserungen für die Leistung beim Erstellen von virtuellen Computern und optimierte Verwendung des zugrunde liegenden Speichers.

- Es wurden **verschiedene Fehlerbehebungen** hinsichtlich der Leistung, Stabilität, Sicherheit und des von Azure Stack eingesetzten Betriebssystems vorgenommen.


### <a name="changes"></a>Änderungen
<!-- 1697698  | IS, ASDK --> 
- *Schnellstarttutorials* im Benutzerportaldashboard weisen jetzt Links zu relevanten Artikeln in der Azure Stack-Dokumentation auf.

<!-- 2515955   | IS ,ASDK--> 
- *Alle Dienste* ersetzt *Weitere Dienste* in den Azure Stack-Administrator- und -Benutzerportalen. Sie können nun *Alle Dienste* als Alternative zum Navigieren in den Azure Stack-Portalen wie in den Azure-Portalen verwenden.

<!-- TBD | IS, ASDK --> 
- *+ Ressource erstellen* ersetzt *+ Neu* in den Azure Stack Administrator- und Benutzerportalen.  Sie können nun *Ressource erstellen* als Alternative zum Navigieren in den Azure Stack-Portalen wie in den Azure-Portalen verwenden.  

<!--  TBD – IS, ASDK --> 
- Die VM-Größen *Basic A* stehen für das [Erstellen von VM-Skalierungsgruppen](azure-stack-compute-add-scalesets.md) (VMSS) über das Portal nicht mehr zur Verfügung. Verwenden Sie PowerShell oder eine Vorlage, um eine VMSS mit dieser Größe zu erstellen.  

### <a name="common-vulnerabilities-and-exposures"></a>Common Vulnerabilities and Exposures (CVE, allgemeine Sicherheitslücken und Schwachstellen)

Dieses Update installiert die folgenden Updates:  

- [CVE-2018-0952](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0952)
- [CVE-2018-8200](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8200)
- [CVE-2018-8204](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8204)
- [CVE-2018-8253](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8253)
- [CVE-2018-8339](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8339)
- [CVE-2018-8340](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8340)
- [CVE-2018-8341](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8341)
- [CVE-2018-8343](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8343)
- [CVE-2018-8344](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8344)
- [CVE-2018-8345](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8345)
- [CVE-2018-8347](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8347)
- [CVE-2018-8348](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8348)
- [CVE-2018-8349](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8349)
- [CVE-2018-8394](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8394)
- [CVE-2018-8398](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8398)
- [CVE-2018-8401](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8401)
- [CVE-2018-8404](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8404)
- [CVE-2018-8405](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8405)
- [CVE-2018-8406](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8406)  

Weitere Informationen zu diesen Sicherheitslücken erhalten Sie durch Klicken auf die vorhergehenden Links oder im Microsoft Knowledge Base-Artikel [4343887](https://support.microsoft.com/help/4343887).

Dieses Update enthält auch die Minderung des Sicherheitsrisikos für die spekulative Ausführung des Seitenkanals, bekannt als L1 Terminal Fault (L1TF), beschrieben in der [Microsoft-Sicherheitsempfehlung ADV180018](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018).  

### <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie das Azure Stack-[Update 1807](azure-stack-update-1807.md), bevor Sie das Azure Stack-Update 1808 anwenden. 

- Installieren Sie das neueste verfügbare [Update oder den Hotfix für Version 1807](azure-stack-update-1807.md#post-update-steps).  
  > [!TIP]  
  > Abonnieren Sie die folgenden *RRS*- oder *Atom*-Feeds, um im Hinblick auf Azure Stack-Hotfixes auf dem neuesten Stand zu bleiben:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss … 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom …


- Bevor Sie mit der Installation dieses Updates beginnen, führen Sie [Test-AzureStack](azure-stack-diagnostic-test.md) mit den folgenden Parametern aus, um den Status von Azure Stack zu überprüfen und alle gefundenen operativen Probleme (einschließlich aller Warnungen und Fehler) zu beheben. Überprüfen Sie auch aktive Warnungen, und lösen Sie solche auf, die eine Aktion erfordern.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ```   

### <a name="known-issues-with-the-update-process"></a>Bekannte Probleme mit dem Updateprozess

- Wenn Sie [Test-AzureStack](azure-stack-diagnostic-test.md) nach dem Update 1808 ausführen, wird eine Warnmeldung des Baseboard Management Controllers (BMC) angezeigt. Sie können diese Warnung problemlos ignorieren.

<!-- 2468613 - IS --> 
- Während der Installation des Updates werden möglicherweise Warnungen mit folgender Meldung angezeigt: *Fehler: Die Vorlage für FaultType UserAccounts.New fehlt.*  Sie können diese Warnungen ignorieren. Diese Warnungen werden automatisch nach der Installation dieses Updates geschlossen.

<!-- 2489559 - IS --> 
- Versuchen Sie nicht, während der Installation dieses Updates virtuelle Computer zu erstellen. Weitere Informationen zum Verwalten von Updates finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md#plan-for-updates).

<!-- 2830461 - IS --> 
- Wenn ein Update eine Aktion erfordert, wird die entsprechende Warnung unter bestimmten Umständen möglicherweise nicht generiert. Der genaue Status wird im Portal dennoch angegeben und nicht beeinträchtigt.

### <a name="post-update-steps"></a>Schritte nach dem Update

> [!Important]  
> Bereiten Sie Ihre Azure Stack-Bereitstellung auf den Erweiterungshost vor. Bereiten Sie Ihr System mithilfe der folgenden Anleitung vor: [Vorbereiten auf den Erweiterungshost für Azure Stack](azure-stack-extension-host-prepare.md).

Installieren Sie nach der Installation dieses Updates alle entsprechenden Hotfixes. Weitere Informationen finden Sie in den folgenden Knowledge Base-Artikeln sowie in unserer [Wartungsrichtlinie](azure-stack-servicing-policy.md). 
- [KB 4481066 – Azure Stack Hotfix Azure Stack Hotfix 1.1808.9.117](https://support.microsoft.com/help/4481066/)


## <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

Im Folgenden werden bekannte Probleme nach der Installation zu dieser Buildversion vorgestellt.

### <a name="portal"></a>Portal

- Die technische Dokumentation für Azure Stack konzentriert sich auf das neueste Release von Azure Stack. Aufgrund von Portaländerungen zwischen den Releases kann das, was bei der Verwendung der Azure Stack-Portale angezeigt wird, von dem abweichen, was in der Dokumentation angezeigt wird. 

<!-- TBD - IS ASDK --> 
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Klicken Sie zum Wiederherstellen des Dashboards auf **Dashboard bearbeiten**, klicken Sie dann mit der rechten Maustaste, und wählen Sie **Auf Standardzustand zurücksetzen** aus.

<!-- 2930718 - IS ASDK --> 
- Im Administratorportal wird beim Zugriff auf die Details eines Benutzerabonnements nach dem Schließen des Blatts und dem Klicken auf **Aktuell** der Name des Benutzerabonnements nicht angezeigt.

<!-- 3060156 - IS ASDK --> 
- Sowohl im Administrator- als auch im Benutzerportal funktioniert das Klicken auf die Portaleinstellungen und das Auswählen von **Alle Einstellungen und private Dashboards löschen** nicht wie erwartet. Es wird eine Fehlerbenachrichtigung angezeigt. 

<!-- 2930799 - IS ASDK --> 
- In den Administrator- und Benutzerportalen wird unter **Alle Dienste** die Ressource **DDoS Protection-Pläne** nicht ordnungsgemäß aufgeführt. Sie ist in Azure Stack nicht verfügbar. Wenn Sie versuchen, sie zu erstellen, wird ein Fehler angezeigt, der besagt, dass das Portal das Marketplace-Element nicht erstellen konnte. 

<!-- 2930820 - IS ASDK --> 
- Wenn Sie im Administrator- oder im Benutzerportal nach „Docker“ suchen, wird das Element nicht ordnungsgemäß zurückgegeben. Sie ist in Azure Stack nicht verfügbar. Wenn Sie versuchen, sie zu erstellen, wird ein Blatt mit einem Fehlerhinweis angezeigt. 

<!-- 2967387 – IS, ASDK --> 
- Das Konto, mit dem Sie sich beim Administrator- oder Benutzerportal von Azure Stack anmelden, wird als **Unbekannter Benutzer** angezeigt. Dieses Verhalten tritt auf, wenn für das Konto kein *Vorname* oder *Nachname* angegeben wurde. Um dieses Problem zu umgehen, bearbeiten Sie das Benutzerkonto so, dass der Vor- oder Nachname angegeben wird. Sie müssen sich dann abmelden und erneut am Portal anmelden. 

<!--  2873083 - IS ASDK --> 
-  Wenn Sie das Portal verwenden, um eine VM-Skalierungsgruppe zu erstellen, wird das Dropdownmenü *Instanzgröße* bei Verwendung von Internet Explorer nicht ordnungsgemäß geladen. Um dieses Problem zu umgehen, nutzen Sie einen anderen Browser beim Verwenden des Portals zum Erstellen einer VMSS.  

<!-- 2931230 – IS  ASDK --> 
- Pläne, die einem Benutzerabonnement als Add-On-Plan hinzugefügt wurden, können nicht gelöscht werden, auch wenn Sie den Plan aus dem Benutzerabonnement entfernen. Der Plan ist so lange vorhanden, bis die Abonnements gelöscht werden, die auf den Add-On-Plan verweisen. 

<!--2760466 – IS  ASDK --> 
- Bei der Installation einer neuen Azure Stack-Umgebung, die diese Version ausführt, wird die Warnung, dass eine *Aktivierung erforderlich* ist, möglicherweise nicht angezeigt. Die [Aktivierung](azure-stack-registration.md) ist erforderlich, damit Sie die Marketplace-Syndikation verwenden können.  

<!-- TBD - IS ASDK --> 
- Die zwei administrativen Abonnementtypen, die in Version 1804 eingeführt wurden, sollten nicht verwendet werden. Die Abonnementtypen sind **Messungsabonnement** und **Verbrauchsabonnement**. Diese Abonnementtypen sind in neuen Azure Stack-Umgebungen ab Version 1804 sichtbar, aber noch nicht zur Verwendung bereit. Sie sollten den Abonnementtyp **Standardanbieter** weiterhin verwenden.

<!-- TBD - IS ASDK --> 
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.

<!-- TBD - IS ASDK --> 
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Verwenden Sie für die Problemumgehung PowerShell, um Berechtigungen zu überprüfen.


### <a name="health-and-monitoring"></a>Integrität und Überwachung

<!-- TBD - IS -->
- Die folgenden Warnungen werden in Ihrem Azure Stack-System möglicherweise wiederholt angezeigt und dann nicht mehr angezeigt:
   - *Infrastrukturrolleninstanz nicht verfügbar*
   - *Skalierungseinheitknoten ist offline*
   
  Führen Sie das Cmdlet [Test-AzureStack](azure-stack-diagnostic-test.md) aus, um die Integrität der Infrastrukturrolleninstanzen und Skalierungseinheitenknoten zu überprüfen. Wenn von [Test-AzureStack](azure-stack-diagnostic-test.md) keine Probleme gefunden werden, können Sie diese Warnungen ignorieren. Wenn ein Problem erkannt wird, können Sie versuchen, die Infrastrukturrolleninstanz oder den Knoten über das Verwaltungsportal oder PowerShell zu starten.

  Dieses Problem wurde im aktuellen [Hotfixrelease 1808](https://support.microsoft.com/help/4481066/) behoben. Sie sollten diesen Hotfix daher unbedingt installieren, falls das Problem auftritt.

<!-- 1264761 - IS ASDK --> 
- Möglicherweise werden Warnungen für die **Health Controller**-Komponente mit folgenden Details angezeigt:  

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


<!-- 2812138 | IS --> 
- Möglicherweise wird eine Warnung für die **Storage**-Komponente mit den folgenden Details angezeigt:

   - NAME: Interner Kommunikationsfehler des Storage-Diensts  
   - SCHWEREGRAD: Kritisch  
   - KOMPONENTE: Storage  
   - BESCHREIBUNG: Beim Senden von Anforderungen an die folgenden Knoten ist ein interner Kommunikationsfehler des Storage-Diensts aufgetreten.  

    Die Warnung kann ignoriert werden, Sie müssen sie jedoch manuell schließen.

<!-- 2368581 - IS. ASDK --> 
- Wenn Sie als Azure Stack-Operator eine Fehlermeldung über unzureichenden Speicher erhalten und virtuelle Computer der Mandanten nicht mit einem **Fabric-VM-Erstellungsfehler** bereitgestellt werden können, verfügt der Azure Stack-Stempel möglicherweise nicht über genügend Arbeitsspeicher. Mit dem [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) können Sie die verfügbare Kapazität für Ihre Workloads besser bestimmen.

### <a name="compute"></a>Compute

<!-- 3164607 – IS, ASDK -->
- Das erneute Anfügen eines abgetrennten Datenträgers an denselben virtuellen Computer mit dem gleichen Namen und der gleichen LUN führt zu einem Fehler wie **Datenträger „Datenträger“ kann nicht an VM „vm1“ angefügt werden**. Der Fehler tritt auf, da der Datenträger derzeit getrennt wird oder beim letzten Trennvorgang ein Fehler aufgetreten ist. Warten Sie, bis der Datenträger vollständig getrennt wurde, und versuchen Sie es dann erneut, oder löschen/trennen Sie den Datenträger explizit erneut. Die Problemumgehung besteht darin, ihn unter einem anderen Namen oder mit einer anderen LUN erneut anzufügen. 

<!-- 3099544 – IS, ASDK --> 
- Wenn Sie im Azure Stack-Portal eine neue VM erstellen und die VM-Größe auswählen, wird die Spalte „USD/Monat“ mit der Meldung **Nicht verfügbar** angezeigt. Diese Spalte sollte nicht angezeigt werden. Die Anzeige der VM-Preisspalte wird in Azure Stack nicht unterstützt.

<!-- 3090289 – IS, ASDK --> 
- Nach dem Anwenden des Updates 1808 können beim Bereitstellen von virtuellen Computern mit Managed Disks die folgenden Probleme auftreten:

   1. Wenn das Abonnement vor dem Update 1808 erstellt wurde, kann die Bereitstellung eines virtuellen Computers mit Managed Disks mit einer internen Fehlermeldung fehlschlagen. Um den Fehler zu beheben, führen Sie die folgenden Schritte für jedes Abonnement aus:
      1. Navigieren Sie im Mandantenportal zu **Abonnements**, und suchen Sie nach dem Abonnement. Klicken Sie auf **Ressourcenanbieter**, klicken Sie dann auf **Microsoft.Compute**, und klicken Sie anschließend auf **Erneut registrieren**.
      2. Navigieren Sie unter dem gleichen Abonnement zu **Zugriffssteuerung (IAM)**, und überprüfen Sie, ob **Azure Stack – Verwalteter Datenträger** aufgeführt wird.
   2. Wenn Sie eine Umgebung mit mehreren Mandanten konfiguriert haben, kann die Bereitstellung von virtuellen Computern in einem Abonnement, dem ein Gastverzeichnis zugeordnet ist, mit einer internen Fehlermeldung fehlschlagen. Gehen Sie folgendermaßen vor, um den Fehler zu beheben:
      1. Wenden Sie den [Azure Stack-Hotfix 1808](https://support.microsoft.com/help/4481066/) an.
      2. Führen Sie die in [diesem Artikel](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) beschriebenen Schritte aus, um alle Gastverzeichnisse neu zu konfigurieren.
      
<!-- 3179561 - IS --> 
- Die Verwendung von Managed Disks wird in Stunden gemeldet, wie in den [häufig gestellte Fragen zur Azure Stack-Nutzung](azure-stack-usage-related-faq.md#managed-disks) beschrieben. Bei der Azure Stack-Abrechnung wird jedoch der monatliche Preis verwendet, sodass die Managed Disks-Nutzung bis zum 27. September eventuell nicht ordnungsgemäß in Rechnung gestellt werden kann. Wir haben die Gebühren für Managed Disks nach dem 27. September vorübergehend unterbrochen, bis auf das Abrechnungsproblem eingegangen werden kann. Wenn Ihnen Ihre Managed Disks-Nutzung nicht ordnungsgemäß berechnet wurde, wenden Sie sich an den Abrechnungssupport von Microsoft.
Von den Azure Stack-Nutzungs-APIs erstellte Nutzungsberichte zeigen richtige Mengen an und können verwendet werden.

<!-- 3507629 - IS, ASDK --> 
- Managed Disks erstellt zwei neue [Computekontingenttypen](azure-stack-quota-types.md#compute-quota-types) zur Begrenzung der maximalen Kapazität von verwalteten Datenträgern, die bereitgestellt werden können. Standardmäßig werden für jeden Kontingenttyp von verwalteten Datenträgern 2.048 GiB zugeordnet. Allerdings können die folgenden Probleme auftreten:

   - Bei Kontingenten, die vor dem Update 1808 erstellt wurden, werden im Administratorportal für das "Managed Disks"-Kontingent 0 Werte angezeigt, obwohl 2.048 GiB zugeordnet wurden. Sie können den Wert auf der Grundlage Ihrer tatsächlichen Anforderungen erhöhen oder verringern. Dann setzt der neu festgelegte Kontingentwert den Standardwert 2.048 GiB außer Kraft.
   - Wenn Sie den Kontingentwert auf „0“ aktualisieren, entspricht dies dem Standardwert 2.048 GiB. Als Problemumgehung können Sie den Kontingentwert auf „1“ festlegen.

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

<!-- 2724873 - IS --> 
- Wenn Sie die PowerShell-Cmdlets **Start-AzsScaleUnitNode** oder **Stop-AzsScaleunitNode** verwenden, um Skalierungseinheiten zu verwalten, tritt beim ersten Versuch, die Skalierungseinheit zu starten oder zu beenden, möglicherweise ein Fehler auf. Wenn beim ersten Ausführen des Cmdlets ein Fehler auftritt, führen Sie das Cmdlet ein zweites Mal aus. Bei der zweiten Ausführung sollte der Vorgang erfolgreich sein. 

<!-- TBD - IS ASDK --> 
- Beim Erstellen von virtuellen Computern im Azure Stack-Benutzerportal zeigt das Portal eine falsche Anzahl von Datenträgern an, die an eine VM der Serie DS angefügt werden können. VMs der Serie DS können so viele Datenträger wie bei der Azure-Konfiguration aufnehmen.

<!-- TBD - IS ASDK --> 
- Wenn die Bereitstellung einer Erweiterung für eine VM-Bereitstellung zu lange dauert, sollten Benutzer eine Zeitüberschreitung der Bereitstellung zulassen und nicht versuchen, den Vorgang zum Aufheben der Zuordnung oder Löschen der VMs zu beenden.  

<!-- 1662991 IS ASDK --> 
- Die Linux-VM-Diagnose wird in Azure Stack nicht unterstützt. Wenn Sie eine Linux-VM mit aktivierter VM-Diagnose bereitstellen, schlägt die Bereitstellung fehl. Die Bereitstellung schlägt auch fehl, wenn Sie die grundlegenden Linux-VM-Metriken über die Diagnoseeinstellungen aktivieren.  

<!-- 2724961- IS ASDK --> 
- Wenn Sie den Ressourcenanbieter **Microsoft.Insight** in den Abonnementeinstellungen registrieren und eine Windows-VM mit aktivierter Gastbetriebssystemdiagnose erstellen, können im Diagramm „CPU-Prozentsatz“ auf der Übersichtsseite der VM keine Metrikdaten angezeigt werden.

   Navigieren Sie zum Anzeigen des Diagramms „CPU-Prozentsatz“ für die VM zum Blatt **Metriken**, und zeigen Sie alle unterstützten Gastmetriken für Windows-VMs an.

- Ein virtueller Ubuntu 18.04-Computer, der mit aktivierter SSH-Autorisierung erstellt wurde, lässt nicht zu, dass Sie die SSH-Schlüssel für die Anmeldung verwenden. Um dieses Problem zu umgehen, verwenden Sie VM-Zugriff für die Linux-Erweiterung, um SSH-Schlüssel nach der Bereitstellung zu implementieren, oder verwenden Sie kennwortbasierte Authentifizierung.

### <a name="networking"></a>Netzwerk  

<!-- 1766332 - IS ASDK --> 
- Wenn Sie unter **Netzwerk** auf **VPN Gateway erstellen** klicken, um eine VPN-Verbindung einzurichten, wird als VPN-Typ **Richtlinienbasiert** aufgeführt. Wählen Sie diese Option nicht aus. Nur die Option **Routenbasiert** wird in Azure Stack unterstützt.

<!-- 1902460 - IS ASDK --> 
- Azure Stack unterstützt ein einzelnes *Gateway eines lokalen Netzwerks* pro IP-Adresse. Dies gilt für alle Mandantenabonnements. Nach der Herstellung der Verbindung mit dem ersten Gateway eines lokalen Netzwerks werden nachfolgende Versuche zum Erstellen einer Ressource für Gateways lokaler Netzwerke mit der gleichen IP-Adresse blockiert.

<!-- 16309153 - IS ASDK --> 
- In einem virtuellen Netzwerk, das mit der DNS-Servereinstellung *Automatisch* erstellt wurde, tritt bei der Änderung eines benutzerdefinierten DNS-Servers ein Fehler auf. Die aktualisierten Einstellungen werden nicht per Pushvorgang auf VMs in diesem VNET übertragen.

<!-- 2702741 -  IS ASDK --> 
- Öffentliche IP-Adressen, die mithilfe der dynamischen Zuordnungsmethode bereitgestellt wurden, werden nach der Ausgabe eines Befehls zum Beenden und Aufheben der Zuordnung nicht unbedingt beibehalten.

<!-- 2529607 - IS ASDK --> 
- Während der *Geheimnisrotation* von Azure Stack sind öffentliche IP-Adressen für einen Zeitraum von zwei bis fünf Minuten nicht erreichbar.

<!-- 2664148 - IS ASDK --> 
- In Szenarien, in denen der Mandant über einen S2S-VPN-Tunnel auf seine VMs zugreift, kann es passieren, dass bei Verbindungsversuchen Fehler auftreten, wenn das lokale Subnetz dem lokalen Netzwerkgateway erst nach der Erstellung des Gateways hinzugefügt wurde. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Benutzer müssen den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

<!-- 2489178 - IS ASDK --> 
- Um die Infrastruktur (Worker-, Verwaltungs-, Front-End-Rollen) horizontal hochzuskalieren, müssen Sie PowerShell verwenden, wie in den Anmerkungen zu dieser Version für die Berechnung beschrieben.



### <a name="usage"></a>Verwendung  
<!-- TBD - IS ASDK --> 
- Die Ansicht „Verwendung“ für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz anstatt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Herunterladen des Updates

Sie können das Paket für das Azure Stack-Update 1808 [hier](https://aka.ms/azurestackupdatedownload) herunterladen. 

Nur in verbundenen Szenarios überprüfen Azure Stack-Bereitstellungen in regelmäßigen Abständen einen gesicherten Endpunkt und benachrichtigen Sie automatisch, wenn ein Update für Ihre Cloud verfügbar ist. Weitere Informationen finden Sie unter [Verwalten von Updates für Azure Stack](azure-stack-updates.md).

## <a name="next-steps"></a>Nächste Schritte
- Informationen zur Wartungsrichtlinie für integrierte Azure Stack-Systeme und dazu, wie Sie vorgehen müssen, um den unterstützten Zustand des Systems aufrechtzuerhalten, finden Sie unter [Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md).  
- Informationen zum Überwachen und Fortsetzen von Updates mithilfe des privilegierten Endpunkts (PEP) finden Sie unter [Überwachen von Änderungen in Azure Stack mithilfe des privilegierten Endpunkts](azure-stack-monitor-update.md).  
- Eine Übersicht über die Updateverwaltung in Azure Stack finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md).  
- Informationen zur Anwendung von Updates mit Azure Stack finden Sie unter [Anwenden von Updates in Azure Stack](azure-stack-apply-updates.md).  
