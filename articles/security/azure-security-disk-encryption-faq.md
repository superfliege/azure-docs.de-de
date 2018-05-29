---
title: Azure Disk Encryption FAQ| Microsoft-Dokumentation
description: Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Microsoft Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2018
ms.author: barclayn
ms.openlocfilehash: 47ccf91a64653c928cc4da01bc98535c97440d37
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187690"
---
# <a name="azure-disk-encryption-faq"></a>Häufig gestellte Fragen zu Azure Disk Encryption

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer. Weitere Informationen zu diesem Dienst finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Wo ist Azure Disk Encryption allgemein verfügbar?

Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer ist in allen öffentlichen Azure-Regionen allgemein verfügbar.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Welche Benutzeroberflächen sind für Azure Disk Encryption verfügbar?

Azure Disk Encryption mit allgemeiner Verfügbarkeit unterstützt Azure Resource Manager-Vorlagen, Azure PowerShell und die Azure CLI. Dies bietet Ihnen ein hohes Maß an Flexibilität. Ihnen stehen drei Optionen zum Aktivieren der Datenträgerverschlüsselung für Ihre IaaS-VMs zur Verfügung. Weitere Informationen zur Benutzeroberfläche und eine Schritt-für-Schritt-Anleitung sind in den [Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences) verfügbar.

## <a name="how-much-does-azure-disk-encryption-cost"></a>Was kostet Azure Disk Encryption?

