---
title: Azure-Sicherheitsfeatures, die für virtuelle Azure-Computer verwendet werden | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die wichtigsten Sicherheitsfunktionen von Azure, die mit Azure Virtual Machines verwendet werden können.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 7d31a434d4ead6dd8f8a13a08d368389904b5b4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364232"
---
# <a name="azure-virtual-machines-security-overview"></a>Virtuelle Azure-Computer – Sicherheitsübersicht
Mit Azure Virtual Machines können Sie sehr flexibel eine Vielzahl unterschiedlicher Computinglösungen bereitstellen. Der Dienst unterstützt Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP und Azure BizTalk Services. Daher können Sie jede Workload und jede Sprache für nahezu alle Betriebssysteme bereitstellen.

Ein virtueller Azure-Computer bietet Ihnen die Flexibilität der Virtualisierung, ohne Zeit und Geld für den Kauf und das Verwalten der Hardware aufwenden zu müssen, die den virtuellen Computer hostet. Sie können Ihre Anwendungen in der Gewissheit erstellen und bereitstellen, dass Ihre Daten in hochsicheren Datencentern geschützt und abgesichert sind.

Mit Azure können Sie richtlinienkonforme Lösungen mit erweiterter Sicherheit erstellen, die:

* Ihre virtuellen Computer vor Viren und Schadsoftware schützen.
* Ihre sensiblen Daten verschlüsseln.
* Netzwerkdatenverkehr absichern.
* Bedrohungen erkennen.
* Konformitätsvorgaben einhalten.

Das Ziel dieses Artikels ist, eine Übersicht über die wichtigsten Azure-Sicherheitsfunktionen zu bieten, die mit virtuellen Computern verwendet werden können. Links zu Artikeln bieten weitere Informationen zu den einzelnen Features.  

## <a name="antimalware"></a>Antimalware
Mit Azure können Sie Antischadsoftware von Sicherheitsanbietern wie Microsoft, Symantec, Trend Micro und Kaspersky verwenden. Diese Software dient dem Schutz Ihrer virtuellen Computer vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen.

Microsoft Antimalware for Azure Cloud Services and Virtual Machines ist eine Echtzeit-Schutzfunktion zum Bestimmen und Entfernen von Viren, Spyware und anderer Schadsoftware.  Microsoft Antimalware für Azure gibt konfigurierbare Warnungen aus, wenn bekannte schädliche oder unerwünschte Software versucht, sich selbst auf Ihren Azure-Systemen zu installieren oder dort auszuführen.

Microsoft Antimalware für Azure ist eine Lösung mit einem einzelnen Agent für Anwendungen und Mandantenumgebungen, die im Hintergrund ohne Eingreifen des Benutzers ausgeführt wird. Sie können Schutz basierend auf den Anforderungen der Anwendungsworkloads bereitstellen, entweder mit der einfachen Konfiguration, die Schutz durch die Standardeinstellungen bietet, oder mit einer erweiterten benutzerdefinierten Konfiguration, einschließlich Antischadsoftwareüberwachung.

Wenn Sie Microsoft Antimalware für Azure bereitstellen und aktivieren, sind die folgenden Kernfunktionen verfügbar:

* **Echtzeitschutz**: Überwacht die Aktivitäten in Cloud Services und Virtual Machines, um die Ausführung von Schadsoftware zu erkennen und zu blockieren.
* **Geplantes Scannen**: Führt regelmäßig gezielte Scans aus, um Schadsoftware, einschließlich aktiv ausgeführter Programme, zu erkennen.
* **Schadsoftwarebehandlung**: Führt automatisch Aktionen für erkannte Schadsoftware aus, z.B. das Löschen schädlicher Dateien, das Unter-Quarantäne-Stellen schädlicher Dateien und das Bereinigen schädlicher Registrierungseinträge.
* **Signaturaktualisierungen**: Installiert automatisch mit einer vordefinierten Häufigkeit die neuesten Schutzsignaturen (Virendefinitionen), um sicherzustellen, dass der Schutz auf dem neuesten Stand ist.
* **Aktualisierungen der Antimalware-Engine**: Aktualisiert automatisch die Engine für Microsoft Antimalware für Azure.
* **Aktualisierungen der Antimalware-Plattform**: Aktualisiert automatisch die Plattform für Microsoft Antimalware für Azure.
* **Aktiver Schutz**: Meldet Telemetriemetadaten zu erkannten Bedrohungen und verdächtigen Ressourcen an Azure, um eine schnelle Reaktion zu gewährleisten. Ermöglicht die synchrone Signaturbereitstellung in Echtzeit über den Microsoft Active Protection Service (MAPS).
* **Übermittlung von Stichproben**: Übermittelt Stichproben an den Microsoft Antimalware für Azure-Dienst, um den Dienst zu optimieren und die Problembehandlung zu ermöglichen.
* **Ausschlüsse**: Ermöglicht Anwendungs- und Dienstadministratoren, aus Leistungs- und anderen Gründen bestimmte Dateien, Prozesse und Laufwerke vom Schutz und von Scans auszuschließen.
* **Antischadsoftware-Ereigniserfassung**: Zeichnet die Integrität des Antischadsoftwarediensts, verdächtige Aktivitäten und durchgeführte Wiederherstellungsaktionen im Ereignisprotokoll des Betriebssystems auf und erfasst sie in Ihrem Azure Storage-Konto.

