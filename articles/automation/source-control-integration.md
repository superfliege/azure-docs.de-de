---
title: Integration der Quellcodeverwaltung in Azure Automation
description: In diesem Artikel wird die Integration der Quellcodeverwaltung mit GitHub in Azure Automation erläutert.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 400cf53172fbd1ce5803cf3de298749afbf45cd4
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430193"
---
# <a name="source-control-integration-in-azure-automation"></a>Integration der Quellcodeverwaltung in Azure Automation

Mit der Quellcodeverwaltung halten Sie die Runbooks in Ihrem Automation-Konto auf demselben aktuellen Stand wie Ihre Skripts in der Quellcodeverwaltung Ihres GitHub- oder Azure DevOps-Repositorys. Sie können mit der Quellcodeverwaltung einfach mit Ihrem Team zusammenarbeiten, Änderungen nachverfolgen und Ihre Runbooks auf frühere Versionen zurücksetzen. Mit der Quellcodeverwaltung können Sie beispielsweise verschiedene Verzweigungen in der Quellcodeverwaltung mit Ihren Automation-Entwicklungs- oder Testkonten synchronisieren. So ist es einfach, Code, der in Ihrer Entwicklungsumgebung getestet wurde, in Ihr Automation-Produktionskonto höher zu stufen.

Azure Automation unterstützt drei Arten von Quellcodeverwaltung:

* GitHub
* Azure DevOps (Git)
* Azure DevOps (TFVC)

## <a name="pre-requisites"></a>Voraussetzungen

