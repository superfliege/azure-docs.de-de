---
title: Sicherheitsfunktionen, die mit Azure Storage verwendet werden können | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die wichtigsten Sicherheitsfunktionen von Azure, die mit Azure Storage verwendet werden können.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 9558f1ec0d8ccd83da764a0967fa83d93e1e6a02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365368"
---
# <a name="azure-storage-security-overview"></a>Übersicht über die Sicherheit von Azure Storage
Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit benötigen, um die Anforderungen ihrer Kunden zu erfüllen. Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen. Ihre Möglichkeiten:

* Schützen Sie das Speicherkonto mit rollenbasierter Zugriffssteuerung (RBAC) und Azure Active Directory.
* Schützen Sie Daten während der Übertragung zwischen einer Anwendung und Azure mit clientseitiger Verschlüsselung, HTTPS oder SMB 3.0.
* Legen Sie fest, dass Daten automatisch mit der Speicherdienstverschlüsselung verschlüsselt werden, wenn sie in Azure Storage geschrieben werden.
* Legen Sie fest, dass Betriebssystemdatenträger und sonstige von virtuellen Computern (VMs) verwendete Datenträger mit Azure Disk Encryption verschlüsselt werden.
* Erteilen Sie delegierten Zugriff auf die Datenobjekte in Azure Storage mit Shared Access Signatures (SAS).
* Verwenden Sie Analysen zum Verfolgen der Authentifizierungsmethode, die ein Benutzer beim Zugriff auf Storage verwendet.

Eine ausführlichere Betrachtung der Sicherheit in Azure Storage finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage/common/storage-security-guide.md). Dieser Leitfaden enthält fundierte Informationen zu den Sicherheitsfeatures von Azure Storage. Zu diesen Features gehören Speicherkontoschlüssel, Datenverschlüsselung während der Übertragung und Speicherung sowie Speicheranalysen.

Dieser Artikel bietet eine Übersicht über die Sicherheitsfunktionen von Azure, die mit Azure Storage verwendet werden können. Links zu Artikeln bieten weitere Informationen zu den einzelnen Features.

## <a name="role-based-access-control"></a>Rollenbasierte Access Control
Sie können Ihr Speicherkonto mit rollenbasierter Zugriffssteuerung (RBAC) sichern. Das Einschränken des Zugriffs auf der Grundlage der Sicherheitsprinzipien [Need to know](https://en.wikipedia.org/wiki/Need_to_know) und [Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ist für Organisationen unerlässlich, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. Diese Zugriffsrechte werden gewährt, indem Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zugewiesen wird. Sie können [integrierte RBAC-Rollen](../role-based-access-control/built-in-roles.md)(etwa „Speicherkontomitwirkender“) verwenden, um Benutzern Berechtigungen zuzuweisen.

Weitere Informationen:

* [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Delegierter Zugriff auf Speicherobjekte
Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Eine SAS bietet die Möglichkeit, einem Client für einen bestimmten Zeitraum spezielle eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu erteilen. Dazu müssen Sie nicht Ihre Kontozugriffsschlüssel freigeben. 

Die SAS ist ein URI, dessen Abfrageparameter alle erforderlichen Informationen für den authentifizierten Zugriff auf eine Speicherressource enthalten. Für den Zugriff auf Speicherressourcen mit der SAS braucht der Client diese nur an den entsprechenden Konstruktor bzw. die entsprechende Methode zu übergeben.

Weitere Informationen:

* [Grundlagen zum SAS-Modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Erstellen und Verwenden einer SAS mit Blob Storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
Verschlüsselung während der Übertragung ist ein Mechanismus zum Schutz der Daten bei der Übertragung über Netzwerke hinweg. Mit Azure Storage können Sie Daten mit folgenden Features schützen:

* [Verschlüsselung auf Transportebene](../storage/common/storage-security-guide.md#encryption-in-transit) (etwa HTTPS), wenn Sie Daten in oder aus Azure Storage übertragen.
* [Wire-Verschlüsselung](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares) (etwa SMB 3.0-Verschlüsselung für Azure-Dateifreigaben).
* [Clientseitige Verschlüsselung](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), um die Daten vor ihrer Übertragung in Storage zu verschlüsseln und nach der Übertragung aus Storage zu entschlüsseln.

Weitere Informationen zur clientseitigen Verschlüsselung finden Sie hier:

* [Clientseitige Verschlüsselung für Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Cloud security controls series: Encrypting Data in Transit (Cloudsicherheit steuert Serie: Verschlüsseln von Daten bei der Übertragung)](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
Für viele Organisationen ist die Verschlüsselung [ruhender Daten](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Konformität und Datenhoheit. Drei Azure-Features ermöglichen die Verschlüsselung ruhender Daten:

* Mit der [Speicherdienstverschlüsselung](../storage/common/storage-security-guide.md#encryption-at-rest) können Sie anfordern, dass der Speicherdienst die Daten beim Schreiben in Azure Storage automatisch verschlüsselt.
* Die [clientseitige Verschlüsselung](../storage/common/storage-security-guide.md#client-side-encryption) ermöglicht ebenfalls eine Verschlüsselung ruhender Daten.
* Mit [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) können Sie die Betriebssystemdatenträger und andere Datenträger verschlüsseln, die von einem virtuellen IaaS-Computer verwendet werden.

Weitere Informationen zu Storage Service Encryption finden Sie hier:

* Die [Azure-Speicherdienstverschlüsselung](https://azure.microsoft.com/services/storage/) ist für [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) verfügbar. Ausführliche Informationen zu anderen Arten von Azure-Speicher finden Sie unter [Azure Files](https://azure.microsoft.com/services/storage/files/), [Datenträger (Storage Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [Tabellenspeicher](https://azure.microsoft.com/services/storage/tables/) und [Warteschlangenspeicher](https://azure.microsoft.com/services/storage/queues/).
* [Azure Storage Service Encryption für ruhende Daten](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Mit Azure Disk Encryption für virtuelle Computer können Sie die Sicherheits- und Konformitätsanforderungen Ihrer Organisation erfüllen. Dieses Feature verschlüsselt Ihre VM-Datenträger (einschließlich Start- und anderer Datenträger) mit Schlüsseln und Richtlinien, die Sie in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) steuern.

Datenträgerverschlüsselung für VMs funktioniert für Linux- und Windows-Betriebssysteme. Es wird auch Key Vault verwendet, um Ihnen das Schützen, Verwalten und Überwachen der Nutzung Ihrer Datenträgerverschlüsselungs-Schlüssel zu erleichtern. Alle Daten auf den Datenträgern Ihrer virtuellen Computer werden im Ruhezustand mithilfe von Verschlüsselungstechnologien des Industriestandards in Ihren Azure-Speicherkonten verschlüsselt. Die Disk Encryption-Lösung für Windows basiert auf der [Microsoft BitLocker-Laufwerkverschlüsselung](https://technet.microsoft.com/library/cc732774.aspx), die Linux-Lösung auf [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Weitere Informationen:

* [Azure Disk Encryption for Windows and Linux IaaS Virtual Machines (Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer)](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Disk Encryption verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Außerdem wird sichergestellt, dass alle ruhenden Daten auf den Datenträgern der virtuellen Computer in Azure Storage verschlüsselt sind. Es empfiehlt sich, die Verwendung von Schlüsseln und Richtlinien mithilfe des Azure-Schlüsseltresors zu überwachen.

Weitere Informationen:

* [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-whatis.md)
* [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md)
