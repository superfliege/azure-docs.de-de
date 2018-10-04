---
title: Integration der Quellcodeverwaltung in Azure Automation
description: In diesem Artikel wird die Integration der Quellcodeverwaltung mit GitHub in Azure Automation erläutert.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c2d13a409d095bca64da781e5c5ca58553f9710c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046252"
---
# <a name="source-control-integration-in-azure-automation"></a>Integration der Quellcodeverwaltung in Azure Automation

Mit der Quellcodeverwaltung können Sie Ihre Runbooks in Ihrem Automation-Konto in Bezug auf Ihre Skripts auf GitHub oder im Azure DevOps-Repository für die Quellcodeverwaltung aktuell halten. Sie können mit der Quellcodeverwaltung einfach mit Ihrem Team zusammenarbeiten, Änderungen nachverfolgen und Ihre Runbooks auf frühere Versionen zurücksetzen. Die Quellcodeverwaltung ermöglicht beispielsweise das Synchronisieren verschiedener Verzweigungen der Quellcodeverwaltung Ihrer Automation-Konten für Entwicklung, Test oder Produktion. Dadurch können Sie Code, der in Ihrer Entwicklungsumgebung getestet wurde, in Ihr Automation-Produktionskonto hochstufen.

Azure Automation unterstützt drei Arten von Quellcodeverwaltung:

* GitHub
* Visual Studio Team Services (Git)
* Visual Studio Team Services (TFVC)

## <a name="pre-requisites"></a>Voraussetzungen

* Ein Repository für die Quellcodeverwaltung (GitHub oder Visual Studio Team Services)
* Ein [ausführendes Konto und eine Verbindung](manage-runas-account.md)

> [!NOTE]
> Synchronisierungsaufträge für die Quellcodeverwaltung werden unter dem Automation-Konto eines Benutzers ausgeführt und mit der gleichen Rate wie andere Automation-Aufträge berechnet.

## <a name="configure-source-control"></a>Konfigurieren der Quellcodeverwaltung

Wählen Sie in Ihrem Automation-Konto die Option **Quellcodeverwaltung (Vorschau)**, und klicken Sie auf **+ Hinzufügen**.

![Auswählen der Quellcodeverwaltung](./media/source-control-integration/select-source-control.png)

Wählen Sie **Typ der Quellcodeverwaltung**, und klicken Sie auf **Authentifizieren**.

Prüfen Sie die Informationen auf der Seite mit den Berechtigungen für die App-Anforderung, und klicken Sie auf **Akzeptieren**.

Geben Sie auf der Seite **Quellcodeverwaltung – Übersicht** die Informationen ein, und klicken Sie auf **Speichern**. Die folgende Tabelle enthält eine Beschreibung der verfügbaren Felder.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Name der Quellcodeverwaltung     | Ein Anzeigename für die Quellcodeverwaltung        |
|Typ der Quellcodeverwaltung     | Der Typ der Quelle für die Quellcodeverwaltung. Die verfügbaren Optionen lauten wie folgt:</br> Github</br>Visual Studio Team Services (Git)</br>Visual Studio Team Services (TFVC)        |
|Repository     | Der Name des Repositorys oder Projekts. Er wird aus dem Repository der Quellcodeverwaltung abgerufen. Beispiel: $/ContosoFinanceTFVCExample         |
|Verzweigung     | Die Verzweigung, von der die Quelldateien abgerufen werden. Die Ausrichtung auf Verzweigungen ist für den TFVC-Quellcodeverwaltungstyp nicht verfügbar.          |
|Ordnerpfad     | Der Ordner, der die Runbooks für die Synchronisierung enthält. Beispiel: /Runbooks         |
|Automatische Synchronisierung     | Aktiviert oder deaktiviert die automatische Synchronisierung, wenn im Repository für die Quellcodeverwaltung ein Commit durchgeführt wird.         |
|Runbook veröffentlichen     | Wenn diese Option auf **Ein** festgelegt ist, werden die Runbooks automatisch veröffentlicht, nachdem sie aus der Quellcodeverwaltung synchronisiert wurden.         |
|Beschreibung     | Ein Textfeld, in dem weitere Details bereitgestellt werden können.        |

![Zusammenfassung der Quellcodeverwaltung](./media/source-control-integration/source-control-summary.png)

## <a name="syncing"></a>Synchronisierung wird durchgeführt

Wenn die automatische Synchronisierung beim Konfigurieren der Integration der Quellcodeverwaltung festgelegt wurde, wird die erste Synchronisierung automatisch gestartet. Falls die automatische Synchronisierung nicht festgelegt wurde, können Sie die Quelle in der Tabelle auf der Seite **Quellcodeverwaltung (Vorschau)** auswählen. Klicken Sie auf **Synchronisierung starten**, um den Synchronisierungsprozess zu starten.  

Sie können den Status des aktuellen Synchronisierungsauftrags oder von früheren Aufträgen anzeigen, indem Sie auf die Registerkarte **Synchronisierungsaufträge** klicken. Wählen Sie in der Dropdownliste **Quellcodeverwaltung** eine Quellcodeverwaltung aus.

![Synchronisierungsstatus](./media/source-control-integration/sync-status.png)

Sie können die Ausgabe des Auftrags anzeigen, indem Sie auf einen Auftrag klicken. Unten ist ein Beispiel für die Ausgabe eines Synchronisierungsauftrags der Quellcodeverwaltung angegeben.

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

## <a name="disconnecting-source-control"></a>Trennen der Verbindung zur Quellcodeverwaltung

Öffnen Sie zum Trennen der Verbindung mit dem Repository der Quellcodeverwaltung in Ihrem Automation-Konto unter **Kontoeinstellungen** die Option **Quellcodeverwaltung (Vorschau)**.

Wählen Sie die Quellcodeverwaltung aus, die Sie entfernen möchten. Klicken Sie auf der Seite **Quellcodeverwaltung – Übersicht** auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
