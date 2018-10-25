---
ms.assetid: ''
title: VNET-Dienstendpunkte für Azure Key Vault | Microsoft-Dokumentation
description: Übersicht über Dienstendpunkte virtueller Netzwerke für Key Vault
services: key-vault
author: amitbapat
ms.author: ambapat
manager: mbaldwin
ms.date: 08/31/2018
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.openlocfilehash: 9b0600a7afb07600116440461037c7abcb9236de
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404323"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Dienstendpunkte virtueller Netzwerke für Azure Key Vault

Die Dienstendpunkte virtueller Netzwerke für Key Vault ermöglichen Ihnen, den Zugriff auf angegebene virtuelle Netzwerke und/oder eine Liste von IPv4-Adressbereichen (Internet Protocol, Version 4) zu beschränken. Allen Aufrufern, die außerhalb dieser Quellen eine Verbindung mit Ihrem Schlüsseltresor herstellen, wird der Zugriff verweigert. Wenn Kunden ein Abonnement abgeschlossen haben, bei dem Verbindungen über „vertrauenswürdige Microsoft-Dienste“ wie z.B. Office 365 Exchange Online, Office 365 SharePoint Online, Azure Compute, Azure Resource Manager oder Azure Backup zulässig sind, werden diese Dienste durch die Firewall weitergeleitet. Solche Aufrufer müssen natürlich weiterhin ein gültiges AAD-Token vorlegen und über Berechtigungen (als Zugriffsrichtlinien konfiguriert) zum Ausführen des angeforderten Vorgangs verfügen. Weitere technische Details finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Verwendungsszenarien

Sie können [Key Vault-Firewalls und virtuelle Netzwerke](key-vault-network-security.md) so konfigurieren, dass der Zugriff auf Datenverkehr aus allen Netzwerken (einschließlich Internetdatenverkehr) standardmäßig verweigert wird. Es kann Zugriff auf Datenverkehr aus bestimmten virtuellen Azure-Netzwerken und/oder IP-Adressbereichen des öffentlichen Internets gewährt werden, sodass Sie eine sichere Netzwerkgrenze für Ihre Anwendungen erstellen können.

> [!NOTE]
> Key Vault-Firewalls und Regeln für virtuelle Netzwerke gelten NUR für die Key Vault-[Datenebene](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control). Vorgänge auf Key Vault-Steuerungsebene (z.B. Vorgänge zum Erstellen, Löschen und Ändern des Schlüsseltresors, Festlegen von Zugriffsrichtlinien, Festlegen von Firewalls und Regeln für virtuelle Netzwerke) sind von Firewalls und Regeln für virtuelle Netzwerke nicht betroffen.

Beispiel:
* Sie verwenden Key Vault zum Speichern von Verschlüsselungsschlüsseln, Anwendungsgeheimnissen und Zertifikaten und möchten den Zugriff auf Ihren Schlüsseltresor aus dem öffentlichen Internet blockieren.
* Sie möchten den Zugriff auf Ihren Schlüsseltresor sperren, sodass nur Ihre Anwendung oder einige festgelegte Hosts eine Verbindung mit dem Schlüsselspeicher herstellen können.
* Sie führen eine Anwendung in Ihrem virtuellen Azure-Netzwerk (Azure VNET) aus, und dieses VNET ist für den gesamten eingehenden und ausgehenden Datenverkehr gesperrt. Die Anwendung muss dennoch weiterhin eine Verbindung mit dem Schlüsseltresor herstellen, um Geheimnisse abzurufen oder Kryptografieschlüssel zu verwenden.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken

Es folgen die erforderlichen Schritte zum Konfigurieren von Firewalls und virtuellen Netzwerken. Diese Schritte sind immer gleich, unabhängig davon, welche Schnittstelle (PowerShell, CLI, Azure-Portal) Sie verwenden, um die Firewallregeln und Regeln für virtuelle Netzwerke einzurichten.
1. Optional, jedoch dringend empfohlen: Aktivieren Sie die [Schlüsseltresor-Protokollierung](key-vault-logging.md), um detaillierte Zugriffsprotokolle anzuzeigen. Dies ist hilfreich bei der Diagnose, wenn Firewallregeln und Regeln für virtuelle Netzwerke den Zugriff auf einen Schlüsseltresor verhindern.
2. Aktivieren Sie Dienstendpunkte für Key Vault für virtuelle Zielnetzwerke und Subnetze.
3. Legen Sie Firewalls und Regeln für virtuelle Netzwerke für einen Schlüsseltresor fest, um den Zugriff auf diesen Schlüsseltresor aus bestimmten virtuellen Netzwerken, Subnetzen und IPv4-Adressbereichen zu beschränken.
4. Wenn dieser Schlüsseltresor für vertrauenswürdige Microsoft-Dienste zugänglich sein soll, müssen Sie die Option zum Zulassen des Zugriffs auf den Schlüsselspeicher durch vertrauenswürdige Azure-Dienste aktivieren.

