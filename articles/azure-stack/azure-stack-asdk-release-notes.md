---
title: "Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation"
description: 
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
ms.date: 10/10/2017
ms.author: helaw
ms.openlocfilehash: b3f8768b612f476485cb45cfc47b8b7ab0f00e96
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit

*Gilt für: Azure Stack Development Kit*

Diese Versionshinweise enthalten Informationen zu neuen Features und bekannten Problemen in Azure Stack Development Kit.  Wenn Sie nicht sicher sind, welche Version ausgeführt wird, können Sie diese im [Portal überprüfen](azure-stack-updates.md#determine-the-current-version).

## <a name="release-build-201709283"></a>Versionsbuild 20170928.3

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="powershell"></a>PowerShell

Azure PowerShell 1.2.11 ist noch nicht verfügbar, aber für ein Release Ende dieser Woche vorgesehen. Bis zu diesem Zeitpunkt können folgende Probleme auftreten:
* Die Ressourcenanbieter für App Service, SQL-Adapter und MySQL-Adapter hängen von PowerShell 1.2.11 ab. Aus diesem Grund wird ihre Veröffentlichung verzögert, bis die neue Version von Azure PowerShell verfügbar ist.
* Azure PowerShell 1.2.11 wird in der Dokumentation verwendet, obwohl diese Version noch nicht verfügbar ist.
* AzureStack-Tools verweist noch auf Azure PowerShell 1.2.10, bis 1.2.11 verfügbar ist.
* Azure PS 1.2.10 kann weiterhin mit dem Development Kit für viele Vorgänge verwendet werden. Die Version muss ersetzt werden, wenn Azure PS 1.2.11 verfügbar ist.



#### <a name="deployment"></a>Bereitstellung
* Sie müssen während der Bereitstellung einen Zeitserver mithilfe einer IP-Adresse angeben.  

#### <a name="portal"></a>Portal
* Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird.  Sie können das Dashboard wiederherstellen, indem Sie das Zahnrad oben rechts im Portal und dann die Option „Standardeinstellungen wiederherstellen“ wählen.
* Mandanten können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt.  Diese Elemente sind für Mandanten nicht funktional.
* Die Schaltfläche „Verschieben“ ist unter „Ressourcengruppe“ deaktiviert.  Das Deaktivieren der Schaltfläche ist das erwartete Verhalten, da das Verschieben von Ressourcengruppen zwischen Abonnements derzeit nicht unterstützt wird.
* Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen.  Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
* In einer Warnung wird darauf hingewiesen, dass Sie Ihr Azure Stack Development Kit registrieren müssen.  Bei dieser Warnung handelt es sich um erwartetes Verhalten.  
  

#### <a name="services"></a>Dienste
* Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden, aber sie können per Vorlage erstellt werden.
* Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen.  Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
* VM-Verfügbarkeitsgruppen können nur mit einer Fehlerdomäne und einer Updatedomäne konfiguriert werden.  
* Ein Mandant muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
* Das Löschen von Mandantenabonnements führt zu verwaisten Ressourcen.  Eine Problemumgehung besteht darin, zuerst Mandantenressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Mandantenabonnements zu löschen. 
* Sie müssen bei der Erstellung eines Netzwerklastenausgleichs eine NAT-Regel erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
* Mandanten haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen.  Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt.
* Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
* Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.
* Das Blatt „Infrastructure backup“ (Infrastruktursicherung) darf nicht verwendet werden.


#### <a name="fabric"></a>Fabric
* Der Compute-Ressourcenanbieter zeigt einen unbekannten Status an.
* Die IP-Adresse und das Modell für BMC werden nicht in den Hauptinformationen eines Skalierungseinheitknotens angezeigt.  Dies ist das erwartete Verhalten für das Azure Stack Development Kit.

