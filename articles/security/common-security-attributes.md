---
title: Sicherheitsattribute für Azure-Dienste
description: Eine Prüfliste allgemeiner Sicherheitsattribute für die Auswertung von Azure Service Fabric
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 2eb480e10ca3b674895d2d22cc44fb52f305f988
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007668"
---
# <a name="common-security-attributes-for-azure-services"></a>Allgemeine Sicherheitsattribute für Azure-Dienste

Sicherheit ist ein wesentlicher Bestandteil jedes Azure-Diensts. In diesem Artikel werden allgemeine Sicherheitsattribute für ausgewählte Azure-Dienste beschrieben. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## [<a name="azure-backup"></a>Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja | Verwenden der Speicherdienstverschlüsselung für Speicherkonten |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Nein  | Mithilfe von HTTPS |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein  |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Nein  |  |
| Verschlüsselte API-Aufrufe| Ja |  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein  |  |
| vNET Injection-Unterstützung| Nein  |  |
| Netzwerkisolation/Firewallunterstützung| Ja | Die Tunnelerzwingung wird für VM-Sicherungen unterstützt. Die Tunnelerzwingung wird für Workloads in virtuellen Computern nicht unterstützt. |
| Unterstützung für Tunnelerzwingung | Nein  |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Log Analytics wird über Diagnoseprotokolle unterstützt. Weitere Informationen finden Sie unter „Überwachen von Azure Backup-geschützten Workloads mit Log Analytics“ (unter https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/)). |

### <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Zugriffsverwaltung – Autorisierung| Ja | Vom Kunden erstellte und integrierte RBAC-Rollen werden verwendet. Weitere Informationen finden Sie unter „Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten“ (unter /azure/backup/backup-rbac-rs-vault). |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Alle vom Kunden über das Azure-Portal ausgelösten Aktionen werden in Aktivitätsprotokollen protokolliert. |
| Datenebene – Protokollierung und Überwachung| Nein  | Die Azure Backup-Datenebene kann nicht direkt aufgerufen werden.  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja|  |

## [<a name="azure-key-vault"></a>Azure Key Vault](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja | Alle Objekte werden verschlüsselt. |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Ja | Die gesamte Kommunikation erfolgt über verschlüsselte API-Aufrufe. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Der Kunde überwacht alle Schlüssel im Schlüsseltresor. Wenn vom Hardwaresicherheitsmodul (HSM) unterstützte Schlüssel angegeben werden, bietet ein FIPS Level 2 HSM Schutz für Schlüssel, Zertifikate oder Geheimnisse. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Mithilfe von Dienstendpunkten im virtuellen Netzwerk (VNet) |
| vNET Injection-Unterstützung| Nein  |  |
| Netzwerkisolation/Firewallunterstützung| Ja | Mithilfe von VNet-Firewallregeln |
| Unterstützung für Tunnelerzwingung | Nein  |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Mithilfe von Log Analytics |

### <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Zugriffsverwaltung – Autorisierung| Ja | Mithilfe der Schlüsseltresor-Zugriffsrichtlinie |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |
| Datenebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |

### <a name="access-controls"></a>Zugriffssteuerung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Zugriffskontrollen | Ja | Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene – Zugriffskontrollen (auf jeder Dienstebene) | Ja | Schlüsseltresor-Zugriffsrichtlinie |

## [<a name="azure-service-fabric"></a>Azure Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja | Der Cluster und die VM-Skalierungsgruppe, auf der der Cluster basiert, befinden sich im Besitz des Kunden. Azure Disk Encryption kann für die VM-Skalierungsgruppe aktiviert werden. |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Ja |  |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Der Cluster und die VM-Skalierungsgruppe, auf der der Cluster basiert, befinden sich im Besitz des Kunden. Azure Disk Encryption kann für die VM-Skalierungsgruppe aktiviert werden. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja | Service Fabric-API-Aufrufe erfolgen über Azure Resource Manager. Ein gültiges JSON Web Token (JWT) ist erforderlich. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| vNET Injection-Unterstützung| Ja |  |
| Netzwerkisolation/Firewallunterstützung| Ja | Verwendung von Netzwerksicherheitsgruppen (NSG) |
| Unterstützung für Tunnelerzwingung | Ja | Azure-Netzwerke unterstützen die Tunnelerzwingung. |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Durch die Unterstützung der Azure-Überwachung und von Drittanbietern. |

### <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Zugriffsverwaltung – Autorisierung| Ja | Identitäts- und Zugriffsverwaltung (IAM) für Aufrufe über SFRP. Direkte Aufrufe von Clusterendpunkten unterstützen zwei Rollen: „Benutzer“ und „Administrator“. Der Kunde kann die APIs einer der beiden Rollen zuordnen. |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Alle Vorgänge auf Steuerungsebene durchlaufen Überwachungs- und Genehmigungsprozesse. |
| Datenebene – Protokollierung und Überwachung| – | Der Cluster befindet sich im Besitz des Kunden.  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Die Dienstkonfiguration unterliegt der Versionsverwaltung und wird mit Azure Deploy bereitgestellt. Der Code (Anwendungs- und Laufzeitcode) unterliegt der Versionsverwaltung durch Azure Build.
 |

## <a name="azure-storage"></a>Azure Storage

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja |  |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Ja | Unterstützung für die Standardverfahren HTTPS/TLS.  Benutzer können Daten auch verschlüsseln, bevor sie mit dem Dienst übertragen werden. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Weitere Informationen finden Sie unter [Speicherdienstverschlüsselung mit von Kunden verwalteten Schlüsseln in Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja |  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| vNET Injection-Unterstützung| – |  |
| Netzwerkisolation/Firewallunterstützung| Ja | |
| Unterstützung für Tunnelerzwingung | – |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Azure Monitor-Metriken bereits verfügbar, Azure Monitor-Protokolle in der Vorschauversion |

### <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Azure Active Directory, gemeinsam verwendeter Schlüssel, SAS-Token. |
| Zugriffsverwaltung – Autorisierung| Ja | Unterstützung für die Autorisierung über rollenbasierte Zugriffssteuerung (RBAC), POSIX-Zugriffssteuerungslisten (ACLs) und SAS-Token |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung | Ja | Azure Resource Manager-Aktivitätsprotokoll |
| Datenebene – Protokollierung und Überwachung| Ja | Diagnoseprotokolle des Diensts, Azure Monitor-Protokolle in der Vorschauversion  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützung für die Versionierung durch den Ressourcenanbieter über Azure Resource Manager-APIs |