Weitere Informationen zu Antischadsoftware zum Schutz Ihrer virtuellen Computer:

* [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](azure-security-antimalware.md)
* [Deploying Antimalware Solutions on Azure Virtual Machines (Bereitstellen von Antischadsoftware-Lösungen auf virtuellen Azure-Computern, in englischer Sprache)](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-trend.md)
* [Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-symantec.md)
* [Willkommen im Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardwaresicherheitsmodul
Verschlüsselungs- und Authentifizierungsschutzmaßnahmen können durch eine höhere Sicherheit verbessert werden. Sie können die Verwaltung und Sicherheit Ihrer geschäftskritischen geheimen Daten und Schlüssel vereinfachen, indem Sie diese in Azure Key Vault speichern. 

Mit Key Vault können Sie Ihre Schlüssel in Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) speichern, die gemäß FIPS 140-2 Level 2-Standards zertifiziert sind. Sie können Ihre SQL Server-Verschlüsselungsschlüssel für Sicherungen oder [transparente Datenverschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx) gemeinsam mit den Schlüsseln oder geheimen Daten Ihrer Anwendungen in Key Vault speichern. Der Zugriff auf diese geschützten Elemente sowie die zugehörigen Berechtigungen werden über [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)verwaltet.

Weitere Informationen:

* [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-whatis.md)
* [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md)
* [Azure Key Vault-Blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Verschlüsselung der Datenträger virtueller Computer
Azure Disk Encryption ist eine neue Funktion zur Verschlüsselung der Datenträger virtueller Windows- und Linux-Computer. Azure Disk Encryption verwendet das Branchenstandardfeature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen.

Die Lösung ist in Azure Key Vault integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Sie stellt außerdem sicher, dass alle ruhenden Daten auf den Datenträgern der virtuellen Computer in Azure Storage verschlüsselt sind.

Weitere Informationen:

* [Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption für virtuelle Computer unter Windows und Linux](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Verschlüsseln eines virtuellen Computers](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sicherung virtueller Computer
Azure Backup ist eine skalierbare Lösung, die Ihre Anwendungsdaten schützt – ganz ohne Investitionskosten und mit sehr niedrigen Betriebskosten. Anwendungsfehler können Ihre Daten beschädigen, und menschliche Fehler können Bugs in Ihren Anwendungen verursachen. Mit Azure Backup sind Ihre virtuellen Windows- und Linux-Computer geschützt.

Weitere Informationen:

* [Was ist Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup-Dienst – Häufig gestellte Fragen](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Ein wichtiger Teil der BCDR-Strategie Ihrer Organisation ist die Ermittlung, wie geschäftliche Workloads und Apps verfügbar gehalten werden können, wenn geplante und ungeplante Ausfälle auftreten. Azure Site Recovery unterstützt die Orchestrierung von Replikation, Failover und Wiederherstellung von Workloads und Apps, damit diese Komponenten über einen sekundären Standort zur Verfügung stehen, wenn der primäre Standort ausfällt.

Site Recovery:

* **Vereinfachung Ihrer BCDR-Strategie**: Site Recovery vereinfacht die Replikation, das Failover und die Wiederherstellung mehrerer Geschäftsworkloads und -Apps von einem einzelnen Standort. Site Recovery orchestriert die Replikation und das Failover, aber das Tool fängt keine Anwendungsdaten ab und besitzt auch keinerlei Informationen hierzu.
* **Ermöglicht flexible Replikation**: Mithilfe von Site Recovery können Sie Workloads auf virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows-/Linux-Servern replizieren.
* **Einfaches Failover und leichte Wiederherstellung werden unterstützt**: Site Recovery verfügt über Testfailover, mit denen Übungen zur Notfallwiederherstellung unterstützt werden, ohne dass sich dies auf Produktionsumgebungen auswirkt. Außerdem können Sie geplante Failover ohne Datenverlust für erwartete Ausfälle oder ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) für unerwartete Notfälle ausführen. Nach dem Failover können Sie ein Failback zu Ihren primären Standorten durchführen. Site Recovery verfügt über Wiederherstellungspläne, die Skripts und Azure Automation-Arbeitsmappen enthalten können, sodass Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen anpassen können.
* **Beseitigung sekundärer Datencenter**: Sie können die Replikation zu einem sekundären lokalen Standort oder zu Azure durchführen. Wenn Sie Azure als Ziel für die Notfallwiederherstellung nutzen, vermeiden Sie die Kosten und Komplexität, die mit der Verwaltung eines sekundären Standorts verbunden sind. Replizierte Daten werden in Azure Storage gespeichert.
* **Integration in vorhandene BCDR-Technologien**: Site Recovery kann zusammen mit anderen BCDR-Features für Anwendungen eingesetzt werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen. Dies umfasst auch die native Unterstützung für SQL Server Always On zum Verwalten des Failovers von Verfügbarkeitsgruppen.

Weitere Informationen:

* [Was ist Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Wie funktioniert Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Welche Workloads werden durch Azure Site Recovery geschützt?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuelle Netzwerke
Virtuelle Computer benötigen Netzwerkkonnektivität. Azure erfordert von einem virtuellen Computer eine Verbindung mit einem virtuellen Azure-Netzwerk, damit die Konnektivitätsanforderung unterstützt wird. 

Ein virtuelles Azure-Netzwerk ist ein logisches Konstrukt, das auf dem physischen Azure-Netzwerkfabric basiert. Jedes logische virtuelle Azure-Netzwerk ist von allen anderen virtuellen Azure-Netzwerken isoliert. Mit dieser Isolierung wird sichergestellt, dass der Netzwerkverkehr in Ihren Bereitstellungen nicht für andere Microsoft Azure-Kunden zugänglich ist.

Weitere Informationen:

* [Die Netzwerksicherheit in Azure in der Übersicht](security-network-overview.md)
* [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)
* [Netzwerkfeatures und Partnerschaften für Enterpriseszenarien](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Verwaltung von Sicherheitsrichtlinien und Berichtserstellung
Mit Azure Security Center können Sie Bedrohungen verhindern, erkennen und bekämpfen. Security Center sorgt für eine größere Transparenz und bessere Kontrolle der Sicherheit Ihrer Azure-Ressourcen. Es bietet eine integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements. Es hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum von Sicherheitslösungen verwendet werden.

Mit Security Center können Sie die Sicherheit Ihrer virtuellen Computer durch folgende Aktionen optimieren und überwachen:

* Bereitstellen von [Sicherheitsempfehlungen](../security-center/security-center-recommendations.md) für die virtuellen Computer. Empfehlungen beziehen sich beispielsweise auf das Anwenden von Systemupdates, das Konfigurieren von ACL-Endpunkten, das Aktivieren von Antischadsoftware, das Aktivieren von Netzwerksicherheitsgruppen und das Anwenden der Datenträgerverschlüsselung.
* Überwachen des Status Ihrer virtuellen Computer.

Weitere Informationen:

* [Einführung in Azure Security Center](../security-center/security-center-intro.md)
* [Azure Security Center – Häufig gestellte Fragen](../security-center/security-center-faq.md)
* [Planungs- und Betriebshandbuch für Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Compliance
Azure Virtual Machines ist für FISMA, FedRAMP, HIPAA, PCI DSS Level 1 und andere Complianceprogramme zertifiziert. So können Sie auch mit Ihren eigenen Azure-Anwendungen die verschiedenen Complianceanforderungen besser erfüllen, und Ihr Unternehmen kann sicherstellen, dass diverse nationale und internationale rechtliche Vorgaben eingehalten werden.

Weitere Informationen:

* [Microsoft Trust Center – Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Die vertrauenswürdige Cloud: Sicherheit, Datenschutz und Compliance von Microsoft Azure](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
