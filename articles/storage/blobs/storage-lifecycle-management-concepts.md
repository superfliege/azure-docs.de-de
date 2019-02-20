---
title: Verwalten des Azure Storage-Lebenszyklus
description: Erfahren Sie, wie Sie Regeln für Lebenszyklusrichtlinien erstellen, um alternde Daten von heißen zu kalten und zu Archivebenen zu übertragen.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/04/2018
ms.author: yzheng
ms.subservice: common
ms.openlocfilehash: 284a590a484052fdb7da2f03c6155078268b2aac
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211443"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Verwalten des Azure Blob Storage-Lebenszyklus (Vorschau)

Datasets haben eindeutige Lebenszyklen. Früh im Lebenszyklus greifen Benutzer häufig auf einige Daten zu. Aber der Zugriffsbedarf sinkt mit zunehmendem Alter der Daten drastisch. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur selten zugegriffen wird. Einige Daten laufen Tage oder Monate nach der Erstellung ab, während andere Daten im Verlauf ihres gesamten Lebens aktiv gelesen und geändert werden. Die Azure Blob Storage-Lebenszyklusverwaltung (Vorschau) bietet eine umfassende, regelbasierte Richtlinie für GPv2- und Blob Storage-Konten. Verwenden Sie die Richtlinie, um Ihre Daten in die entsprechenden Zugriffsebenen zu übertragen oder am Ende des Lebenszyklus der Daten ablaufen zu lassen.

Mit der Richtlinie für die Lebenszyklusverwaltung können Sie die folgenden Aufgaben ausführen:

- Überführen von Blobs in eine kältere Speicherebene (heiß in kalt, heiß in Archiv oder kalt in Archiv) zur Optimierung von Leistung und Kosten
- Löschen von Blobs am Ende ihres Lebenszyklus
- Definieren von Regeln, die ein Mal täglich auf Speicherkontoebene ausgeführt werden
- Anwenden von Regeln auf Container oder eine Teilmenge von Blobs (durch Einsatz von Präfixen als Filter)

Betrachten Sie das Szenario, bei dem ein Dataset in den frühen Phasen seines Lebenszyklus häufig aufgerufen wird, nach zwei Wochen aber nur noch gelegentlich. Nach dem ersten Monat wird auf das Dataset nur noch selten zugegriffen. In diesem Szenario empfiehlt sich in den frühen Phasen heißer Speicher. Die kalte Speicherebene ist am besten für den gelegentlichen Zugriff geeignet und Archivspeicher die beste Wahl für die Ebene, nachdem die Daten das Alter von einem Monat überschritten haben. Durch Anpassen der Speicherebenen im Hinblick auf das Alter der Daten können Sie die kostengünstigsten Speicheroptionen für Ihre Anforderungen entwerfen. Für diesen Übergang stehen Richtlinienregeln für die Lebenszyklusverwaltung zur Verfügung, um alternde Daten in kühlere Ebenen zu verschieben.

## <a name="storage-account-support"></a>Speicherkontounterstützung

Die Richtlinien zur Lebenszyklusverwaltung sind sowohl für GPv2-Konten (General Purpose v2) als auch für Blob Storage-Konten verfügbar. Für ein vorhandenes GPv1-Konto (General Purpose v1) kann in einem einfachen Prozess im Azure-Portal mit einem Mausklick ein Upgrade in ein GPv2-Konto erfolgen. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).  

## <a name="pricing"></a>Preise 

