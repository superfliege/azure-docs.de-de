---
title: Verwalten des Azure Storage-Lebenszyklus
description: Erfahren Sie, wie Sie Regeln für Lebenszyklusrichtlinien erstellen können, um den Übergang von alternden Daten von heißen zu kalten und zu Archivebenen zu steuern.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: 25e6fba6ac8aa34c0c30fd61f5fe297b94720439
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983666"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Verwalten des Azure Blob Storage-Lebenszyklus (Vorschau)

Datasets haben eindeutige Lebenszyklen. Auf manche Daten wird früh in ihrem Lebenszyklus häufig zugegriffen, aber die Notwendigkeit für den Zugriff sinkt mit zunehmendem Alter der Daten dramatisch. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur selten zugegriffen wird. Einige Daten laufen Tage oder Monate nach der Erstellung ab, während andere Daten im Verlauf ihres gesamten Lebens aktiv gelesen und geändert werden. Die Azure Blob Storage-Lebenszyklusverwaltung (Vorschau) bietet eine umfassende, regelbasierte Richtlinie, mit deren Hilfe Sie den Übergang Ihrer Daten in die optimale Zugriffsebene und den Ablauf der Daten am Ende ihres Lebenszyklus umsetzen können.

Die Richtlinie zur Lebenszyklusverwaltung unterstützt Sie in diesen Punkten:

- Übergang von Blobs in eine kühlere Speicherebene (Heiß nach kalt, heiß nach Archiv oder kalt nach Archiv), um Leistung und Kosten zu optimieren
- Löschen von Blobs am Ende ihres Lebenszyklus
- Definieren von Regeln für die Ausführung einmal täglich auf Speicherkontoebene (sowohl GPv2- als auch Blob-Speicherkonten werden unterstützt)
- Anwenden von Regeln auf Container oder eine Teilmenge von Blobs (durch Einsatz von Präfixen als Filter)

Betrachten Sie eine Menge von Daten, auf die in der Frühphase des Lebenszyklus häufig zugegriffen wird, die nach zwei Wochen nur noch gelegentlich benötigt wird und auf die nach einem Monat oder später nur noch selten zugegriffen wird. In diesem Szenario ist die heiße Speicherebene in den frühen Phasen optimal, die kalte Speicherebene am besten für den gelegentlichen Zugriff geeignet und Archivspeicher die beste Wahl für die Ebene, nachdem die Daten das Alter von einem Monat überschritten haben. Durch Anpassen der Speicherebenen im Hinblick auf das Alter der Daten können Sie die kostengünstigsten Speicheroptionen für Ihre Anforderungen entwerfen. Um diesen Übergang zu vollziehen, stehen Richtlinien zur Lebenszyklusverwaltung zur Verfügung, um alternde Daten in kühlere Ebenen zu verschieben.

## <a name="storage-account-support"></a>Speicherkontounterstützung

Richtlinien zur Lebenszyklusverwaltung sind sowohl für das GPv2-Konto (General Purpose v2) als auch für das Blob-Speicherkonto erhältlich. Ein vorhandenes GPv1-Konto (General Purpose v1) kann in einem einfachen Prozess im Azure-Portal mit einem Mausklick in ein GPv2-Konto konvertiert werden. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).  

## <a name="pricing"></a>Preise 