Ausführliche Anweisungen finden Sie unter [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md).

> [!IMPORTANT]
> Sobald Firewallregeln in Kraft sind, können alle Vorgänge auf Key Vault-[Datenebene](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) NUR ausgeführt werden, wenn Aufruferanforderungen aus zulässigen virtuellen Netzwerken oder IPV4-Adressbereichen stammen. Dies gilt auch für den Zugriff auf den Schlüsseltresor im Azure-Portal. Obwohl ein Benutzer im Azure-Portal zu einem Schlüsseltresor navigieren kann, kann er möglicherweise keine Schlüssel, Geheimnisse oder Zertifikate auflisten, wenn sein Clientcomputer nicht in der Zulassungsliste enthalten ist. Dies wirkt sich auch auf die Key Vault-Auswahl anderer Azure-Dienste aus. Benutzer können möglicherweise eine Liste von Schlüsseltresoren einsehen, aber keine Schlüssel auflisten, wenn Firewallregeln ihren Clientcomputer blockieren.


> [!NOTE]
> * Maximal 127 VNET-Regeln und 127 IPv4-Regeln sind zulässig. 
> * Kleine Adressbereiche mit der Präfixgröße „/ 31“ oder „/ 32“ werden nicht unterstützt. Diese Bereiche müssen mit einzelnen IP-Adressregeln konfiguriert werden.
> * IP-Netzwerkregeln sind nur für öffentliche IP-Adressen zulässig. Für private Netzwerke reservierte IP-Adressbereiche (gemäß RFC 1918) sind in IP-Adressregeln nicht zulässig. Private Netzwerke enthalten Adressen, die mit *10.*\*, *172.16.*\* und *192.168.*\* beginnen. 
> * Derzeit werden nur IPv4-Adressen unterstützt.

## <a name="trusted-services"></a>Vertrauenswürdige Dienste
Es folgt eine Liste der vertrauenswürdigen Dienste, denen Zugriff auf einen Schlüsseltresor gewährt wird, wenn die Option zum Zulassen vertrauenswürdiger Dienste aktiviert ist.

|Vertrauenswürdiger Dienst|Verwendungsszenarien|
| --- | --- |
|Azure Virtual Machines-Bereitstellung (Dienst)|[Deploy Certificates to VMs from customer-managed Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (Bereitstellen von Zertifikaten auf virtuellen Computern über eine vom Kunden verwaltete Key Vault-Instanz)|
|Azure Resource Manager-Vorlagenbereitstellung (Dienst)|[Übergeben sicherer Werte während der Bereitstellung](../azure-resource-manager/resource-manager-keyvault-parameter.md)|
|Azure Disk Encryption-Volumeverschlüsselung (Dienst)|Zulassen des Zugriffs auf den BitLocker-Schlüssel (virtueller Windows-Computer) oder die DM-Passphrase (virtueller Linux-Computer) und den Schlüssel für die Schlüsselverschlüsselung bei der VM-Bereitstellung zum Aktivieren von [Azure Disk Encryption](../security/azure-security-disk-encryption.md)|
|Azure Backup|Zulassen der Sicherung und Wiederherstellung von relevanten Schlüsseln und Geheimnissen während der Azure-VM-Sicherung mithilfe von [Azure Backup](../backup/backup-introduction-to-azure-backup.md)|
|Exchange Online und SharePoint Online|Zulassen des Zugriffs auf den Kundenschlüssel für die Dienstverschlüsselung mit [Kundenschlüssel](https://support.office.com/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697)|
|Azure Information Protection|Zulassen des Zugriffs auf den Mandantenschlüssel für [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|App-Dienste|[Bereitstellen eines Azure-Web-App-Zertifikats über Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)|
|Azure SQL|[Transparent Data Encryption mit BYOK-Unterstützung (Bring Your Own Key) für Azure SQL-Datenbank und Data Warehouse](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current)|
|Azure Storage|[Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md)|
|Azure Data Lake Store|[Datenverschlüsselung in Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md) mit von Kunden verwalteten Schlüsseln|



> [!NOTE]
> Die relevanten Key Vault-Zugriffsrichtlinien müssen so festgelegt werden, dass die entsprechenden Dienste Zugriff auf den Schlüsseltresor erhalten.

## <a name="next-steps"></a>Nächste Schritte

* [Schützen Ihrer Key Vault-Instanz](key-vault-secure-your-key-vault.md)
* [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](key-vault-network-security.md)