* Ein Quellcodeverwaltungsrepository (GitHub oder Azure DevOps)
* Die richtigen [Berechtigungen](#personal-access-token-permissions) für das Repository für die Quellcodeverwaltung
* Ein [ausführendes Konto und eine Verbindung](manage-runas-account.md)

> [!NOTE]
> Synchronisierungsaufträge für die Quellcodeverwaltung werden unter dem Automation-Konto eines Benutzers ausgeführt und mit der gleichen Rate wie andere Automation-Aufträge berechnet.

## <a name="configure-source-control"></a>Konfigurieren der Quellcodeverwaltung

Wählen Sie in Ihrem Automation-Konto die Option **Quellcodeverwaltung (Vorschau)**, und klicken Sie auf **+ Hinzufügen**.

![Auswählen der Quellcodeverwaltung](./media/source-control-integration/select-source-control.png)

Wählen Sie **Typ der Quellcodeverwaltung** aus, und klicken Sie auf **Authentifizieren**.

Prüfen Sie die Informationen auf der Seite mit den Berechtigungen für die App-Anforderung, und klicken Sie auf **Akzeptieren**.

Geben Sie auf der Seite **Quellcodeverwaltung – Übersicht** die Informationen ein, und klicken Sie auf **Speichern**. Die folgende Tabelle enthält eine Beschreibung der verfügbaren Felder.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Name der Quellcodeverwaltung     | Ein Anzeigename für die Quellcodeverwaltung        |
|Typ der Quellcodeverwaltung     | Der Typ der Quelle für die Quellcodeverwaltung. Die verfügbaren Optionen lauten wie folgt:</br> GitHub</br>Azure DevOps (Git)</br> Azure DevOps (TFVC)        |
|Repository     | Der Name des Repositorys oder Projekts. Dieser Wert wird aus dem Repository der Quellcodeverwaltung abgerufen. Beispiel: $/ContosoFinanceTFVCExample         |
|Verzweigung     | Die Verzweigung, von der die Quelldateien abgerufen werden. Die Ausrichtung auf Verzweigungen ist für den TFVC-Quellcodeverwaltungstyp nicht verfügbar.          |
|Ordnerpfad     | Der Ordner, der die Runbooks für die Synchronisierung enthält. Beispiel: /Runbooks         |
|Automatische Synchronisierung     | Aktiviert oder deaktiviert die automatische Synchronisierung, wenn im Repository für die Quellcodeverwaltung ein Commit durchgeführt wird.         |
|Runbook veröffentlichen     | Wenn diese Option auf **Ein** festgelegt ist, werden die Runbooks automatisch veröffentlicht, nachdem sie aus der Quellcodeverwaltung synchronisiert wurden.         |
|BESCHREIBUNG     | Ein Textfeld, in dem weitere Details bereitgestellt werden können.        |

![Zusammenfassung der Quellcodeverwaltung](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Stellen Sie sicher, dass Sie mit dem richtigen Konto angemeldet sind, wenn Sie die Quellcodeverwaltung konfigurieren. Wenn Sie diesbezüglich unsicher sind, öffnen Sie in Ihrem Browser eine neue Registerkarte, und melden Sie sich von „visualstudio.com“ oder „github.com“ ab, und stellen Sie erneut eine Verbindung mit der Quellcodeverwaltung her.

## <a name="syncing"></a>Synchronisierung wird durchgeführt

Wenn die automatische Synchronisierung beim Konfigurieren der Integration der Quellcodeverwaltung konfiguriert wird, startet die erste Synchronisierung automatisch. Falls die automatische Synchronisierung nicht festgelegt wurde, können Sie die Quelle in der Tabelle auf der Seite **Quellcodeverwaltung (Vorschau)** auswählen. Klicken Sie auf **Synchronisierung starten**, um den Synchronisierungsprozess zu starten.

Sie können den Status des aktuellen Synchronisierungsauftrags oder von früheren Aufträgen anzeigen, indem Sie auf die Registerkarte **Synchronisierungsaufträge** klicken. Wählen Sie in der Dropdownliste **Quellcodeverwaltung** eine Quellcodeverwaltung aus.

![Synchronisierungsstatus](./media/source-control-integration/sync-status.png)

Sie können die Ausgabe des Auftrags anzeigen, indem Sie auf einen Auftrag klicken. Dies ist ein Beispiel für die Ausgabe eines Synchronisierungsauftrags der Quellcodeverwaltung:

```output
========================================================================================================

Azure Automation Source Control Public Preview.
Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

Setting AzureRmEnvironment.

Getting AzureRunAsConnection.

Logging in to Azure...

Source control information for syncing:

[Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

Connecting to VSTS...


Source Control Sync Summary:


2 files synced:
 - ExampleRunbook1.ps1
 - ExampleRunbook2.ps1



========================================================================================================
```

## <a name="personal-access-token-permissions"></a>Berechtigungen für persönliche Zugriffstoken

Die Quellcodeverwaltung erfordert einige Mindestberechtigungen für persönliche Zugriffstoken. Die folgenden Tabellen enthalten die für GitHub und Azure DevOps erforderlichen Mindestberechtigungen.

### <a name="github"></a>GitHub

|Bereich  |BESCHREIBUNG  |
|---------|---------|
|**Repository**     |         |
|repo:status     | Zugriff auf den Commitstatus         |
|repo_deployment      | Zugriff auf den Bereitstellungsstatus         |
|public_repo     | Zugriff auf öffentliche Repositorys         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Schreiben von Repositoryhooks         |
|read:repo_hook|Lesen von Repositoryhooks|

### <a name="azure-devops"></a>Azure DevOps

|Bereich  |
|---------|
|Code (Lesen)     |
|Projekt und Team (Lesen)|
|Identität (Lesen)      |
|Benutzerprofil (Lesen)     |
|Arbeitselemente (Lesen)    |
|Dienstverbindungen (Lesen, Abfragen und Verwalten)<sup>1</sup>    |

<sup>1</sup> Die Berechtigung für Dienstverbindungen ist nur erforderlich, wenn die automatische Synchronisierung aktiviert ist.

## <a name="disconnecting-source-control"></a>Trennen der Verbindung zur Quellcodeverwaltung

Öffnen Sie zum Trennen der Verbindung mit dem Repository der Quellcodeverwaltung in Ihrem Automation-Konto unter **Kontoeinstellungen** die Option **Quellcodeverwaltung (Vorschau)**.

Wählen Sie die Quellcodeverwaltung aus, die Sie entfernen möchten. Klicken Sie auf der Seite **Quellcodeverwaltung – Übersicht** auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)

