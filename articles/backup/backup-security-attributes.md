---
title: Häufig verwendete Sicherheitsattribute für Azure Backup
description: Eine Prüfliste allgemeiner Sicherheitsattribute für die Auswertung von Azure Backup
services: backup
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 93dd5372bffb278894987cd3cc2b8322cbc5e577
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679678"
---
# <a name="common-security-attributes-for-azure-backup"></a>Häufig verwendete Sicherheitsattribute für Azure Backup

Sicherheit ist ein wesentlicher Bestandteil jedes Azure-Diensts. In diesem Artikel werden die in Azure Backup integrierten Sicherheitsattribute beschrieben. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja | Verwenden der Speicherdienstverschlüsselung für Speicherkonten |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Nein  | Mithilfe von HTTPS |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein  |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Nein  |  |
| Verschlüsselte API-Aufrufe| Ja |  |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein  |  |
| vNET Injection-Unterstützung| Nein  |  |
| Netzwerkisolation/Firewallunterstützung| Ja | Die Tunnelerzwingung wird für VM-Sicherungen unterstützt. Die Tunnelerzwingung wird für Workloads in virtuellen Computern nicht unterstützt. |
| Unterstützung für Tunnelerzwingung | Nein  |  |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Log Analytics wird über Diagnoseprotokolle unterstützt. Weitere Informationen finden Sie unter [Monitor Azure Backup protected workloads using Log Analytics (Überwachen von Azure Backup-geschützten Workloads mit Log Analytics)](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |

## <a name="iam-support"></a>IAM-Unterstützung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Zugriffsverwaltung – Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Zugriffsverwaltung – Autorisierung| Ja | Vom Kunden erstellte und integrierte RBAC-Rollen werden verwendet. Weitere Informationen finden Sie unter [Use Role-Based Access Control to manage Azure Backup recovery points (Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten)](/azure/backup/backup-rbac-rs-vault). |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Alle vom Kunden über das Azure-Portal ausgelösten Aktionen werden in Aktivitätsprotokollen protokolliert. |
| Datenebene – Protokollierung und Überwachung| Nein  | Die Azure Backup-Datenebene kann nicht direkt aufgerufen werden.  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja|  |