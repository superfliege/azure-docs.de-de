---
title: Azure Stack-Update 1712 | Microsoft-Dokumentation
description: Erfahren Sie, was in Update 1712 für integrierte Azure Stack-Systeme enthalten ist, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: b14f79ad-025f-45d8-9e1d-e53d2b420bb1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: brenduns
ms.openlocfilehash: c3cb8ab8a838a3f831ece617fcf3e218a9510ea5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="azure-stack-1712-update"></a>Azure Stack-Update 1712

*Gilt für: Integrierte Azure Stack-Systeme*

In diesem Artikel wird beschrieben, welche Verbesserungen und Fehlerbehebungen in diesem Updatepaket enthalten sind, welche bekannten Probleme es für dieses Release gibt und wo das Update heruntergeladen werden kann. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]
> Dieses Updatepaket gilt nur für integrierte Azure Stack-Systeme. Wenden Sie dieses Updatepaket nicht auf das Azure Stack Development Kit an.

## <a name="build-reference"></a>Buildreferenz

Die Buildnummer des Azure Stack-Updates 1712 ist **180106.1**. Wenn ein Kunde bereits **180103.2** bereitgestellt hat, muss **180106.1** nicht angewendet werden.

## <a name="before-you-begin"></a>Voraussetzungen

> [!IMPORTANT]
> Versuchen Sie nicht, während der Installation des Updates 1712 virtuelle Computer zu erstellen. Weitere Details finden Sie in der [Übersicht zum Verwalten von Updates in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#plan-for-updates).

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen zunächst das Azure Stack-[Update 1711](https://docs.microsoft.com/azure/azure-stack/azure-stack-update-1711) installieren, bevor Sie dieses Update anwenden können.

### <a name="post-update-steps"></a>Schritte nach dem Update

Dieses Update erfordert auch, dass Sie Firmware-Updates vom OEM-Partner installieren, nachdem Sie die Installation des Azure Stack-Updates 1712 abgeschlossen haben.

> [!NOTE]
> Besuchen Sie die Website Ihres OEM-Partners, um die Updates herunterzuladen.

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen

Dieses Update enthält die folgenden Verbesserungen und Fehlerbehebungen für Azure Stack.

#### <a name="new-features"></a>Neue Funktionen

- Cmdlet „Test-AzureStack“ zur Validierung der Azure Stack-Cloud über privilegierten Endpunkt verfügbar
- Möglichkeit zum Registrieren einer nicht mit Azure verbundenen Bereitstellung von Azure Stack
- Überwachung von Warnungen zum Ablauf von Zertifikaten und Benutzerkonten
- Cmdlet „Set-BmcPassword“ in PEP für BMC-Kennwortrotation hinzugefügt
- Updates für die Netzwerkprotokollierung zur Unterstützung der bedarfsgesteuerten Protokollierung
- Durchführung des Reimagingvorgangs für VM-Skalierungsgruppen unterstützt
- Kioskmodus für ERCS-VM zur CloudAdmin-Anmeldung aktiviert
- Mandanten können virtuelle Windows-Computer automatisch aktivieren

#### <a name="fixes"></a>Fehlerbehebungen

- Korrektur zur Anzeige des Betriebsstatus von Knoten in der Wartung bei laufender Reparatur
- Korrektur zum Berichtigen des Uhrzeit-/Datumsstempels der Nutzungsdatensätze öffentlicher IP-Adressen
- Verschiedene andere Korrekturen in Bezug auf Leistung, Sicherheit und Stabilität
- Fehlerkorrekturen für die Module „TimeSource“ und „Defender“ auf privilegiertem Endpunkt

#### <a name="windows-server-2016-new-features-and-fixes"></a>Windows Server 2016 – neue Features und Fehlerbehebungen

- [3. Januar 2018: KB4056890 (Betriebssystembuild 14393.2007)](https://support.microsoft.com/help/4056890/windows-10-update-kb4056890)
    - Dieses Update enthält die Softwarekorrekturen für das branchenweite Sicherheitsproblem, das in der [MSRC-Sicherheitsempfehlung ADV 180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) beschrieben wird.

### <a name="known-issues-with-the-update-process"></a>Bekannte Probleme mit dem Updateprozess

Dieser Abschnitt enthält bekannte Probleme, die bei der Installation von Update 1712 auftreten können.

1. **Symptom:** Azure Stack-Benutzer sehen möglicherweise folgenden Fehler beim Aktualisierungsvorgang: *"Type 'CheckHealth' of Role 'VirtualMachines' raised an exception:\n\nVirtual Machine health check for -ACS01 produced the following errors.\nThere was an error getting VM information from hosts. Exception details:\nGet-VM : The operation on computer 'Node03' failed: The WS-Management service cannot process the request. The WMI \nservice or the WMI provider returned an unknown error: HRESULT 0x8004106c."*
    1. **Ursache:** Dieses Problem wird durch ein Windows Server-Problem ausgelöst, das bei späteren Updates von Windows Server behoben werden soll.
    2. **Lösung:** Wenden Sie sich an den Microsoft-Kundendienst und -Support, um Hilfe zu erhalten.
<br><br>
2. **Symptom:** Azure Stack-Benutzer sehen möglicherweise folgenden Fehler beim Aktualisierungsvorgang: *"Enabling the seed ring VM failed on node Host-Node03 with an error: [Host-Node03] Connecting to remote server Host-Node03 failed with the following error message: Der WinRM-Client hat einen Status in Bezug auf einen HTTP-Serverfehler (500) empfangen, aber der Remotedienst hat keine anderen Informationen zur Fehlerursache bereitgestellt."*
    1. **Ursache:** Dieses Problem wird durch ein Windows Server-Problem ausgelöst, das bei späteren Updates von Windows Server behoben werden soll. 
    2. **Lösung:** Wenden Sie sich an den Microsoft-Kundendienst und -Support, um Hilfe zu erhalten.
<br><br>

### <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

Dieser Abschnitt enthält bekannte Probleme, die für Build **180106.1** nach der Installation auftreten können.

#### <a name="portal"></a>Portal

- Unter Umständen ist es nicht möglich, Compute- oder Speicherressourcen im Administratorportal anzuzeigen. Es wird angegeben, dass während der Installation des Updates ein Fehler aufgetreten ist und dass das Update fälschlicherweise als erfolgreich gemeldet wurde. Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten, wenn dieses Problem auftritt.
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
- Für alle Workflows, für die Sie in einer Dropdownliste ein Abonnement, eine Ressourcengruppe oder einen Standort auswählen, können unter Umständen folgende Probleme auftreten:

   - Ggf. wird oben in der Liste eine leere Zeile angezeigt. Es sollte trotzdem möglich sein, ein Element auf normale Weise auszuwählen.
   - Wenn die Dropdownliste mit den Elementen kurz ist, ist es ggf. nicht möglich, die Elementnamen anzuzeigen.
   - Wenn Sie über mehrere Benutzerabonnements verfügen, kann die Dropdownliste für die Ressourcengruppen leer sein.

        > [!NOTE]
        > Zum Umgehen der beiden letztgenannten Probleme können Sie den Namen des Abonnements oder der Ressourcengruppe eingeben (falls bekannt), oder Sie können stattdessen PowerShell verwenden.

- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
- Das Blatt **Service Health** wird nicht geladen. Beim Öffnen des Blatts „Service Health“ im Verwaltungs- oder Benutzerportal zeigt Azure Stack eine Fehlermeldung an, und Informationen werden nicht geladen. Dieses Verhalten wird erwartet. Sie können Service Health zwar auswählen und öffnen, dieses Feature ist jedoch noch nicht verfügbar, sondern wird in einer zukünftigen Version von Azure Stack implementiert.

#### <a name="health-and-monitoring"></a>Integrität und Überwachung

- Wenn Sie eine Infrastrukturrolleninstanz neu starten, wird ggf. eine Meldung mit dem Hinweis angezeigt, dass der Neustart nicht erfolgreich war. Der Neustart wurde aber erfolgreich durchgeführt.

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
- Sie können die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer nicht aufheben, nachdem der virtuelle Computer erstellt und dieser IP-Adresse zugeordnet wurde. Die Aufhebung der Zuordnung war scheinbar erfolgreich, die zuvor zugewiesene öffentliche IP-Adresse bleibt dem ursprünglichen virtuellen Computer jedoch zugeordnet. Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird. Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer virtueller Computer verwenden.
- Azure Stack-Benutzer können möglicherweise VNETs oder Netzwerksicherheitsgruppen nicht bereitstellen, löschen oder ändern. Dieses Problem tritt in erster Linie bei nachfolgenden Updateversuchen desselben Pakets auf. Die Ursache ist ein Packproblem beim Update, das derzeit geprüft wird.
- Der interne Lastenausgleich (ILB) behandelt MAC-Adressen für Back-End-VMs nicht ordnungsgemäß, wodurch Linux-Instanzen gestört werden.

#### <a name="sqlmysql"></a>SQL/MySQL
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können.
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

    > [!NOTE]
    > Beim Update Ihrer in Azure Stack integrierten Systeme auf Version 1712 sollte es keine Auswirkungen auf Ihre bestehenden SQL- oder MySQL-Ressourcenanbieterbenutzer geben. Sie können weiterhin Ihre aktuellen SQL- oder MySQL-Ressourcenanbieterbuilds verwenden, bis ein neues Azure Stack-Update verfügbar ist.

#### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.

#### <a name="identity"></a>Identity

In Umgebungen, die in Azure Active Directory-Verbunddienste (AD FS) bereitgestellt wurden, ist das Konto **azurestack\azurestackadmin** nicht mehr der Besitzer des Standardabonnements des Anbieters. Anstelle der Protokollierung in **Administratorportal/Admin-Verwaltungsendpunkt** mit **azurestack\azurestackadmin** können Sie das Konto **azurestack\cloudadmin** verwenden, um das Standardabonnement des Anbieters zu verwalten und zu verwenden.

> [!IMPORTANT]
> Obwohl das Konto **azurestack\cloudadmin** in Umgebungen, die in AD FS bereitgestellt wurden, der Besitzer des Standardabonnements des Anbieters ist, verfügt es nicht über die Berechtigung für RDP-Verbindungen mit dem Host. Verwenden Sie weiterhin das Konto **azurestack\azurestackadmin** oder das lokale Administratorkonto, um sich bei Bedarf auf dem Host anzumelden, auf diesen zuzugreifen oder ihn zu verwalten.

## <a name="download-the-update"></a>Herunterladen des Updates

Sie können das Paket für das Azure Stack-Update 1712 [hier](https://aka.ms/azurestackupdatedownload) herunterladen.

## <a name="more-information"></a>Weitere Informationen

Microsoft stellt mithilfe des privilegierten Endpunkts (PEP), der mit Update 1712 installiert wird, eine Möglichkeit zum Überwachen und Fortsetzen von Updates bereit.

- Weitere Informationen finden Sie in der Dokumentation unter [Überwachen von Änderungen in Azure Stack mithilfe des privilegierten Endpunkts](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update). 

## <a name="see-also"></a>Weitere Informationen

- Eine Übersicht über die Updateverwaltung in Azure Stack finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md).
- Informationen zur Anwendung von Updates mit Azure Stack finden Sie unter [Anwenden von Updates in Azure Stack](azure-stack-apply-updates.md).
