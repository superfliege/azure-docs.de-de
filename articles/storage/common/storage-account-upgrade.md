---
title: Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Allgemein v2“ – Azure Storage | Microsoft-Dokumentation
description: Führen Sie ein Upgrade auf ein Speicherkonto vom Typ „Allgemein v2“ durch.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: 6e77c4836531a7efd0b52b9a411ac40ff6a613fa
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224493"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Allgemein v2“

Speicherkonten vom Typ „Allgemein v2“ unterstützen die neuesten Azure Storage-Features und umfassen die gesamte Funktionalität von Konten des Typs „Allgemein v1“ und Blob Storage-Konten. Konten vom Typ „Allgemein v2“ werden für die meisten Speicherszenarien empfohlen. Konten vom Typ „Allgemein v2“ bieten die niedrigsten Preise pro Gigabyte für Azure Storage sowie wettbewerbsfähige Transaktionspreise.

Das Durchführen eines Upgrades zu einem Speicherkonto vom Typ „Allgemein v2“ von einem Speicherkonto vom Typ „Allgemein v1“ oder einem Blobspeicherkonto ist einfach. Sie können für das Upgrade das Azure-Portal, PowerShell oder die Azure CLI verwenden. Das Durchführen eines Upgrades für ein Konto kann nicht rückgängig gemacht werden und es können Gebühren anfallen.

## <a name="upgrade-using-the-azure-portal"></a>Upgrade mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zum Speicherkonto.
3. Klicken Sie im Abschnitt **Einstellungen** auf **Konfiguration**.
4. Klicken Sie unter **Kontoart** auf **Upgrade**.
5. Geben Sie unter **Confirm upgrade** (Upgrade bestätigen) den Namen Ihres Kontos ein. 
6. Klicken Sie unten auf dem Blatt auf **Upgrade durchführen**.

## <a name="upgrade-with-powershell"></a>Aktualisieren mit PowerShell

Wenn Sie für ein Konto vom Typ „Allgemein v1“ mit PowerShell ein Upgrade auf ein Konto vom Typ „Allgemein v1“ durchführen möchten, sollten Sie zuerst PowerShell aktualisieren, damit die aktuelle Version des **AzureRm.Storage**-Moduls verwendet wird. Informationen zur Installation von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 

Rufen Sie anschließend den folgenden Befehl auf, um das Konto zu aktualisieren, und ersetzen Sie den Namen Ihrer Ressourcengruppe und des Speicherkontos:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Aktualisieren per Azure CLI

Wenn Sie für ein Konto vom Typ „Allgemein v1“ mit der Azure CLI ein Upgrade auf ein Konto vom Typ „Allgemein v1“ durchführen möchten, installieren Sie zuerst die aktuelle Version der Azure CLI. Informationen zum Installieren der CLI finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

Rufen Sie anschließend den folgenden Befehl auf, um das Konto zu aktualisieren, und ersetzen Sie den Namen Ihrer Ressourcengruppe und des Speicherkontos:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>Angeben einer Zugriffsebene für Blobdaten

Konten vom Typ „Allgemein v2“ unterstützen alle Azure Storage Services und -Datenobjekte, aber Zugriffsebenen sind nur für Blockblobs im Blobspeicher verfügbar. Wenn Sie ein Upgrade auf ein Speicherkonto vom Typ „Allgemein v2“ durchführen, können Sie eine Zugriffsebene für Ihre Blobdaten angeben. 

Zugriffsebenen ermöglichen es, den kostengünstigsten Speicher basierend auf Ihren erwarteten Nutzungsmustern auszuwählen. Blockblobs können in einer heißen, kalten oder Archivebene gespeichert werden. Weitere Informationen zu Zugriffsebenen finden Sie unter [Azure Blob Storage: Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“](../blobs/storage-blob-storage-tiers.md).

