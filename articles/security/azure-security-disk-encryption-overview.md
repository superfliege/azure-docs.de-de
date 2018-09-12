---
title: Azure Disk Encryption für virtuelle IaaS-Computer | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über Microsoft Azure Disk Encryption für virtuelle IaaS-Computer.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/10/2018
ms.openlocfilehash: ea72e4ac778e52d4f6e4f5597d38a1de59ba43fc
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348938"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption für virtuelle IaaS-Computer 
Bei Microsoft Azure wird sehr darauf geachtet, den Schutz Ihrer Daten und die Datenhoheit sicherzustellen. Außerdem können Sie für Ihre unter Azure gehosteten Daten eine Reihe von modernen Techniken zum Verschlüsseln, Steuern und Verwalten von Verschlüsselungsschlüsseln und Steuern und Überprüfen des Datenzugriffs nutzen. So können Azure-Kunden flexibel eine Lösung auswählen, die Ihre Anforderungen am besten erfüllt. In diesem Artikel lernen Sie die Technologie „Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer“ kennen, die zum Schützen und Absichern Ihrer Daten dient, um Vorgaben in den Bereichen Unternehmenssicherheit und Compliance zu erfüllen. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Übersicht
Azure Disk Encryption (ADE) ist eine Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. ADE verwendet das Branchen-Standardfeature [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, um Ihnen die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern. Diese Lösung stellt außerdem sicher, dass alle ruhenden Daten auf den Datenträgern der virtuellen Computer in Azure Storage verschlüsselt sind.

Die Azure-Datenträgerverschlüsselung (Azure Disk Encryption) für virtuelle Windows- und Linux-IaaS-Computer befindet sich in allen öffentlichen Azure-Regionen und AzureGov-Regionen für Standard-VMs und VMs mit Storage Premium in der Phase **Allgemeine Verfügbarkeit**. Mit der Azure Disk Encryption-Verwaltungslösung können Sie die folgenden geschäftlichen Anforderungen erfüllen:

* Virtuelle IaaS-Computer werden im Ruhezustand geschützt, indem Verschlüsselungstechnologie nach Branchenstandard eingesetzt wird, um die Anforderungen an Unternehmenssicherheit und Compliance zu erfüllen.
* Virtuelle IaaS-Computer werden mit vom Kunden gesteuerten Schlüsseln und Richtlinien gestartet, und Sie können ihre Nutzung in Ihrem Schlüsseltresor überwachen.


Bei Verwendung von Azure Security Center erhalten Sie eine Warnung, wenn nicht verschlüsselte virtuelle Computer vorhanden sind. Dies wird als Warnung mit hohem Schweregrad angezeigt. Empfohlen wird in diesem Fall die Verschlüsselung der virtuellen Computer.
![Azure Security Center: Datenträgerverschlüsselungswarnung](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung, was zusätzliche Lizenz- oder Abonnementkosten nach sich ziehen kann.


## <a name="encryption-scenarios"></a>Verschlüsselungsszenarien
Die Azure Disk Encryption-Lösung unterstützt die folgenden Kundenszenarien:

* Aktivieren der Verschlüsselung auf neuen virtuellen Windows-IaaS-Computern, die mit einer vorverschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden 
* Aktivieren der Verschlüsselung auf neuen IaaS-VMs, die über Images aus dem unterstützten Azure-Katalog erstellt werden
* Aktivieren der Verschlüsselung auf vorhandenen IaaS-VMs, die unter Azure ausgeführt werden
* Aktivieren der Verschlüsselung für VM-Skalierungsgruppen unter Windows
* Aktivieren der Verschlüsselung auf Datenlaufwerken für VM-Skalierungsgruppen unter Linux
* Deaktivieren der Verschlüsselung auf Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs
* Deaktivieren der Verschlüsselung für VM-Skalierungsgruppen unter Windows
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für VM-Skalierungsgruppen unter Linux
* Aktivieren der Verschlüsselung virtueller Computer auf verwalteten Datenträgern
* Aktualisieren von Verschlüsselungseinstellungen eines vorhandenen verschlüsselten virtuellen Computers (mit oder ohne Storage Premium)
* Sichern und Wiederherstellen verschlüsselter virtueller Computer

Die Lösung unterstützt die folgenden Szenarien für virtuelle IaaS-Computer, wenn sie in Microsoft Azure aktiviert sind:

* Integration in Azure Key Vault
* VMs im Tarif „Standard“: [IaaS-VMs der Reihe A, D, DS, F, G, GS usw.](https://azure.microsoft.com/pricing/details/virtual-machines/)
    * [Virtuelle Linux-Computer](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) in diesen Tarifen müssen die Arbeitsspeicher-Mindestanforderungen von 7 GB erfüllen.
* Aktivieren der Verschlüsselung auf virtuellen IaaS-Computern unter Windows und Linux, auf virtuellen Computern mit verwalteten Datenträgern und auf virtuellen Skalierungsgruppencomputern aus den unterstützten Azure-Katalogimages
* Deaktivieren der Verschlüsselung für Betriebssystem- und Datenlaufwerke für virtuelle Windows-IaaS-Computer, virtuelle Skalierungsgruppencomputer und virtuelle Computer mit verwalteten Datenträgern
* Deaktivieren der Verschlüsselung für Datenlaufwerke für virtuelle Linux-IaaS-Computer, virtuelle Skalierungsgruppencomputer und virtuelle Computer mit verwalteten Datenträgern
* Aktivieren der Verschlüsselung auf IaaS-VMs mit dem Windows-Clientbetriebssystem
* Aktivieren der Verschlüsselung auf Volumes mit Einbindungspfaden
* Aktivieren der Verschlüsselung auf Linux-VMs mit Datenträgerstriping (RAID) mithilfe von mdadm
* Aktivieren der Verschlüsselung auf Linux-VMs mit LVM für Datenträger
* Aktivieren der Verschlüsselung unter Linux-VM-Betriebssystemen und Datenträgern 
* Aktivieren der Verschlüsselung auf Windows-VMs mit Speicherplätzen
* Aktualisieren von Verschlüsselungseinstellungen eines vorhandenen verschlüsselten virtuellen Computers (mit oder ohne Storage Premium)
* Sichern und Wiederherstellen verschlüsselter virtueller Computer in Szenarien mit und ohne Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK)
* Alle öffentlichen Azure-Regionen und AzureGov-Regionen werden unterstützt.

Folgende Szenarien, Features und Technologien werden von der Lösung nicht unterstützt:

* IaaS-VMs des Basic-Tarifs
* Deaktivieren der Verschlüsselung auf Betriebssystemlaufwerken für virtuelle Linux-IaaS-Computer
* Deaktivieren der Verschlüsselung auf einem Datenlaufwerk, wenn das Betriebssystemlaufwerk für Linux-IaaS-VMs verschlüsselt ist
* Virtuelle IaaS-Computer, die mithilfe der klassischen Methode zum Erstellen von virtuellen Computern erstellt werden
* Aktivieren der Verschlüsselung für benutzerdefinierte Images auf virtuellen Linux-IaaS-Computern
* Integration in den lokalen Schlüsselverwaltungsdienst
* Azure Files (freigegebenes Dateisystem), Network File System (NFS), dynamische Volumes und virtuelle Windows-Computer, die mit softwarebasierten RAID-Systemen konfiguriert sind

## <a name="encryption-features"></a>Verschlüsselungsfunktionen
Beim Aktivieren und Bereitstellen von Azure Disk Encryption für virtuelle Azure-IaaS-Computer werden je nach bereitgestellter Konfiguration die folgenden Funktionen aktiviert:

* Verschlüsselung des Betriebssystemvolumes zum Schützen des Startvolumes im Ruhezustand in Ihrem Speicher
* Verschlüsselung des Datenvolumes zum Schützen des Datenvolumes im Ruhezustand in Ihrem Speicher
* Deaktivieren der Verschlüsselung auf den Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung für die Datenlaufwerke für virtuelle Linux-IaaS-Computer (nur wenn das Betriebssystemlaufwerk nicht verschlüsselt ist)
* Schutz der Verschlüsselungsschlüssel und Geheimnisse in Ihrem Key Vault-Abonnement
* Meldung des Verschlüsselungsstatus des verschlüsselten virtuellen IaaS-Computers
* Entfernen der Konfigurationseinstellungen für die Datenträgerverschlüsselung vom virtuellen IaaS-Computer
* Sichern und Wiederherstellen verschlüsselter virtueller Computer mit dem Azure Backup-Dienst

Azure Disk Encryption für IaaS-VMs für die Windows- und Linux-Lösung beinhaltet:

* Die Disk Encryption-Erweiterung für Windows.
* Die Disk Encryption-Erweiterung für Linux.
* Die PowerShell-Cmdlets zur Datenträgerverschlüsselung.
* Die Azure-Befehlszeilenschnittstellen-Cmdlets (Command Line Interface, CLI) zur Datenträgerverschlüsselung.
* Die Azure Resource Manager-Vorlagen für die Datenträgerverschlüsselung.

Die Azure Disk Encryption-Lösung wird auf virtuellen IaaS-Computern unterstützt, die unter dem Windows- oder Linux-Betriebssystem ausgeführt werden. Weitere Informationen zu den unterstützten Betriebssystemen finden Sie im Artikel [Voraussetzungen](azure-security-disk-encryption-prerequisites.md).

> [!NOTE]
> Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine zusätzlichen Gebühren an. Für den Schlüsseltresor, in dem die Verschlüsselungsschlüssel gespeichert werden, gelten die unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/) aufgeführten Standardpreise. 


## <a name="encryption-workflow"></a>Verschlüsselungsworkflow

 Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Wählen Sie aus den vorangehenden Szenarien für die Verschlüsselung ein Verschlüsselungsszenario aus.
2. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder einen CLI-Befehl, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten virtuellen Computer und für die vorhandenen, bereits in Azure ausgeführten virtuellen Computer an, um die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

3. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

4. Azure aktualisiert das VM-Dienstmodell mit Verschlüsselung und der Schlüsseltresorkonfiguration und richtet Ihren verschlüsselten virtuellen Computer ein.

 ![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Entschlüsselungsworkflow
Um die Datenträgerverschlüsselung für virtuelle IaaS-Computer zu deaktivieren, führen Sie die folgenden allgemeinen Schritte aus:

1. Deaktivieren Sie die Verschlüsselung (Entschlüsselung) auf einer in Azure ausgeführten IaaS-VM, und geben Sie die Entschlüsselungskonfiguration an. Sie können die Deaktivierung über die Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI vornehmen.

 Dieser Schritt deaktiviert die Verschlüsselung des Betriebssystemdatenträgers und/oder Datenträgers für Daten in der ausgeführten Windows-IaaS-VM. Wie jedoch im vorherigen Abschnitt erwähnt, wird die Deaktivierung der Verschlüsselung des Betriebssystemdatenträgers für Linux nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf virtuellen Linux-Computern zulässig, wenn der Betriebssystemdatenträger nicht verschlüsselt ist.
2. Azure aktualisiert das VM-Dienstmodell, und die IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt.

> [!NOTE]
> Durch den Vorgang zum Deaktivieren der Verschlüsselung werden Ihr Schlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme oder Passphrase für Linux) nicht gelöscht.
 > Das Deaktivieren der Betriebssystem-Datenträgerverschlüsselung für Linux wird nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf Linux-VMs zulässig.
Das Deaktivieren der Datenträgerverschlüsselung für Linux wird nicht unterstützt, wenn das Betriebssystemlaufwerk verschlüsselt ist.


## <a name="encryption-workflow-previous-release"></a>Verschlüsselungsworkflow (Vorgängerversion)

Bei der neuen Version der Azure-Datenträgerverschlüsselung muss kein Azure AD-Anwendungsparameter mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Bei der neuen Version müssen Sie während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen mit der neuen Version und ohne die Azure AD-Anwendungsparameter verschlüsselt werden. Virtuelle Computer, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der AAD-Syntax gepflegt werden. Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer (Vorgängerversion) zu aktivieren:

1. Wählen Sie aus den vorangehenden Szenarien für die Verschlüsselung ein Verschlüsselungsszenario aus.
2. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder einen CLI-Befehl, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten virtuellen Computer und für die vorhandenen, bereits in Azure ausgeführten virtuellen Computer an, um die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

3. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

4. Geben Sie die Identität der Azure Active Directory-Anwendung (Azure AD) an, um das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor zu schreiben. So wird die Verschlüsselung auf der IaaS-VM für die in Schritt 2 erwähnten Szenarien aktiviert.

5. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.


## <a name="terminology"></a>Begriff
Verwenden Sie die folgende Terminologietabelle, um sich mit einigen allgemeinen Begriffen im Rahmen dieser Technologie vertraut zu machen:

| Begriff | Definition |
| --- | --- |
| Azure AD | Azure AD steht für [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Ein Azure AD-Konto wird zum Authentifizieren, Speichern und Abrufen von Geheimnissen aus einem Schlüsseltresor verwendet. |
| Azure Key Vault | Azure Key Vault ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung (Federal Information Processing Standards) basiert, damit Ihre kryptografischen Schlüssel und Geheimnisse sicher und geschützt sind. Weitere Informationen finden Sie in der [Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| BitLocker |Bei [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) handelt es sich um eine branchenübliche Verschlüsselungstechnologie für Windows-Volumes, um die Datenträgerverschlüsselung auf virtuellen Windows-IaaS-Computern zu aktivieren. |
| BEK | BitLocker-Verschlüsselungsschlüssel (BitLocker Encryption Keys) werden verwendet, um das Startvolume des Betriebssystems und Datenvolumes zu verschlüsseln. Die BitLocker-Schlüssel sind in einem Schlüsseltresor als Geheimnisse geschützt. |
| Befehlszeilenschnittstelle (CLI) | Siehe [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-IaaS-Computern verwendet wird. |
| KEK | Der Schlüsselverschlüsselungsschlüssel (Key Encryption Key) ist der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen des Geheimnisses verwendet wird. Sie können einen mit Hardwaresicherheitsmodulen geschützten Schlüssel oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| PS-Cmdlets | Siehe [Azure PowerShell-Cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Azure Disk Encryption – Voraussetzungen](azure-security-disk-encryption-prerequisites.md)
