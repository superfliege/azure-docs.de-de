---
title: Automatisches Update der Azure-Notfallwiederherstellung in Mobility Service in Azure | Microsoft-Dokumentation
description: Bietet einen Überblick über die automatische Aktualisierung von Mobility Service, die für die Replikation von Azure-VMs mit Azure Site Recovery verwendet wird.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: rajanaki
ms.openlocfilehash: 45f2e2927f699769bb385038c04d4dd23e075a9a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779687"
---
# <a name="automatic-update-of-mobility-service-extension-in-azure-to-azure-replication"></a>Automatische Aktualisierung der Mobility Service-Erweiterung in Azure-zu-Azure-Replikation

Bei Azure Site Recovery werden in einem monatlichen Versionsrhythmus Verbesserungen an vorhandenen Features vorgenommen oder neue hinzugefügt, und ggf. werden bekannte Probleme behoben. Um den Dienst stets auf dem neuesten Stand zu halten, müssen Sie also die Bereitstellung dieser Patches in monatlichem Rhythmus planen. Um unnötigen Aufwand in Verbindung mit dem Upgrade zu vermeiden, können Benutzer stattdessen wahlweise das Verwalten von Updates der Komponenten durch Site Recovery zulassen. Wie im [Architekturverweis](azure-to-azure-architecture.md) für Azure-zu-Azure-Notfallwiederherstellung beschrieben, wird beim Replizieren virtueller Computer von einer Azure-Region zu einer anderen Mobility Service auf allen Azure-VMs installiert. Dieses Dokument erläutert Folgendes:

- Wie funktioniert das automatische Update?
- Aktivieren automatischer Updates
- Allgemeine Probleme und Problembehandlung
 
## <a name="how-does-automatic-update-work"></a>Wie funktioniert das automatische Update?

Sobald Sie das Verwalten von Updates durch Site Recovery zulassen, wird ein globales Runbook (das von Azure-Diensten verwendet wird) über ein Automatisierungskonto bereitgestellt, das im selben Abonnement wie der Tresor erstellt wird. Ein Automatisierungskonto wird für einen bestimmten Tresor verwendet. Das Runbook überprüft für jeden virtuellen Computer in einem Tresor, welche automatischen Updates eingeschaltet sind, und initiiert ein Upgrade der Mobility Service-Erweiterung, wenn eine neuere Version verfügbar ist. Der Standardzeitplan für das Runbook wird täglich um 12:00 Uhr gemäß der Zeitzone des geografischen Gebiets der replizierten VM ausgeführt. Der Runbook-Zeitplan kann auch ggf. über das Automatisierungskonto durch den Benutzer geändert werden. 

## <a name="enable-automatic-updates"></a>Aktivieren automatischer Updates

Sie können das Verwalten von Updates durch Site Recovery wahlweise folgendermaßen ermöglichen:

- [Als Teil des Schritts zum Aktivieren der Replikation](#as-part-of-the-enable-replication-step)
- [Durch Ein-/Ausschalten der Erweiterungsupdateeinstellungen im Tresor](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Als Teil des Schritts zum Aktivieren der Replikation:

Wenn Sie die Replikation für einen virtuellen Computer aktiveren, indem Sie entweder [von der Ansicht der VM](azure-to-azure-quickstart.md) oder [vom Recovery Services-Tresor](azure-to-azure-how-to-enable-replication.md) aus starten, können Sie optional auswählen, entweder das Verwalten von Updates für die Site Recovery-Erweiterung durch Site Recovery zuzulassen, oder diese manuell zu verwalten.

![enable-replication-auto-update](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Durch Ein-/Ausschalten der Erweiterungsupdateeinstellungen im Tresor

1. Navigieren Sie im Tresor zu **Verwalten**-> **Site Recovery-Infrastruktur**.
2. Klicken Sie unter **Für Azure-VMs**-> **Erweiterungsupdateeinstellungen** auf die Umschaltfläche, um Ihre Auswahl zwischen *Verwalten von Updates durch ASR zulassen* oder *Manuell verwalten* zu treffen. Klicken Sie auf **Speichern**.

![vault-toggle-autuo-update](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Bei Auswahl von *Verwalten von Updates durch ASR zulassen* wird die Einstellung auf alle virtuellen Computer in dem entsprechenden Tresor angewendet.


> [!Note] 
> Bei beiden Optionen werden Sie benachrichtigt, welches Automatisierungskonto für die Verwaltung der Updates verwendet wird. Wenn Sie dieses Feature zum ersten Mal in einem Tresor aktivieren, wird ein neues Automatisierungskonto erstellt. Alle nachfolgend im selben Tresor aktivierten Replikationen verwenden das zuvor erstellte.

## <a name="common-issues--troubleshooting"></a>Allgemeine Probleme und Problembehandlung

Wenn ein Problem mit den automatischen Updates auftritt, werden Sie unter „Konfigurationsprobleme“ im Tresordashboard benachrichtigt. 

Falls Sie vergeblich versucht haben, automatische Updates zu aktivieren, lesen Sie die folgenden Informationen zur Problembehandlung.

**Fehler**: Sie sind nicht berechtigt, ein ausführendes Azure-Konto (Dienstprinzipal) zu erstellen und dem Dienstprinzipal die Rolle „Mitwirkender“ zu gewähren. 
- Empfohlene Aktion: Stellen Sie sicher, dass das angemeldete Konto dem „Mitwirkenden“ zugewiesen ist, und wiederholen Sie den Vorgang.
 
Wenn automatische Updates eingeschaltet sind, können die meisten Probleme durch den Site Recovery-Dienst bereinigt werden, indem Sie auf die Schaltfläche **Reparatur** klicken.

![repair-button](./media/azure-to-azure-autoupdate/repair.png)

Für den Fall, dass die Schaltfläche „Reparieren“ nicht verfügbar ist, beachten Sie die Fehlermeldung, die unter dem Bereich für „Erweiterungseinstellungen“ angezeigt wird.

 - **Fehler**: Das ausführende Konto verfügt nicht über die Berechtigung zum Zugriff auf die Recovery Services-Ressource.

    **Empfohlene Aktion**: Löschen und anschließendes [Neuerstellen des ausführenden Kontos](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account), oder stellen Sie sicher, dass das ausführende Automatisierungskonto der Azure Active Directory-Anwendung Zugriff auf die Recovery Services-Ressource hat.

- **Fehler**: Ausführendes Konto wurde nicht gefunden. Eine der folgenden Komponenten wurde gelöscht oder nicht erstellt – Azure Active Directory-Anwendung, Dienstprinzipal, Rolle, Automation-Zertifikatasset, Automation-Verbindungsasset – oder der Fingerabdruck im Zertifikat ist nicht identisch mit dem der Verbindung. 

    **Empfohlene Aktion**: Löschen und [Neuerstellen des ausführenden Kontos](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account).