Die Funktion zur Lebenszyklusverwaltung ist in der Vorschau kostenlos. Kunden werden die gewöhnlichen Betriebskosten für die API-Aufrufe [Blobs auflisten](https://docs.microsoft.com/rest/api/storageservices/list-blobs) und [Blobtarif festlegen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) in Rechnung gestellt. Weitere Informationen zu den Preisen finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Registrieren für die Vorschau 
Um sich für die öffentliche Vorschau zu registrieren, müssen Sie eine Anforderung einreichen, damit diese Funktion für Ihr Abonnement registriert werden kann. Anforderungen werden in der Regel innerhalb von 72 Stunden genehmigt. Nach der Genehmigung beinhalten alle vorhandenen und neuen GPv2- oder Blob Storage-Konten in den folgenden Regionen die Funktion: USA, Westen 2, USA, Westen-Mitte, USA, Osten 2 und Europa, Westen. Die Vorschau unterstützt nur Blockblobs. Wie bei den meisten Vorschauversionen sollte diese Funktion bis zum Erreichen der allgemeinen Verfügbarkeit nicht für Produktionsworkloads verwendet werden.

Um eine Anforderung zu senden, führen Sie die folgenden PowerShell- oder CLI-Befehle aus.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

So senden Sie eine Anforderung:

```powershell
Register-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Sie können den Genehmigungsstatus der Anforderung mit dem folgenden Befehl überprüfen:
```powershell
Get-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Mit der Genehmigung und der ordnungsgemäßen Registrierung erhalten Sie den Status *Registriert*, wenn Sie die vorherigen Anforderungen übermitteln.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

So senden Sie eine Anforderung: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Sie können den Genehmigungsstatus der Anforderung mit dem folgenden Befehl überprüfen:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Mit der Genehmigung und der ordnungsgemäßen Registrierung erhalten Sie den Status *Registriert*, wenn Sie die vorherigen Anforderungen übermitteln.


## <a name="add-or-remove-a-policy"></a>Hinzufügen oder Entfernen einer Richtlinie 

Sie können eine Richtlinie über das Azure-Portal bzw. mit [PowerShell](https://www.powershellgallery.com/packages/Az.Storage), der [Azure CLI](https://docs.microsoft.com/cli/azure/ext/storage-preview/storage/account/management-policy?view=azure-cli-latest#ext-storage-preview-az-storage-account-management-policy-create), [REST-APIs](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate) oder Clienttools in den folgenden Sprachen hinzufügen, bearbeiten oder entfernen: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0) und [Ruby](https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie die Option **Alle Ressourcen** und dann Ihr Speicherkonto aus.

3. Wählen Sie **Lebenszyklusverwaltung** unter der Gruppe „Blobdienst“ aus, um Ihre Richtlinie anzuzeigen oder zu ändern.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```
az account set --subscription "[subscriptionName]”
az extension add --name storage-preview
az storage account management-policy show --resource-group [resourceGroupName] --account-name [accountName]
```

> [!NOTE]
Wenn Sie Firewallregeln für Ihr Speicherkonto aktivieren, werden Anforderungen für die Lebenszyklusverwaltung möglicherweise blockiert. Sie können die Sperre dieser Anforderungen durch Bereitstellen von Ausnahmen aufheben. Weitere Informationen finden Sie im Abschnitt „Ausnahmen“ unter [Konfigurieren von Firewalls und virtuellen Netzwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>Richtlinie

Eine Richtlinie zur Lebenszyklusverwaltung ist eine Sammlung von Regeln in einem JSON-Dokument:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Eine Richtlinie erfordert zwei Parameter:

| Parametername | Parametertyp | Notizen |
|----------------|----------------|-------|
| Version        | Eine Zeichenfolge, die als `x.x` ausgedrückt ist. | Die Versionsnummer der Vorschau ist 0.5. |
| Regeln          | Ein Array von Regelobjekten | Jede Richtlinie muss mindestens eine Regel enthalten. In der Vorschauphase können Sie bis zu vier Regeln pro Richtlinie angeben. |

Jede Regel in der Richtlinie erfordert drei Parameter:

| Parametername | Parametertyp | Notizen |
|----------------|----------------|-------|
| NAME           | Zeichenfolge | Ein Regelname kann aus einer beliebigen Kombination von alphanumerischen Zeichen bestehen. Bei Regelnamen wird die Groß-/Kleinschreibung unterschieden. Er muss innerhalb einer Richtlinie eindeutig sein. |
| type           | Ein Enumerationswert | Der gültige Wert für die Vorschau ist `Lifecycle`. |
| Definition     | Ein Objekt, das die Lebenszyklusregel definiert | Jede Definition beinhaltet einen Filtersatz und einen Aktionssatz. |

## <a name="rules"></a>Regeln

Jede Regeldefinition enthält einen Filtersatz und einen Aktionssatz. Der [Filtersatz](#rule-filters) schränkt Regelaktionen auf eine bestimmte Menge von Objekten innerhalb eines Containers oder auf Objektnamen ein. Der [Aktionssatz](#rule-actions) wendet die Ebenen- oder Löschaktionen auf den gefilterten Satz von Objekten an.

### <a name="sample-rule"></a>Beispielregel
Die folgende Beispielregel filtert das Konto so, dass Aktionen nur auf `container1/foo` ausgeführt werden. Führen Sie für alle Objekte, die sich innerhalb von `container1` befinden **UND** mit `foo` beginnen, diese Aktionen aus: 

- Blob 30 Tage nach der letzten Änderung in die kalte Ebene verschieben
- Blob 90 Tage nach der letzten Änderung in die Archivebene verschieben
- Blob 2.555 Tage (sieben Jahre) nach der letzten Änderung löschen
- Blob-Momentaufnahmen 90 Tage nach der Erstellung der Momentaufnahmen löschen

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Regelfilter

Filter schränken Regelaktionen auf eine Teilmenge der Blobs innerhalb des Speicherkontos ein. Wenn mehrere Filter definiert sind, wird ein logisches `AND` für alle Filter ausgeführt.

Diese Filter sind in der Vorschauphase gültig:

| Filtername | Filtertyp | Notizen | Ist erforderlich |
|-------------|-------------|-------|-------------|
| blobTypes   | Ein Array von vordefinierten Enumerationswerten. | In der Vorschauversion wird nur `blockBlob` unterstützt. | Ja |
| prefixMatch | Ein Array von Zeichenfolgen für Präfixe, mit denen Übereinstimmung erzielt werden soll. Eine Präfixzeichenfolge muss mit einem Containernamen beginnen. Wenn Sie beispielsweise alle Blobs unter „https://myaccount.blob.core.windows.net/container1/foo/...“ für eine Regel zuordnen möchten, lautet der prefixMatch-Wert `container1/foo`. | Wenn Sie prefixMatch nicht definieren, gelten die Regeln für alle Blobs im Konto. | Nein  |

### <a name="rule-actions"></a>Regelaktionen

Aktionen werden auf die gefilterten Blobs angewendet, wenn die Ausführungsbedingung erfüllt ist.

In der Vorschauversion unterstützt die Lebenszyklusverwaltung Ebenenverschiebung und Löschen von Blobs sowie Löschen von Blobmomentaufnahmen. Definieren Sie mindestens eine Aktion für jede Regel für Blobs oder Blobmomentaufnahmen.

| Aktion        | Basisblob                                   | Momentaufnahme      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Unterstützt Blobs, die sich aktuell in der heißen Ebene befinden.         | Nicht unterstützt |
| tierToArchive | Unterstützt Blobs, die sich aktuell in der heißen oder der kalten Ebene befinden. | Nicht unterstützt |
| delete        | Unterstützt                                   | Unterstützt     |

>[!NOTE] 
Wenn für das gleiche Blob mehrere Aktionen definiert sind, wendet die Lebenszyklusverwaltung die am wenigsten teure Aktion auf das Blob an. Beispielsweise ist die Aktion `delete` kostengünstiger als die Aktion `tierToArchive`. Die Aktion `tierToArchive` ist kostengünstiger als die Option `tierToCool`.

In der Vorschau basieren die Ausführungsbedingungen für Aktionen auf dem Alter. Basisblobs verwenden den Zeitpunkt der letzten Änderung, um das Alter nachzuverfolgen, während Blobmomentaufnahmen für den gleichen Zweck den Erstellungszeitpunkt der Momentaufnahme verwenden.

| Aktionsausführungsbedingung | Wert der Bedingung | BESCHREIBUNG |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Ganzzahliger Wert, der das Alter in Tagen angibt | Gültige Bedingung für Basisblobaktionen |
| daysAfterCreationGreaterThan     | Ganzzahliger Wert, der das Alter in Tagen angibt | Gültige Bedingung für Aktionen für Blobmomentaufnahmen | 

## <a name="examples"></a>Beispiele
Die folgenden Beispiele veranschaulichen die Behandlung von gängigen Szenarien mithilfe von Regeln zur Lebenszyklusverwaltung.

### <a name="move-aging-data-to-a-cooler-tier"></a>Verschieben von alternden Daten auf eine kühlere Ebene

Dieses Beispiel zeigt den Übergang von Blockblobs mit dem Präfix `container1/foo` oder `container2/bar`. Die Richtlinie überführt Blobs, die mehr als 30 Tage nicht verändert wurden, in den kalten Speicher und Blobs, die in 90 Tagen nicht verändert wurden, in die Archivebene:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Archivieren von Daten bei der Erfassung 

Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird. Archivieren Sie diese Daten sofort nach der Erfassung. Die folgende Lebenszyklusrichtlinie ist so konfiguriert, dass Daten nach der Erfassung archiviert werden. In diesem Beispiel werden Blockblobs im Speicherkonto in Container `archivecontainer` sofort an eine Archivebene überführt. Die unmittelbare Umstellung wird durch die Ausführung der Aktion für Blobs 0 Tage nach dem Zeitpunkt der letzten Änderung erreicht:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Ablauf von Daten nach dem Alter

Es wird erwartet, dass einige Daten Tage oder Monate nach der Erstellung ablaufen, um Kosten zu senken oder gesetzliche Vorschriften zu erfüllen. Sie können eine Richtlinie zur Lebenszyklusverwaltung so einrichten, dass Daten durch Löschung auf der Grundlage ihres Alters ablaufen. Das folgende Beispiel zeigt eine Richtlinie, die alle Blockblobs (für die kein Präfix angegeben ist) löscht, die älter als 365 Tage sind.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Löschen von alten Momentaufnahmen

Bei Daten, die regelmäßig geändert und auf die während ihrer Lebensdauer regelmäßig zugegriffen wird, werden Momentaufnahmen häufig verwendet, um ältere Versionen der Daten nachzuverfolgen. Sie können eine Richtlinie erstellen, die alte Momentaufnahmen auf der Grundlage des Alters der Momentaufnahme löscht. Das Alter der Momentaufnahme wird durch Auswertung der Erstellungszeit der Momentaufnahme bestimmt. Diese Richtlinie löscht Blockblob-Momentaufnahmen in Container `activedata`, die mindestens 90 Tage alt sind (ab dem Zeitpunkt der Erstellung der Momentaufnahme).

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq---i-created-a-new-policy-why-are-the-actions-not-run-immediately"></a>FAQ: Ich habe eine neue Richtlinie erstellt. Warum werden die Aktionen nicht sofort ausgeführt? 

Die Plattform führt die Lebenszyklusrichtlinie ein Mal täglich aus. Sobald Sie eine neue Richtlinie festgelegt haben, kann es bis zu 24 Stunden dauern, bis einige Aktionen (z.B. Ebenenverschiebung und Löschen) gestartet und ausgeführt werden.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Daten nach versehentlichem Löschen wiederhergestellt werden:

- [Vorläufiges Löschen für Azure Storage-Blobs ](../blobs/storage-blob-soft-delete.md)