Die Funktion zur Lebenszyklusverwaltung ist in der Vorschau kostenlos. Kunden werden die gewöhnlichen Betriebskosten für die API-Aufrufe [Blobs auflisten](https://docs.microsoft.com/rest/api/storageservices/list-blobs) und [Blobtarif festlegen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) in Rechnung gestellt. Weitere Informationen zu Preisen finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Registrieren für die Vorschau 
Um sich für die öffentliche Vorschau zu registrieren, müssen Sie eine Anforderung einreichen, diese Funktion für Ihr Abonnement zu registrieren. Nachdem Ihre Anforderung genehmigt wurde (innerhalb einiger Tage), wird die Funktion für alle vorhandenen und neuen GPv2- oder Blob Storage-Konten in „USA, Westen 2“ und „Europa, Westen“ aktiviert. Während der Vorschau wird nur Blockblob unterstützt. Wie bei den meisten Vorschauversionen sollte diese Funktion bis zum Erreichen der allgemeinen Verfügbarkeit nicht für Produktionsworkloads verwendet werden.

Um eine Anforderung zu senden, führen Sie die folgenden PowerShell- oder CLI-Befehle aus.

### <a name="powershell"></a>PowerShell

So senden Sie eine Anforderung:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Sie können den Genehmigungsstatus der Anforderung mit dem folgenden Befehl überprüfen:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Wenn die Funktion genehmigt und ordnungsgemäß registriert wurde, sollte der Status „Registriert“ zurückgegeben werden.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

So senden Sie eine Anforderung: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Sie können den Genehmigungsstatus der Anforderung mit dem folgenden Befehl überprüfen:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Wenn die Funktion genehmigt und ordnungsgemäß registriert wurde, sollte der Status „Registriert“ zurückgegeben werden. 


## <a name="add-or-remove-policies"></a>Hinzufügen oder Entfernen von Richtlinien 

Sie können eine Richtlinie über das Azure-Portal bzw. mit [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [REST-APIs](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/createorupdatemanagementpolicies) oder Clienttools in den folgenden Sprachen hinzufügen, bearbeiten oder entfernen: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0) und [Ruby](   https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Speicherkonto, wählen Sie „Alle Ressourcen“, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Lebenszyklusverwaltung**, die unter „Blob-Dienst“ gruppiert ist, um Richtlinien anzuzeigen und/oder zu ändern.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Wenn Sie Firewallregeln für Ihr Speicherkonto aktivieren, werden Anforderungen für die Lebenszyklusverwaltung möglicherweise blockiert. Sie können die Sperre durch Bereitstellen von Ausnahmen aufheben. Weitere Informationen finden Sie im Abschnitt „Ausnahmen“ unter [Konfigurieren von Firewalls und virtuellen Netzwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>Richtlinien

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


In einer Richtlinie sind zwei Parameter erforderlich:

| Parametername | Parametertyp | Notizen |
|----------------|----------------|-------|
| Version        | Eine Zeichenfolge, die als `x.x` ausgedrückt ist. | Die Versionsnummer der Vorschau ist 0.5. |
| Regeln          | Ein Array von Regelobjekten | Jede Richtlinie muss mindestens eine Regel enthalten. In der Vorschauphase können Sie bis zu vier Regeln pro Richtlinie angeben. |

Parameter, die innerhalb einer Regel erforderlich sind:

| Parametername | Parametertyp | Notizen |
|----------------|----------------|-------|
| NAME           | Zeichenfolge | Ein Regelname kann aus einer beliebigen Kombination von alphanumerischen Zeichen bestehen. Bei Regelnamen wird die Groß-/Kleinschreibung unterschieden. Er muss innerhalb einer Richtlinie eindeutig sein. |
| type           | Ein Enumerationswert | Der gültige Wert für die Vorschau ist `Lifecycle` |
| Definition     | Ein Objekt, das die Lebenszyklusregel definiert | Jede Definition beinhaltet einen Filtersatz und einen Aktionssatz. |

## <a name="rules"></a>Regeln

Jede Regeldefinition enthält einen Filtersatz und einen Aktionssatz. Die folgende Beispielregel ändert die Ebene für Basis-Blockblobs mit dem Präfix `container1/foo`. In der Richtlinie sind diese Regeln wie folgt definiert:

- Blob 30 Tage nach der letzten Änderung in die kalte Speicherebene verschieben
- Blob 90 Tage nach der letzten Änderung in die Archivspeicherebene verschieben
- Blob 2.555 Tage (7 Jahre) nach der letzten Änderung löschen
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

Filter schränken Regelaktionen auf eine Teilmenge der Blobs innerhalb des Speicherkontos ein. Wenn mehrere Filter definiert sind, wird ein logisches `AND` auf alle Filter angewendet.

Diese Filter sind in der Vorschauphase gültig:

| Filtername | Filtertyp | Notizen | Ist erforderlich |
|-------------|-------------|-------|-------------|
| blobTypes   | Ein Array von vordefinierten Enumerationswerten. | In der Vorschauversion wird nur `blockBlob` unterstützt. | JA |
| prefixMatch | Ein Array von Zeichenfolgen für Präfixe, mit denen Übereinstimmung erzielt werden soll. Eine Präfixzeichenfolge muss mit einem Containernamen beginnen. Wenn beispielsweise alle Blobs unter https://myaccount.blob.core.windows.net/mycontainer/mydir/.. für eine Regel gefunden werden, ist das Präfix „mycontainer/mydir“. | Wenn es nicht definiert ist, gilt diese Regel für alle Blobs im Konto. | Nein  |

### <a name="rule-actions"></a>Regelaktionen

Aktionen werden auf die gefilterten Blobs angewendet, wenn die Ausführungsbedingung erfüllt ist.

In der Vorschauversion unterstützt die Lebenszyklusverwaltung Tearing und Löschen von Blobs und Löschen von Blobmomentaufnahmen. In jeder Regel muss mindestens eine Aktion für Blobs oder Blobmomentaufnahmen definiert sein.

| Aktion        | Basisblob                                   | Momentaufnahme      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Unterstützt Blobs, die sich aktuell in der heißen Ebene befinden         | Nicht unterstützt |
| tierToArchive | Unterstützt Blobs, die sich aktuell in der heißen oder der kalten Ebene befinden | Nicht unterstützt |
| delete        | Unterstützt                                   | Unterstützt     |

>[!NOTE] 
Wenn für das gleiche Blob mehr als eine Aktion definiert ist, wendet die Lebenszyklusverwaltung die am wenigsten teure Aktion auf das Blob an. (beispielsweise ist die Aktion `delete` billiger als die Option `tierToArchive`. Die Aktion `tierToArchive` ist billiger als die Option `tierToCool`.)

In der Vorschau basieren die Ausführungsbedingungen für Aktionen auf dem Alter. Das Basisblob verwendet den Zeitpunkt der letzten Änderung, um das Alter nachzuverfolgen, während Blobmomentaufnahmen für den gleichen Zweck den Erstellungszeitpunkt der Momentaufnahme verwenden.

| Aktionsausführungsbedingung | Wert der Bedingung | BESCHREIBUNG |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Ganzzahliger Wert, der das Alter in Tagen angibt | Gültige Bedingung für Basisblobaktionen |
| daysAfterCreationGreaterThan     | Ganzzahliger Wert, der das Alter in Tagen angibt | Gültige Bedingung für Aktionen für Blobmomentaufnahmen | 

## <a name="examples"></a>Beispiele
Die folgenden Beispiele veranschaulichen die Behandlung von gängigen Szenarien mithilfe von Regeln zur Lebenszyklusverwaltung.

### <a name="move-aging-data-to-a-cooler-tier"></a>Verschieben von alternden Daten auf eine kühlere Ebene

Das folgende Beispiel zeigt den Übergang von Blockblobs mit dem Präfix `container1/foo` oder `container2/bar`. Die Richtlinie überführt Blobs, die mehr als 30 Tage nicht verändert wurden, in den kalten Speicher und Blobs, die in 90 Tagen nicht verändert wurden, in die Archivebene:

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

Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird. Diese Daten sollten idealerweise unmittelbar nach der Erfassung archiviert werden. Die folgende Lebenszyklusrichtlinie ist so konfiguriert, dass Daten nach der Erfassung archiviert werden. In diesem Beispiel werden Blockblobs im Speicherkonto in Container `archivecontainer` sofort an eine Archivebene überführt. Die unmittelbare Umstellung wird durch die Ausführung der Aktion für Blobs 0 Tage nach dem Zeitpunkt der letzten Änderung erreicht:

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

Bei einigen Daten wird der Ablauf Tage oder Monate nach der Erstellung erwartet, um Kosten zu sparen oder gesetzlichen Bestimmungen zu genügen. Eine Richtlinie zur Lebenszyklusverwaltung kann so eingerichtet werden, dass Daten durch Löschung auf der Grundlage ihres Alters ablaufen. Das folgende Beispiel zeigt eine Richtlinie, die alle Blockblobs (für die kein Präfix angegeben ist) löscht, die älter als 365 Tage sind.

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

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Daten nach versehentlichem Löschen wiederhergestellt werden:

- [Vorläufiges Löschen für Azure Storage-Blobs ](../blobs/storage-blob-soft-delete.md)
