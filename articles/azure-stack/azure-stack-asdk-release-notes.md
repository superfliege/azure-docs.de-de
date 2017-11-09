---
title: "Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation"
description: Verbesserungen, Fehlerbehebungen und bekannte Probleme in Azure Stack Development Kit
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: twooley
ms.openlocfilehash: 81ccb4a731b71f87bccb2f2a0e333443428f32ee
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit

*Gilt für: Azure Stack Development Kit*

Die Anmerkungen zu dieser Version enthalten Informationen zu Verbesserungen, Fehlerbehebungen und bekannten Problemen in Azure Stack Development Kit. Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](azure-stack-updates.md#determine-the-current-version).

## <a name="build-201710201"></a>Build 20171020.1

### <a name="improvements-and-fixes"></a>Verbesserungen und Fehlerbehebungen

Eine Liste der Verbesserungen und Fehlerbehebungen in Build 20171020.1 finden Sie im Abschnitt [Verbesserungen und Fehlerbehebungen](azure-stack-update-1710.md#improvements-and-fixes) der Anmerkungen zu Version 1710 für integrierte Azure Stack-Systeme. Einige der Elemente, die im Abschnitt „Zusätzliche Qualitätsverbesserungen und Fehlerbehebungen“ aufgeführt sind, sind nur für integrierte Systeme relevant.

Außerdem wurden folgende Fehler behoben:
- Es wurde ein Problem behoben, bei dem der Computeressourcenanbieter einen unbekannten Status anzeigt.
- Es wurde ein Problem behoben, bei dem Kontingente nicht im Administratorportal angezeigt wurden, nachdem Sie diese erstellt und später versucht haben, Plandetails anzuzeigen.

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="powershell"></a>PowerShell
- Das Release des PowerShell-Moduls AzureRM 1.2.11 enthält eine Liste mit wichtigen Änderungen. Informationen zu einem Upgrade aus Version 1.2.10 finden Sie im [Migrationshandbuch](https://aka.ms/azspowershellmigration).
 
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
- Klicken Sie in den Details der Warnung **Aktivierung erforderlich** nicht auf den Link zu der Komponente **AzureBridge**. Wenn Sie dies tun, wird versucht, das Blatt **Übersicht** zu laden. Dies ist jedoch nicht erfolgreich, und es erfolgt kein Timeout.
- Im Administratorportal wird Ihnen möglicherweise der Fehler **Fehler beim Abrufen von Mandanten** im Bereich **Benachrichtigungen** angezeigt. Sie können diesen Fehler ignorieren.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
 
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
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

#### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
 
#### <a name="usage-and-billing"></a>Nutzung und Abrechnung
- Die Ansicht für die Nutzungsdaten öffentlicher IP-Adressen zeigt den gleichen *EventDateTime*-Wert für jeden Datensatz an statt des *TimeDate*-Stempels, der anzeigt, wann dieser jeweils erstellt wurde. Derzeit können Sie diese Daten nicht nutzen, um eine genaue Buchhaltung über die Nutzung öffentlicher IP-Adressen durchzuführen.

## <a name="build-201709283"></a>Build 20170928.3

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="powershell"></a>PowerShell
- Das Release des PowerShell-Moduls AzureRM 1.2.11 enthält eine Liste mit wichtigen Änderungen. Informationen zu einem Upgrade aus Version 1.2.10 finden Sie im [Migrationshandbuch](https://aka.ms/azspowershellmigration).

#### <a name="deployment"></a>Bereitstellung
- Sie müssen während der Bereitstellung einen Zeitserver mithilfe einer IP-Adresse angeben.

 #### <a name="infrastructure-management"></a>Infrastrukturverwaltung
- Belassen Sie die Infrastruktursicherung auf dem Blatt **Infrastructure backup** (Infrastruktursicherung) im deaktivierten Zustand.
- Der Computeressourcenanbieter zeigt einen unbekannten Status an.
- Die IP-Adresse und das Modell des Baseboard-Verwaltungscontrollers (Baseboard Management Controller, BMC) werden nicht in den Hauptinformationen eines Skalierungseinheitknotens angezeigt. Dies ist das erwartete Verhalten für das Azure Stack Development Kit. 
   
#### <a name="portal"></a>Portal
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
- Ihnen wird die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.
- Klicken Sie in den Details der Warnung **Aktivierung erforderlich** nicht auf den Link zu der Komponente **AzureBridge**. Wenn Sie dies tun, wird versucht, das Blatt **Übersicht** zu laden. Dies ist jedoch nicht erfolgreich, und es erfolgt kein Timeout.
- Kontingente werden möglicherweise nicht im Administratorportal angezeigt, nachdem Sie diese erstellt haben und später versuchen, Plandetails anzuzeigen. Eine Problemumgehung besteht darin, unter **Dienste und Kontingente** auf **Hinzufügen** zu klicken und einen neuen Eintrag hinzufügen.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
  
#### <a name="marketplace"></a>Marketplace
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
 
#### <a name="compute"></a>Compute
- Benutzer haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen. Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt.
- Sie können eine VM-Verfügbarkeitsgruppe nur mit einer Fehlerdomäne und einer Updatedomäne konfigurieren.
- Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden. Sie können eine Skalierungsgruppe erstellen, indem Sie eine Vorlage verwenden.

#### <a name="networking"></a>Netzwerk
- Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen. Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
- Beim Erstellen eines Netzwerklastenausgleichs müssen Sie eine NAT-Regel (Network Address Translation, Netzwerkadressübersetzung) erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
- Wenn Sie unter **Netzwerk** auf **Verbindung** klicken, um eine VPN-Verbindung einzurichten, wird **VNET-zu-VNET** als möglicher Verbindungstyp aufgeführt. Wählen Sie diese Option nicht aus. Derzeit wird nur die Option **Standort-zu-Standort (IPsec)** unterstützt.


#### <a name="sqlmysql"></a>SQL/MySQL
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

#### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
