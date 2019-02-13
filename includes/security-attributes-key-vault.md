---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: aba09012bf2e9d2741f598280add8b599a6f6d1a
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55807264"
---
## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Hinweise |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja | Alle Objekte werden verschlüsselt. |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Ja | Die gesamte Kommunikation erfolgt über verschlüsselte API-Aufrufe. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Der Kunde überwacht alle Schlüssel im Schlüsseltresor. Wenn vom Hardwaresicherheitsmodul (HSM) unterstützte Schlüssel angegeben werden, bietet ein FIPS Level 2 HSM Schutz für Schlüssel, Zertifikate oder Geheimnisse. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Hinweise |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Mithilfe von Dienstendpunkten im virtuellen Netzwerk (VNet) |
| vNET Injection-Unterstützung| Nein |  |
| Netzwerkisolation/Firewallunterstützung| Ja | Mithilfe von VNet-Firewallregeln |
| Unterstützung für Tunnelerzwingung | Nein  |  |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Hinweise|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, App Insights usw.)| Ja | Mithilfe von Log Analytics |

## <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Hinweise|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Zugriffsverwaltung – Authentifizierung| Ja | Mithilfe der Schlüsseltresor-Zugriffsrichtlinie |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Hinweise|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |
| Datenebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |

## <a name="access-controls"></a>Zugriffssteuerung

| Sicherheitsattribut | Ja/Nein | Hinweise|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Zugriffskontrollen | Ja | Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene – Zugriffskontrollen (auf jeder Dienstebene) | Ja | Schlüsseltresor-Zugriffsrichtlinie |
