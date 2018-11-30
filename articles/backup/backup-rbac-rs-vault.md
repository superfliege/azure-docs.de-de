---
title: Verwalten von Sicherungen mit der rollenbasierten Zugriffssteuerung in Azure
description: Verwenden Sie die rollenbasierte Zugriffssteuerung zum Verwalten des Zugriffs auf Vorgänge der Sicherungsverwaltung im Recovery Services-Tresor.
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: trinadhk
ms.openlocfilehash: de7c00717349a1c814c5a13508adb11879aa10a5
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51704646"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten
Die rollenbasierte Access Control in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen.

> [!IMPORTANT]
> Rollen, die von Azure Backup bereitgestellt werden, sind auf Aktionen beschränkt, die im Azure-Portal oder mit PowerShell-Cmdlets für Recovery Services-Tresore ausgeführt werden können. Aktionen, die auf der Benutzeroberfläche des Azure Backup-Agent-Clients oder der Benutzeroberfläche von System Center Data Protection Manager oder der Benutzeroberfläche von Azure Backup Server ausgeführt werden, werden von diesen Rollen nicht gesteuert.

Azure Backup bietet drei integrierte Rollen, um Vorgänge der Sicherungsverwaltung zu steuern. Erfahren Sie mehr über [integrierte Rollen von Azure RBAC](../role-based-access-control/built-in-roles.md).

* [Mitwirkender für Sicherungen](../role-based-access-control/built-in-roles.md#backup-contributor): Diese Rolle verfügt über alle Berechtigungen zum Erstellen und Verwalten von Sicherungen. Mit dieser Rolle können aber keine Recovery Services-Tresore erstellt, und es kann anderen Personen kein Zugriff gewährt werden. Stellen Sie sich diese Rolle als Administrator der Sicherungsverwaltung vor, der alle Vorgänge der Sicherungsverwaltung ausführen kann.
* [Sicherungsoperator](../role-based-access-control/built-in-roles.md#backup-operator): Diese Rolle verfügt über Berechtigungen für alles, was ein Mitwirkender ausführen kann, außer Entfernen von Sicherungen und Verwalten von Sicherungsrichtlinien. Diese Rolle ist gleichbedeutend mit einem Mitwirkenden, es können damit jedoch keine destruktiven Vorgänge ausgeführt werden, z.B. Beenden einer Sicherung mit Löschung von Daten oder Entfernen die Registrierung von lokalen Ressourcen.
* [Sicherungsleser](../role-based-access-control/built-in-roles.md#backup-reader): Diese Rolle verfügt über Berechtigungen zum Anzeigen aller Vorgänge für die Sicherungsverwaltung. Stellen Sie sich diese Rolle für eine Überwachungsperson vor.

Wenn Sie Ihre eigenen Rollen definieren möchten, um eine noch bessere Kontrolle zu haben, helfen Ihnen die Informationen zum [Erstellen von benutzerdefinierten Rollen](../role-based-access-control/custom-roles.md) in Azure RBAC weiter.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Zuordnen der integrierten Backup-Rollen zu Aktionen der Sicherungsverwaltung
In der folgenden Tabelle sind die Aktionen der Sicherungsverwaltung und die entsprechende minimale RBAC-Rolle aufgeführt, die zum Ausführen des jeweiligen Vorgangs erforderlich ist.

| Verwaltungsvorgang | Minimal erforderliche RBAC-Rolle | Bereich erforderlich |
| --- | --- | --- |
| Erstellen eines Recovery Services-Tresors | Mitwirkender | Ressourcengruppe mit dem Tresor |
| Aktivieren der Sicherung von virtuellen Azure-Computern | Sicherungsoperator | Ressourcengruppe mit dem Tresor |
| | Mitwirkender von virtuellen Computern | VM-Ressource |
| Bedarfsgesteuerte Sicherung eines virtuellen Computers | Sicherungsoperator | Wiederherstellungstresorressource |
| Wiederherstellen eines virtuellen Computers | Sicherungsoperator | Recovery Services-Tresor |
| | Mitwirkender | Ressourcengruppe, in der der virtuelle Computer bereitgestellt wird. |
| | Mitwirkender von virtuellen Computern | Gesicherte Quell-VM |
| Wiederherstellen von VM-Sicherung nicht verwalteter Datenträger | Sicherungsoperator | Wiederherstellungstresorressource |
| | Mitwirkender von virtuellen Computern | Gesicherte Quell-VM |
| | Mitwirkender von Speicherkonto | Speicherkontoressource, in dem Datenträger wiederhergestellt werden sollen |
| Wiederherstellen von verwalteten Datenträgern aus VM-Sicherung | Sicherungsoperator | Wiederherstellungstresorressource |
| | Mitwirkender von virtuellen Computern | Gesicherte Quell-VM |
| | Mitwirkender von Speicherkonto | Temporäres Speicherkonto, das als Teil der Wiederherstellung ausgewählt wurde, um Daten aus dem Tresor zu speichern, bevor sie in verwaltete Datenträger konvertiert werden |
| | Mitwirkender | Die Ressourcengruppe, in der der/die verwaltete(n) Datenträger wiederhergestellt wird |
| Wiederherstellen von einzelnen Dateien aus VM-Sicherungen | Sicherungsoperator | Wiederherstellungstresorressource |
| | Mitwirkender von virtuellen Computern | Gesicherte Quell-VM |
| Erstellen einer Sicherungsrichtlinie für Azure-VM-Sicherungen | Mitwirkender für Sicherungen | Wiederherstellungstresorressource |
| Ändern der Sicherungsrichtlinie der Azure-VM-Sicherungen | Mitwirkender für Sicherungen | Wiederherstellungstresorressource |
| Löschen der Sicherungsrichtlinie der Azure-VM-Sicherungen | Mitwirkender für Sicherungen | Wiederherstellungstresorressource |
| Beenden der Sicherung (mit Beibehaltung oder Löschung von Daten) für VM-Sicherungen | Mitwirkender für Sicherungen | Wiederherstellungstresorressource |
| Registrieren einer lokalen Instanz von Windows Server/Client/SCDPM oder Azure Backup Server | Sicherungsoperator | Wiederherstellungstresorressource |
| Löschen der registrierten lokalen Instanz von Windows Server/Client/SCDPM oder Azure Backup Server | Mitwirkender für Sicherungen | Wiederherstellungstresorressource |

> [!IMPORTANT]
> Wenn Sie als Teil der VM-Einstellungen den VM-Mitwirkenden in einem VM-Ressourcenbereich angeben und auf „Sicherung“ klicken, öffnet sich der Bildschirm „Sicherung aktivieren“, obwohl die VM bereits gesichert ist, da der Aufruf zur Überprüfung des Sicherungsstatus nur auf Abonnementebene funktioniert. Um dies zu vermeiden, navigieren Sie entweder zum Tresor und öffnen die Sicherungselementansicht der VM, oder geben Sie die Rolle „VM-Mitwirkender“ auf Abonnementebene an. 

## <a name="next-steps"></a>Nächste Schritte
* [Rollenbasierte Zugriffssteuerung](../role-based-access-control/role-assignments-portal.md): Erste Schritte mit RBAC im Azure-Portal.
* Informationen zur Zugriffsverwaltung mit:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure-Befehlszeilenschnittstelle](../role-based-access-control/role-assignments-cli.md)
  * [REST-API](../role-based-access-control/role-assignments-rest.md)
* [Problembehandlung bei rollenbasierter Zugriffssteuerung:](../role-based-access-control/troubleshooting.md)Sehen Sie sich Vorschläge zur Behebung häufig auftretender Probleme an.
