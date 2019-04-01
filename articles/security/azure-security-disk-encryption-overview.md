---
title: Übersicht – Azure Disk Encryption für virtuelle IaaS-Computer | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über Microsoft Azure Disk Encryption für virtuelle IaaS-Computer.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/16/2019
ms.custom: seodec18
ms.openlocfilehash: 66d788aec83e3e57a49b063f2ca80484360f639d
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295282"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption für virtuelle IaaS-Computer

Microsoft Azure setzt alles daran, den Schutz Ihrer Daten sowie die Datenhoheit sicherzustellen. Mit Azure können Sie Ihre in Azure gehosteten Daten mit einer Reihe von modernen Technologien zum Verschlüsseln, Steuern und Verwalten von Verschlüsselungsschlüsseln sowie zum Steuern und Überwachen des Datenzugriffs kontrollieren. So können Azure-Kunden flexibel eine Lösung auswählen, die ihre Anforderungen am besten erfüllt. In diesem Artikel wird Ihnen eine Technologielösung vorgestellt: „Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer“. Diese Technologie unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Übersicht

Azure Disk Encryption ist eine Funktion, mit der Sie die Datenträger von Windows- und Linux-IaaS-VMs verschlüsseln können. Disk Encryption nutzt das Branchenstandardfeature [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, um Ihnen die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern. Diese Lösung stellt außerdem sicher, dass alle ruhenden Daten auf den VM-Datenträgern in Azure Storage verschlüsselt sind.

Disk Encryption für Windows- und Linux-IaaS-VMs befindet sich in allen öffentlichen Azure-Regionen und Azure Government-Regionen für Standard-VMs und VMs mit Azure Storage Premium in der Phase „Allgemeine Verfügbarkeit“. Mit der Disk Encryption-Verwaltungslösung können Sie die folgenden geschäftlichen Anforderungen erfüllen:

* IaaS-VMs werden im Ruhezustand geschützt, indem Verschlüsselungstechnologie nach Branchenstandard eingesetzt wird, um die Anforderungen an Unternehmenssicherheit und Compliance zu erfüllen.
* IaaS-VMs werden mit Schlüsseln und Richtlinien gestartet, die vom Kunden gesteuert werden. Sie können deren Nutzung in Ihrem Schlüsseltresor überwachen.

Falls Sie Azure Security Center verwenden, werden Sie gewarnt, wenn VMs nicht verschlüsselt sind. Die Warnungen werden als „Hoher Schweregrad“ angezeigt. Empfohlen wird in diesem Fall die Verschlüsselung der VMs.

![Azure Security Center: Datenträgerverschlüsselungswarnung](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung und ziehen zusätzliche Lizenz- oder Abonnementkosten nach sich.


## <a name="encryption-scenarios"></a>Verschlüsselungsszenarien

Die Disk Encryption-Lösung unterstützt die folgenden Kundenszenarien:

* Aktivieren der Verschlüsselung auf neuen virtuellen Windows-IaaS-Computern, die mit einer vorverschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden
* Aktivieren der Verschlüsselung auf neuen IaaS-VMs, die über unterstützte Images aus dem Azure-Katalog erstellt werden
* Aktivieren der Verschlüsselung auf vorhandenen IaaS-VMs, die in Azure ausgeführt werden
* Aktivieren der Verschlüsselung für VM-Skalierungsgruppen unter Windows
* Aktivieren der Verschlüsselung auf Datenlaufwerken für VM-Skalierungsgruppen unter Linux
* Deaktivieren der Verschlüsselung auf Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs
* Deaktivieren der Verschlüsselung für VM-Skalierungsgruppen unter Windows
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für VM-Skalierungsgruppen unter Linux
* Aktivieren der Verschlüsselung von VMs mit verwalteten Datenträgern
* Aktualisieren von Verschlüsselungseinstellungen einer vorhandenen verschlüsselten VM (mit oder ohne Storage Premium)
* Sichern und Wiederherstellen verschlüsselter VMs

Die Lösung unterstützt die folgenden Szenarien für virtuelle IaaS-Computer, wenn sie in Microsoft Azure aktiviert sind:

* Integration in Azure Key Vault
* Virtuelle Computer im Standard-Tarif: [IaaS-VMs der Serie A, D, DS, F, G, GS usw.](https://azure.microsoft.com/pricing/details/virtual-machines/) [Linux-VMs](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) in diesen Tarifen müssen die Arbeitsspeicher-Mindestanforderungen von 7 GB erfüllen.
* Aktivieren der Verschlüsselung auf IaaS-VMs unter Windows und Linux, auf VMs mit verwalteten Datenträgern und auf Skalierungsgruppen-VMs aus den unterstützten Azure-Katalogimages
* Deaktivieren der Verschlüsselung für Betriebssystem- und Datenlaufwerke für Windows-IaaS-VMs, Skalierungsgruppen-VMs und VMs mit verwalteten Datenträgern
* Deaktivieren der Verschlüsselung für Datenlaufwerke für Linux-IaaS-VMs, Skalierungsgruppen-VMs und VMs mit verwalteten Datenträgern
* Aktivieren der Verschlüsselung auf IaaS-VMs mit dem Windows-Clientbetriebssystem
* Aktivieren der Verschlüsselung auf Volumes mit Einbindungspfaden
* Aktivieren der Verschlüsselung auf Linux-VMs, die mithilfe von „mdadm“ mit Datenträgerstriping (RAID) konfiguriert sind
* Aktivieren der Verschlüsselung auf Linux-VMs, die LVM für Datenträger verwenden
* Aktivieren der Verschlüsselung auf Linux-VM-Datenträgern für Betriebssystem und Daten

   > [!NOTE]
   > Die Verschlüsselung von Betriebssystemlaufwerken wird für einige Linux-Distributionen nicht unterstützt. Weitere Informationen finden Sie im Artikel [Häufig gestellte Fragen zu Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
   
* Aktivieren der Verschlüsselung auf VMs, die mit Windows-Speicherplätzen ab Windows Server 2016 konfiguriert sind
* Aktualisieren von Verschlüsselungseinstellungen einer vorhandenen verschlüsselten VM (mit oder ohne Storage Premium)
* Sichern und Wiederherstellen verschlüsselter VMs in Szenarien mit und ohne Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK)
* Alle öffentlichen Azure-Regionen und Azure Government-Regionen werden unterstützt.

Folgende Szenarien, Features und Technologien werden von der Lösung nicht unterstützt:

* IaaS-VMs des Basic-Tarifs
* Deaktivieren der Verschlüsselung auf einem Betriebssystemlaufwerk für Linux-IaaS-VMs
* Deaktivieren der Verschlüsselung auf einem Datenlaufwerk, wenn das Betriebssystemlaufwerk für Linux-IaaS-VMs verschlüsselt ist.
* Verschlüsselung von Betriebssystemlaufwerken für VM-Skalierungsgruppen unter Linux
* IaaS-VMs, die mithilfe der klassischen Methode zum Erstellen von VMs erstellt werden
* Aktivieren der Verschlüsselung für benutzerdefinierte Images von Kunden auf Linux-IaaS-VMs
* Integration in das lokale Schlüsselverwaltungssystem
* Azure Files (freigegebenes Dateisystem)
* Network File System (NFS)
* Dynamische Volumes
* Windows-VMs, die mit softwarebasierten RAID-Systemen konfiguriert sind

## <a name="encryption-features"></a>Verschlüsselungsfunktionen

Beim Aktivieren und Bereitstellen von Disk Encryption für Azure-IaaS-VMs werden je nach bereitgestellter Konfiguration die folgenden Funktionen aktiviert:

* Verschlüsselung des Betriebssystemvolumes zum Schützen des Startvolumes im Ruhezustand in Ihrem Speicher
* Verschlüsselung der Datenvolumes zum Schützen der Datenvolumes im Ruhezustand in Ihrem Speicher
* Deaktivieren der Verschlüsselung auf den Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf den Datenlaufwerken für Linux-IaaS-VMs (nur wenn das Betriebssystemlaufwerk nicht verschlüsselt ist)
* Schutz der Verschlüsselungsschlüssel und Geheimnisse in Ihrem Azure Key Vault-Abonnement
* Meldung des Verschlüsselungsstatus der verschlüsselten IaaS-VM
* Entfernen der Konfigurationseinstellungen für die Datenträgerverschlüsselung von der IaaS-VM
* Sichern und Wiederherstellen verschlüsselter VMs mit dem Azure Backup-Dienst

Azure Disk Encryption für IaaS-VMs für die Windows- und Linux-Lösung beinhaltet:

* Die Disk Encryption-Erweiterung für Windows.
* Die Disk Encryption-Erweiterung für Linux.
* Die PowerShell-Cmdlets für die Datenträgerverschlüsselung.
* Die Azure CLI-Cmdlets für die Datenträgerverschlüsselung.
* Die Azure Resource Manager-Vorlagen für die Datenträgerverschlüsselung.

Die Azure Disk Encryption-Lösung wird auf IaaS-VMs unter dem Windows- oder Linux-Betriebssystem unterstützt. Weitere Informationen zu den unterstützten Betriebssystemen finden Sie im Artikel [Voraussetzungen](azure-security-disk-encryption-prerequisites.md).

> [!NOTE]
> Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine zusätzlichen Gebühren an. Für den Schlüsseltresor, in dem die Verschlüsselungsschlüssel gespeichert werden, gelten die unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/) aufgeführten Standardpreise. 


## <a name="encryption-workflow"></a>Verschlüsselungsworkflow

Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Wählen Sie ein Verschlüsselungsszenario aus den im Abschnitt [Verschlüsselungsszenarien](#encryption-scenarios) aufgeführten Szenarien aus.

1. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten VMs und für die vorhandenen, bereits in Azure ausgeführten VMs an, um die Verschlüsselung auf der IaaS-VM zu aktivieren.

1. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf der IaaS-VM zu aktivieren.

1. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.

   ![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Entschlüsselungsworkflow
Um die Datenträgerverschlüsselung für virtuelle IaaS-Computer zu deaktivieren, führen Sie die folgenden allgemeinen Schritte aus:

1. Deaktivieren Sie die Verschlüsselung (Entschlüsselung) auf einer in Azure ausgeführten IaaS-VM, und geben Sie die Entschlüsselungskonfiguration an. Sie können die Deaktivierung über die Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI vornehmen.

   Dieser Schritt deaktiviert die Verschlüsselung des Betriebssystemdatenträgers und/oder Datenträgers für Daten in der ausgeführten Windows-IaaS-VM. Wie im vorherigen Abschnitt erwähnt, wird die Deaktivierung der Verschlüsselung des Betriebssystemdatenträgers für Linux nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf virtuellen Linux-Computern zulässig, wenn der Betriebssystemdatenträger nicht verschlüsselt ist.

1. Azure aktualisiert das VM-Dienstmodell, und die IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt.

   > [!NOTE]
   > Durch den Vorgang zum Deaktivieren der Verschlüsselung werden Ihr Schlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme oder Passphrase für Linux) nicht gelöscht.
   >
   > Das Deaktivieren der Betriebssystem-Datenträgerverschlüsselung für Linux wird nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf Linux-VMs zulässig.
   >
   > Das Deaktivieren der Datenträgerverschlüsselung für Linux wird nicht unterstützt, wenn das Betriebssystemlaufwerk verschlüsselt ist.


## <a name="encryption-workflow-previous-release"></a>Verschlüsselungsworkflow (Vorgängerversion)

Beim neuen Release von Azure Disk Encryption muss kein Azure Active Directory-Anwendungsparameter (Azure AD) mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Bei der neuen Version müssen Sie während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen ohne die Azure AD-Anwendungsparameter verschlüsselt werden, wenn Sie die neue Version verwenden. VMs, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der Azure AD-Syntax gepflegt werden. Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer (Vorgängerversion) zu aktivieren:

1. Wählen Sie ein Verschlüsselungsszenario aus den im Abschnitt [Verschlüsselungsszenarien](#encryption-scenarios) aufgeführten Szenarien aus.

1. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten VMs und für die vorhandenen, bereits in Azure ausgeführten VMs an, um die Verschlüsselung auf der IaaS-VM zu aktivieren.

1. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf der IaaS-VM zu aktivieren.

1. Geben Sie die Identität der Azure AD-Anwendung an, um das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor zu schreiben. Mit diesem Schritt wird die Verschlüsselung auf der IaaS-VM für die in Schritt 2 erwähnten Szenarien aktiviert.

1. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.


## <a name="terminology"></a>Begriff
In der folgende Tabelle werden einige gängige Begriffe definiert, die in dieser Technologie verwendet werden:

| Begriff | Definition |
| --- | --- |
| Azure AD | Ein [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)-Konto wird zum Authentifizieren, Speichern und Abrufen von Geheimnissen aus einem Schlüsseltresor verwendet. |
| Azure Key Vault | Key Vault ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung (Federal Information Processing Standards) basiert. Diese Standards tragen dazu bei, Ihre kryptografischen Schlüssel und vertraulichen Geheimnisse zu schützen. Weitere Informationen finden Sie in der [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| BitLocker |Bei [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) handelt es sich um eine branchenübliche Verschlüsselungstechnologie für Windows-Volumes, um die Datenträgerverschlüsselung auf virtuellen Windows-IaaS-Computern zu aktivieren. |
| BEK | BitLocker-Verschlüsselungsschlüssel (BitLocker Encryption Keys, BEK) werden verwendet, um das Startvolume des Betriebssystems und Datenvolumes zu verschlüsseln. BEKs sind in einem Schlüsseltresor als Geheimnisse geschützt. |
| Azure-Befehlszeilenschnittstelle | [Azure CLI](/cli/azure/install-azure-cli) ist für die Verwaltung von Azure-Ressourcen über die Befehlszeile optimiert.|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-IaaS-Computern verwendet wird. |
| KEK | Der Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) ist der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen des Geheimnisses verwendet wird. Sie können einen mit Hardwaresicherheitsmodulen geschützten Schlüssel oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| PowerShell-Cmdlets | Weitere Informationen finden Sie unter [Azure PowerShell-Cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Azure Disk Encryption – Voraussetzungen](azure-security-disk-encryption-prerequisites.md)