Es fallen keine Gebühren für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption an, es werden jedoch Gebühren für die Verwendung von Azure Key Vault berechnet. Weitere Informationen zu den Azure Key Vault-Kosten finden Sie auf der Seite [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Welche VM-Tarife unterstützt Azure Disk Encryption?

Azure Disk Encryption ist in VMs mit Standardtarifen einschließlich IaaS-VMs der Serien [A, D, DS, G, GS und F](https://azure.microsoft.com/pricing/details/virtual-machines/) verfügbar. Es ist auch für VMs mit Storage Premium verfügbar. Es steht nicht für VMs im Tarif „Basic“ zur Verfügung.

## <a name="what-linux-distributions-does-azure-disk-encryption-support"></a>Welche Linux-Distributionen werden von Azure Disk Encryption unterstützt?

Azure Disk Encryption wird für die folgenden Linux-Serverdistributionen und -Versionen unterstützt:

| Linux-Distribution | Version | Für die Verschlüsselung unterstützter Volumetyp|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Betriebssystem- und andere Datenträger |
| Ubuntu | 14.04.5-DAILY-LTS | Betriebssystem- und andere Datenträger |
| RHEL | 7.4 | Datenträger* |
| RHEL | 7.3 | Datenträger* |
| RHEL | 7.2 | Datenträger* |
| RHEL | 6,8 | Datenträger* |
| RHEL | 6.7 | Datenträger* |
| CentOS | 7.3 | Betriebssystem- und andere Datenträger |
| CentOS | 7.2n | Betriebssystem- und andere Datenträger |
| CentOS | 6,8 | Betriebssystem- und andere Datenträger |
| CentOS | 7.1 | Datenträger |
| CentOS | 7.0 | Datenträger |
| CentOS | 6.7 | Datenträger |
| CentOS | 6.6 | Datenträger |
| CentOS | 6,5 | Datenträger |
| openSUSE | 13.2 | Datenträger |
| SLES | 12 SP1 | Datenträger |
| SLES | Priorität: 12-SP1 | Datenträger |
| SLES | HPC 12 | Datenträger |
| SLES | Priorität: 11-SP4 | Datenträger |
| SLES | 11 SP4 | Datenträger |

*__ADE wird für RHEL für Datenträger unterstützt. Die aktuelle ADE-Implementierung funktioniert zwar für Betriebssystemdatenträger, wird aber derzeit nicht gemeinsam unterstützt. Microsoft und Red Hat arbeiten an einer gemeinsam unterstützten Lösung. In der Zwischenzeit können Sie das ADE-Whitepaper für die Verschlüsselung von Linux-Betriebssystemdatenträgern lesen. Sie finden es [hier](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Wie sehen die ersten Schritte mit Azure Disk Encryption aus?

Lesen Sie hierzu das Whitepaper [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Kann ich sowohl Start- als auch Datenvolumes mit Azure Disk Encryption verschlüsseln?

Ja. Sie können Start- und Datenvolumes für virtuelle Windows- und Linux-IaaS-Computer verschlüsseln. Bei Windows-VMs können Sie die Daten ohne vorherige Verschlüsselung des Betriebssystemvolumes nicht verschlüsseln. Bei Linux-VMs können Sie das Datenvolume ohne vorherige Verschlüsselung des Betriebssystemvolumes verschlüsseln. Nachdem Sie das Betriebssystemvolume für Linux verschlüsselt haben, wird das Deaktivieren der Verschlüsselung auf einem Betriebssystemvolume für Linux-IaaS-VMs nicht unterstützt.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>Ermöglicht Azure Disk Encryption die Verwendung von „Bring Your Own Key“ (BYOK, dt. Eigenen Schlüssel verwenden)?

Ja. Sie können Ihre eigenen Schlüssel für die Verschlüsselung anderer Schlüssel (Key Encryption Keys, KEKs) angeben. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Szenarien finden Sie in den [Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Kann ich einen von Azure erstellten KEK verwenden?

Ja. Sie können Azure Key Vault verwenden, um einen KEK für Azure Disk Encryption zu erstellen. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den [Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Kann ich den lokalen Schlüsselverwaltungsdienst oder HSM verwenden, um die Verschlüsselungsschlüssel zu schützen?

Sie können den lokalen Schlüsselverwaltungsdienst oder HSM nicht verwenden, um die Verschlüsselungsschlüssel mit Azure Disk Encryption zu schützen. Sie können hierzu nur den Azure Key Vault-Dienst verwenden. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den [Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Was sind die Voraussetzungen für die Konfiguration von Azure Disk Encryption?

Ein PowerShell-Skript muss ausgeführt werden. Mit diesem Skript können Sie eine Azure Active Directory-Anwendung erstellen, einen neuen Schlüsseltresor einrichten oder einen vorhandenen Schlüsseltresor für den verschlüsselten Datenträgerzugriff einrichten, um die Verschlüsselung zu aktivieren und Geheimnisse und Schlüssel zu schützen. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den [Voraussetzungen sowie den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption](azure-security-disk-encryption.md#prerequisites).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Wo erhalte ich weitere Informationen zum Konfigurieren von Azure Disk Encryption mithilfe von PowerShell?

Es sind einige hervorragende Artikel zu grundlegenden Aufgaben für Azure Disk Encryption sowie zu komplexeren Szenarien verfügbar. Informationen zu den grundlegenden Aufgaben finden Sie unter [Explore Azure Disk Encryption with Azure PowerShell – Part 1 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Informationen zu erweiterten Szenarien finden Sie unter [Explore Azure Disk Encryption with Azure PowerShell – Part 2 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Welche Version von Azure PowerShell wird von Azure Disk Encryption unterstützt?

Verwenden Sie die neueste Version des Azure PowerShell SDK, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter. Azure Disk Encryption wird *nicht* vom Azure SDK Version 1.1.0 unterstützt.

> [!NOTE]
> Die Vorschauerweiterung von Azure Disk Encryption auf Linux ist veraltet. Weitere Informationen finden Sie unter [Deprecating Azure disk encryption preview extension for Linux IaaS VMs](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/) (Die Vorschauerweiterung von Azure Disk Encryption unter Linux ist veraltet).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Kann ich Azure Disk Encryption auf mein benutzerdefiniertes Linux-Image anwenden?

Sie können Azure Disk Encryption nicht auf Ihr benutzerdefiniertes Linux-Image anwenden. Wir unterstützen nur die Linux-Images im Katalog für die zuvor genannten unterstützten Distributionen. Wir unterstützen derzeit keine benutzerdefinierten Linux-Images.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Kann ich eine Linux Red Hat-VM mit dem „yum“-Update aktualisieren?

Ja. Sie können auf eine Linux Red Hat-VM einen Update- oder Patchvorgang anwenden. Weitere Informationen finden Sie unter [Applying updates to an encrypted Azure IaaS Red Hat VM by using the yum update](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/) (Anwenden von Updates auf eine verschlüsselte Azure IaaS Red Hat-VM mithilfe des yum-Updates).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Welcher Azure Disk Encryption-Workflow wird für Linux empfohlen?

Der folgende Workflow wird empfohlen, um unter Linux die besten Ergebnisse zu erzielen:
* Beginnen Sie mit dem unveränderten Katalogimage für die gewünschte Distribution und Version des Betriebssystems.
* Sichern Sie alle eingebundenen Laufwerke, die verschlüsselt werden.  Dies ermöglicht die Wiederherstellung bei einem Ausfall, wenn z.B. der virtuelle Computer vor dem Abschluss der Verschlüsselung neu gestartet wird.
* Führen Sie die Verschlüsselung durch (dies kann je nach VM-Merkmalen und der Größe aller angefügten Datenträger mehrere Stunden oder sogar Tage dauern).
* Passen Sie das Image an, und fügen Sie bei Bedarf Software hinzu.

Wenn dieser Workflow nicht möglich ist, stellt die [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE, Speicherdienstverschlüsselung) auf Ebene des Plattform-Speicherkontos möglicherweise eine Alternative zur vollständigen Datenträgerverschlüsselung mit dm-crypt dar.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>Um was handelt es sich beim Datenträger „Bek Volume“ bzw. „/mnt/azure_bek_disk“?

„Bek volume“ für Windows oder „/mnt/azure_bek_disk“ für Linux ist ein lokales Datenvolume, auf dem die Verschlüsselungsschlüssel für verschlüsselte Azure-IaaS-VMs sicher gespeichert werden.
> [!NOTE]
> Löschen oder bearbeiten Sie keine Inhalte auf diesem Datenträger. Die Bereitstellung des Datenträgers kann nicht aufgehoben werden, da für Verschlüsselungsvorgänge auf der IaaS-VM der Verschlüsselungsschlüssel vorhanden sein muss.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Wo kann ich Fragen stellen oder Feedback geben?

Sie können im [Azure Disk Encryption-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption) Fragen stellen und Feedback geben.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit Azure Disk Encryption erhalten. Weitere Informationen zu diesem Dienst und seinen Funktionen finden Sie in den folgenden Artikeln:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Datenverschlüsselung ruhender Azure-Daten](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