Standardmäßig wird ein neues Speicherkonto auf der Zugriffsebene „Heiß“ (Hot) erstellt, und für ein Speicherkonto vom Typ „Allgemein v1“ wird ein Upgrade auf die Zugriffsebene „Heiß“ durchgeführt. Wenn Sie untersuchen, welche Zugriffsebene nach dem Upgrade Ihrer Daten verwendet werden soll, sollten Sie Ihr Szenario berücksichtigen. Es gibt zwei typische Benutzerszenarien für die Migration auf ein Konto vom Typ „Allgemein v2“:

* Sie verfügen über ein vorhandenes Speicherkonto vom Typ „Allgemein v1“ und möchten eine Umstellung auf ein Speicherkonto vom Typ „Allgemein v2“ mit der richtigen Speicherebene für Blobdaten evaluieren.
* Sie haben sich für die Nutzung eines Speicherkontos vom Typ „Allgemein v2“ entschieden oder besitzen bereits ein Konto dieser Art und möchten evaluieren, ob Sie die Speicherebene „Heiß“ oder „Kalt“ für Blobdaten verwenden sollen.

In beiden Fällen sollten Sie zuerst die Kosten für Speicherung, Zugriff und Arbeiten mit Ihren Daten in einem Speicherkonto vom Typ „Allgemein v2“ schätzen und diesen Betrag mit Ihren derzeitigen Kosten vergleichen.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Schätzen der Kosten für Ihr aktuelles Nutzungsmuster

Zur Ermittlung der Kosten für die Speicherung und den Zugriff auf Blobdaten in einem Speicherkonto vom Typ „Allgemein v2“ auf einer bestimmten Ebene, evaluieren Sie Ihr bestehendes Nutzungsmuster oder schätzen Sie Ihr erwartetes Nutzungsmuster. Dazu benötigen Sie im Allgemeinen folgende Informationen:

* Ihr Blobspeicherverbrauch in GB, einschließlich:
    - Wie viele Daten werden im Speicherkonto gespeichert?
    - Wie ändert sich das Datenvolumen monatlich; werden alte Daten beständig durch neue Daten ersetzt?
* Das primäre Zugriffsmuster für Ihre Blobspeicherdaten, einschließlich:
    - Wie viele Daten werden aus dem Speicherkonto gelesen und in das Speicherkonto geschrieben? 
    - Wie viele Lesevorgänge finden im Vergleich zu Schreibvorgängen für die Daten im Speicherkonto statt?

Um die für Ihre Bedürfnisse am besten geeignete Zugriffsebene zu bestimmen, kann die Feststellung hilfreich sein, wie viel Kapazität Ihre Blobdaten derzeit nutzen und wie diese Daten verwendet werden. 

Sie können das Speicherkonto mit [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) überwachen, um Nutzungsdaten für Ihr Speicherkonto vor der Migration zu erfassen. Azure Monitor führt die Protokollierung durch und liefert Metrikdaten für Azure-Dienste, einschließlich Azure Storage. 

Aktivieren Sie die Kapazitätsmetriken in Azure Monitor, um die Verbrauchsdaten für Blobs in Ihrem Speicherkonto zu überwachen. Kapazitätsmetriken erfassen Daten darüber, wie viel Speicherplatz die Blobs in Ihrem Konto täglich verbrauchen. Mit Kapazitätsmetriken können die Kosten für die Speicherung von Daten auf dem Speicherkonto geschätzt werden. Weitere Informationen zur Preisgestaltung der Blobspeicherkapazität für die verschiedenen Kontoarten finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).

Aktivieren Sie Transaktionsmetriken in Azure Monitor, um Datenzugriffsmuster für die Blobspeicherung zu überwachen. Sie können nach verschiedenen Azure Storage-Vorgängen filtern, um zu schätzen, wie häufig jeder Aufruf erfolgt. Informationen zur Preisgestaltung für verschiedene Arten von Transaktionen für Block- und Anfügeblobs für die einzelnen Kontoarten finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).  

Weitere Informationen zum Sammeln von Metriken über Azure Monitor finden Sie unter [Azure Storage-Metriken in Azure Monitor](storage-metrics-in-azure-monitor.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie ein Speicherkonto](storage-quickstart-create-account.md)
- [Verwalten von Azure Storage-Konten](storage-account-manage.md)