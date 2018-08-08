---
title: Azure-Speicherdienstverschlüsselung für ruhende Daten | Microsoft-Dokumentation
description: Verwenden Sie Azure Storage Service Encryption, um Azure Blob Storage auf der Dienstseite beim Speichern der Daten zu verschlüsseln und beim Abrufen der Daten zu entschlüsseln.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 08/01/2018
ms.author: lakasa
ms.openlocfilehash: f35697139a4be49be8a645cfd4d451ad8e3c8094
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412354"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure-Speicherdienstverschlüsselung für ruhende Daten
Azure-Speicherdienstverschlüsselung für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceanforderungen Ihrer Organisation zu erfüllen. Mit dieser Funktion verschlüsselt die Azure-Speicherplattform Ihre Daten automatisch vor dem Ablegen in Azure Blob Storage, Azure Files oder Azure Queue Storage und entschlüsselt sie vor dem Abrufen. Die Verarbeitung der Ver- und Entschlüsselung, der Verschlüsselung ruhender Daten und der Schlüsselverwaltung in Storage Service Encryption ist für Benutzer transparent. Sämtliche in die Azure-Speicherplattform geschriebene Daten werden mittels 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockchiffreverfahren.

Storage Service Encryption wird für alle neuen und vorhandenen Speicherkonten aktiviert und kann nicht deaktiviert werden. Da Ihre Daten standardmäßig geschützt werden, müssen Sie weder Code noch Anwendungen ändern, um Storage Service Encryption nutzen zu können.

Die Funktion verschlüsselt automatisch Daten in:

- Azure Blob Storage, Azure Files, Azure Queue Storage, Azure Table Storage.  
- beiden Leistungsstufen (Standard und Premium).
- beiden Bereitstellungsmodellen (Azure Resource Manager und klassisch).

> [!Note]  
> Die Speicherdienstverschlüsselung ist für [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) nicht verfügbar. Die Verschlüsselung auf Betriebssystemebene wird empfohlen, z. B. [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md), die den Industriestandard [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) unter Windows und [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) unter Linux verwendet, um eine in Key Vault integrierte Verschlüsselung bereitzustellen.

Die Speicherdienstverschlüsselung hat keine Auswirkungen auf die Leistung der Azure-Speicherdienste.

Mit Storage Service Encryption können von Microsoft verwaltete oder eigene Verschlüsselungsschlüssel verwendet werden. Weitere Informationen zur Verwendung eigener Schlüssel finden Sie unter [Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Anzeigen von Verschlüsselungseinstellungen im Azure-Portal
Melden Sie sich zum Anzeigen der Einstellungen von Storage Service Encryption beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie ein Speicherkonto aus. Wählen Sie im Bereich **EINSTELLUNGEN** die Einstellung **Verschlüsselung** aus.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Häufig gestellte Fragen zu Azure Storage Service Encryption
**Wie verschlüssle ich die Daten in einem Resource Manager-Speicherkonto?**  
Die Speicherdienstverschlüsselung ist für alle vorhandenen Speicherkonten aktiviert (klassisch und Resource Manager). Alle bereits im Speicherkonto vorhandenen Dateien, die vor der Aktivierung der Verschlüsselung erstellt wurden, werden nachträglich durch einen im Hintergrund ausgeführten Verschlüsselungsprozess verschlüsselt.

**Ist die Speicherdienstverschlüsselung beim Erstellen eines Speicherkontos standardmäßig aktiviert?**  
Ja, die Speicherdienstverschlüsselung ist für alle Speicherkonten und alle Azure-Speicherdienste aktiviert.

**Ich besitze ein Resource Manager-Speicherkonto. Kann ich Storage Service Encryption dafür aktivieren?**  
Die Speicherdienstverschlüsselung ist standardmäßig für alle vorhandenen Resource Manager-Speicherkonten aktiviert. Die Funktion wird für Azure Blob Storage, Azure Files, Azure Queue Storage und Azure Table Storage unterstützt. 

**Kann ich die Verschlüsselung für mein Speicherkonto deaktivieren?**  
Die Verschlüsselung ist standardmäßig aktiviert. Es gibt keine Möglichkeit, die Verschlüsselung für Ihr Speicherkonto zu deaktivieren. 

**Wie hoch sind die Mehrkosten für Azure Storage, wenn die Speicherdienstverschlüsselung aktiviert ist?**  
Es fallen keine zusätzlichen Kosten an.

**Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?**  
Ja. Sie können Ihre eigenen Verschlüsselungsschlüssel verwenden. Weitere Informationen finden Sie unter [Azure Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**  
Ja, vorausgesetzt, Sie [verwenden Ihre eigenen Verschlüsselungsschlüssel](storage-service-encryption-customer-managed-keys.md) in Azure Key Vault.

**Ist die Speicherdienstverschlüsselung für Azure Managed Disks verfügbar?**  
Nein. Die Speicherdienstverschlüsselung ist für [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) nicht verfügbar. Die Verschlüsselung auf Betriebssystemebene wird empfohlen, z. B. [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md), die den Industriestandard [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) unter Windows und [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) unter Linux verwendet, um eine in Key Vault integrierte Verschlüsselung bereitzustellen.

**Wie unterscheidet sich die Speicherdienstverschlüsselung von Azure Disk Encryption?**  
Azure Disk Encryption bietet eine Integration zwischen betriebssystembasierten Lösungen, z. B. BitLocker und DM-Crypt, und Azure Key Vault. Die Speicherdienstverschlüsselung ermöglicht die systemeigene Verschlüsselung auf der Azure-Speicherplattform, unter der VM.

**Ich besitze ein klassisches Speicherkonto. Kann ich Storage Service Encryption dafür aktivieren?**  
Die Speicherdienstverschlüsselung ist für alle Speicherkonten (klassisch und Resource Manager) aktiviert.

**Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**  
Aufgrund der standardmäßigen Aktivierung werden in den Azure-Speicherdiensten gespeicherte Daten automatisch verschlüsselt. 

**Kann ich mit Azure PowerShell und der Azure CLI Speicherkonten mit aktivierter Speicherdienstverschlüsselung erstellen?**  
Die Speicherdienstverschlüsselung ist beim Erstellen von Speicherkonten (klassisch und Resource Manager) standardmäßig aktiviert. Sie können Kontoeigenschaften mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle überprüfen.

**Mein Speicherkonto ist so eingerichtet, dass es georedundant repliziert wird. Werden mit Storage Service Encryption auch meine redundanten Kopien verschlüsselt?**  
Ja, alle Kopien des Speicherkontos werden verschlüsselt. Alle Redundanzoptionen werden unterstützt: lokal redundanter Speicher, zonenredundanter Speicher, georedundanter Speicher und georedundanter Speicher mit Lesezugriff.

**Ist die Speicherdienstverschlüsselung nur in bestimmten Regionen zugelassen?**  
Die Speicherdienstverschlüsselung ist in allen Regionen verfügbar.

**Ist die Speicherdienstverschlüsselung mit FIPS 140-2 konform?**  
Ja, die Speicherdienstverschlüsselung ist mit FIPS 140-2 konform.

**An wen wende ich mich, wenn Probleme auftreten, oder wenn ich Feedback geben möchte?**  
Wenden Sie sich bei Problemen oder Feedback in Bezug auf die Speicherdienstverschlüsselung an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet umfassende Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage-security-guide.md).