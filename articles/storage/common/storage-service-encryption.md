---
title: Azure Storage Service Encryption für ruhende Daten | Microsoft Docs
description: Verwenden Sie Azure Storage Service Encryption, um Azure Blob Storage auf der Dienstseite beim Speichern der Daten zu verschlüsseln und beim Abrufen der Daten zu entschlüsseln.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: lakasa
ms.openlocfilehash: d469dfb5092f1269a6600ee8ee2f81778fd83b96
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449060"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Encryption für ruhende Daten

Azure Storage Service Encryption für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch vor dem Ablegen in Azure Storage und entschlüsselt sie vor dem Abrufen. Die Verarbeitung der Ver- und Entschlüsselung, der Verschlüsselung ruhender Daten und der Schlüsselverwaltung in Storage Service Encryption ist für Benutzer transparent. Sämtliche in Azure Storage geschriebene Daten werden mittels 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

Storage Service Encryption wird für alle neuen und vorhandenen Speicherkonten aktiviert und kann nicht deaktiviert werden. Da Ihre Daten standardmäßig geschützt werden, müssen Sie weder Code noch Anwendungen ändern, um Storage Service Encryption nutzen zu können.

Die Funktion verschlüsselt automatisch Daten in:

- beiden Leistungsstufen (Standard und Premium).
- beiden Bereitstellungsmodellen (Azure Resource Manager und klassisch).
- allen Azure Storage-Diensten (Blob Storage, Queue Storage, Table Storage und Azure Files). 

Storage Service Encryption hat keine Auswirkungen auf die Leistung von Azure Storage.

Mit Storage Service Encryption können von Microsoft verwaltete oder eigene Verschlüsselungsschlüssel verwendet werden. Weitere Informationen zur Verwendung eigener Schlüssel finden Sie unter [Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Anzeigen von Verschlüsselungseinstellungen im Azure-Portal

Melden Sie sich zum Anzeigen der Einstellungen von Storage Service Encryption beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie ein Speicherkonto aus. Wählen Sie im Bereich **EINSTELLUNGEN** die Einstellung **Verschlüsselung** aus.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Häufig gestellte Fragen zu Azure Storage Service Encryption

**F: Ich besitze ein klassisches Speicherkonto. Kann ich Storage Service Encryption dafür aktivieren?**

A: Storage Service Encryption ist für alle Speicherkonten (klassisch und Resource Manager) aktiviert.

**F: Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**

A: Da die Verschlüsselung standardmäßig aktiviert ist, verschlüsselt Azure Storage Ihre neuen Daten automatisch. 

**F: Ich besitze ein Resource Manager-Speicherkonto. Kann ich Storage Service Encryption dafür aktivieren?**

A: Storage Service Encryption ist standardmäßig für alle vorhandenen Resource Manager-Speicherkonten aktiviert. Die Funktion wird für Blob Storage, Table Storage, Queue Storage und Azure Files unterstützt. 

**F: Wie verschlüssle ich die Daten in einem Resource Manager-Speicherkonto?**

A: Storage Service Encryption ist für alle vorhandenen Speicherkonten aktiviert – klassisch und Resource Manager. Alle bereits im Speicherkonto vorhandenen Dateien, die vor der Aktivierung der Verschlüsselung erstellt wurden, werden nachträglich durch einen im Hintergrund ausgeführten Verschlüsselungsprozess verschlüsselt.

**F: Kann ich mithilfe von Azure PowerShell und der Azure CLI Speicherkonten erstellen, für die Storage Service Encryption aktiviert ist?**

A: Storage Service Encryption wird beim Erstellen von Speicherkonten (klassisch und Resource Manager) standardmäßig aktiviert. Sie können Kontoeigenschaften mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle überprüfen.

**F: Wie hoch sind die Mehrkosten für Azure Storage, wenn Storage Service Encryption aktiviert ist?**

A: Es fallen keine zusätzlichen Kosten an.

**F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?**

A: Ja. Sie können Ihre eigenen Verschlüsselungsschlüssel verwenden. Weitere Informationen finden Sie unter [Azure Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Ja – vorausgesetzt, Sie [verwenden Ihre eigenen Verschlüsselungsschlüssel](storage-service-encryption-customer-managed-keys.md) in Azure Key Vault.

**F: Ist Storage Service Encryption beim Erstellen eines Speicherkontos standardmäßig aktiviert?**

A: Ja, die Speicherdienstverschlüsselung ist für alle Speicherkonten und Azure Storage-Dienste aktiviert.

**F: Wo liegt der Unterschied zu Azure Disk Encryption?**

A: Azure Disk Encryption wird zum Verschlüsseln von Betriebssystemdatenträgern und sonstigen Datenträgern in virtuellen IaaS-Computern verwendet. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage-security-guide.md).

**F: Was passiert, wenn ich Azure Disk Encryption für meine Datenträger aktiviere?**

A: Dies funktioniert nahtlos. Ihre Daten werden mit beiden Methoden verschlüsselt.

**F: Mein Speicherkonto ist so eingerichtet, dass es georedundant repliziert wird. Werden mit Storage Service Encryption auch meine redundanten Kopien verschlüsselt?**

A: Ja, alle Kopien des Speicherkontos werden verschlüsselt. Alle Redundanzoptionen werden unterstützt: lokal redundanter Speicher, zonenredundanter Speicher, georedundanter Speicher und georedundanter Speicher mit Lesezugriff.

**F: Kann ich die Verschlüsselung für mein Speicherkonto deaktivieren?**

A: Die Verschlüsselung ist standardmäßig aktiviert, und es gibt keine Möglichkeit, die Verschlüsselung für Ihr Speicherkonto zu deaktivieren. 

**F: Ist Storage Service Encryption nur in bestimmten Regionen zugelassen?**

A: Storage Service Encryption ist in allen Regionen für alle Dienste verfügbar.

**F: Ist die Speicherdienstverschlüsselung mit FIPS 140-2 konform?**

A: Ja, die Speicherdienstverschlüsselung ist mit FIPS 140-2 konform.

**F: Wohin wende ich mich, wenn Probleme auftreten oder wenn ich Feedback abgeben möchte?**

A: Wenden Sie sich bei Problemen oder Feedback in Bezug auf Storage Service Encryption an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet umfassende Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage-security-guide.md).
