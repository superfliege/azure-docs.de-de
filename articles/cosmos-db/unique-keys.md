---
title: Eindeutige Schlüssel in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie eindeutige Schlüssel in Ihrer Azure Cosmos DB-Datenbank verwenden.
author: aliuy
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: 006d0ef28d82a7648a56b3bf871c5a3afd6a55a6
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624419"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Eindeutige Schlüssel in Azure Cosmos DB

Mit eindeutigen Schlüsseln können Sie einem Cosmos-Container eine zusätzliche Datenintegritätsebene hinzuzufügen. Eine Richtlinie für eindeutige Schlüssel wird beim Erstellen eines Cosmos-Containers erstellt. Mit eindeutigen Schlüsseln stellen Sie die Eindeutigkeit von Werten innerhalb einer logischen Partition sicher. (Eindeutigkeit kann pro [Partitionsschlüssel](partition-data.md) garantiert werden.) Nachdem Sie einen Container mit einer Richtlinie für eindeutige Schlüssel erstellt haben, können innerhalb einer logischen Partition keine neuen oder aktualisierten duplizierten Elemente mehr erstellt werden (gemäß Angabe der Einschränkung für eindeutige Werte). Die Kombination aus Partitionsschlüssel und eindeutigem Schlüssel garantiert die Eindeutigkeit eines Elements innerhalb des Containerbereichs.

Stellen Sie sich beispielsweise einen Cosmos-Container mit E-Mail-Adresse als Einschränkung für eindeutige Werte und `CompanyID` als Partitionsschlüssel vor. Wenn Sie die E-Mail-Adresse des Benutzers als eindeutigen Schlüssel konfigurieren, stellen Sie sicher, dass jedes Element innerhalb einer bestimmten Unternehmens-ID (`CompanyID`) über eine eindeutige e-Mail-Adresse verfügt. Es können keine zwei Elemente mit der gleichen E-Mail-Adresse und dem gleichen Partitionsschlüsselwert erstellt werden.  

Wenn Benutzer mehrere Elemente mit der gleichen E-Mail-Adresse erstellen können sollen, nicht jedoch mit der gleichen Kombination aus Vorname, Nachname und E-Mail-Adresse, können Sie der Richtlinie für eindeutige Schlüssel zusätzliche Pfade hinzufügen. Anstatt eines eindeutigen Schlüssels, der auf der E-Mail-Adresse basiert, können Sie auch einen zusammengesetzten eindeutigen Schlüssel mit einer Kombination aus Vorname, Nachname und E-Mail-Adresse erstellen. In diesem Fall ist jede eindeutige Kombination der drei Werte innerhalb einer bestimmten Unternehmens-ID (`CompanyID`) zulässig. Der Container kann also beispielsweise Elemente mit den folgenden Werten enthalten, wobei jedes Element der Einschränkung für eindeutige Schlüssel entspricht:

|CompanyID|Vorname|Nachname|E-Mail-Adresse|
|---|---|---|---|
|Contoso|Helga|Kohler|gaby@contoso.com |
|Contoso|Helga|Kohler|gaby@fabrikam.com|
|Fabrikam|Helga|Kohler|gaby@fabrikam.com|
|Fabrikam|Simon|Kohler|gaby@fabrikam.com|
|Fabrikam|   |Kohler|gaby@fabraikam.com|
|Fabrikam|   |   |gaby@fabraikam.com|

Beim Versuch, ein weiteres Element mit den in der obigen Tabelle aufgeführten Kombinationen einzufügen, erhalten Sie eine Fehlermeldung mit dem Hinweis, dass die Einschränkung für eindeutige Schlüssel nicht erfüllt ist. Die zurückgegebene Meldung lautet entweder „Ressource mit der angegebenen ID ist bereits vorhanden“ oder „Ressource mit der angegebenen ID, dem angegebenen Namen oder dem eindeutigen Index ist bereits vorhanden“.  

## <a name="defining-a-unique-key"></a>Definieren eines eindeutigen Schlüssels

Eindeutige Schlüssel können nur beim Erstellen eines Cosmos-Containers definiert werden. Ein eindeutiger Schlüssel ist auf eine logische Partition begrenzt. Wenn Sie den Container im vorherigen Beispiel auf der Grundlage der Postleitzahl partitionieren, kommt es in jeder logischen Partition zu duplizierten Elementen. Berücksichtigen Sie beim Erstellen eindeutiger Schlüssel die folgenden Eigenschaften:

* Sie können einen bereits vorhandenen Container nicht für die Verwendung eines anderen eindeutigen Schlüssels aktualisieren. Anders ausgedrückt: Nachdem ein Container mit einer Richtlinie für eindeutige Schlüssel erstellt wurde, kann die Richtlinie nicht mehr geändert werden.

* Wenn Sie einen eindeutigen Schlüssel für einen vorhandenen Container festlegen möchten, müssen Sie einen neuen Container mit der Einschränkung für eindeutige Schlüssel erstellen und mithilfe des entsprechenden Datenmigrationstools die Daten aus dem vorhandenen Container in den neuen Container verschieben. Verwenden Sie für SQL-Container das [Datenmigrationstool](import-data.md), um die Daten zu verschieben. Verwenden Sie für MongoDB-Container [„mongoimport.exe“ oder „mongorestore.exe“](mongodb-migrate.md), um die Daten zu verschieben.

* Eine Richtlinie für eindeutige Schlüssel kann maximal 16 Pfadwerte („/firstName“, „/lastName“, „/address/zipCode“ und Ähnliches) enthalten. Jede Richtlinie für eindeutige Schlüssel darf höchstens zehn Einschränkungen für eindeutige Schlüssel oder Kombinationen enthalten, und die kombinierten Pfade der jeweiligen eindeutigen Indexbedingung darf 60 Bytes nicht überschreiten. Die Kombination aus Vorname, Nachname und E-Mail-Adresse im vorherigen Beispiel ist eine einzelne Einschränkung mit drei der 16 möglichen Pfade.

* Wenn ein Container über eine Richtlinie für eindeutige Schlüssel verfügt, erhöhen sich geringfügig die Kosten für Anforderungseinheiten (Request Units, RUs) zum Erstellen, Aktualisieren und Löschen eines Elements.

* Eindeutige Schlüssel mit geringer Dichte werden nicht unterstützt. Fehlende Werte für eindeutige Pfade werden als Nullwert behandelt und als solche in die Eindeutigkeitseinschränkung einbezogen. Es kann also nur ein einzelnes Element mit Nullwert vorhanden sein, um diese Einschränkung zu erfüllen.

* Bei den Namen eindeutiger Schlüssel wird die Groß-/Kleinschreibung berücksichtigt. Stellen Sie sich beispielsweise einen Container vor, dessen Einschränkung für eindeutige Schlüssel auf „/address/zipcode“ festgelegt ist. Wenn Ihre Daten ein Feld mit dem Namen „ZipCode“ enthalten, fügt Cosmos DB „null“ als eindeutigen Schlüssel ein, da sich „zipcode“ von „ZipCode“ unterscheidet. Aufgrund der Berücksichtigung der Groß-/Kleinschreibung können die übrigen Datensätze mit „ZipCode“ nicht eingefügt werden, da der duplizierte Nullwert gegen die Einschränkung für eindeutige Schlüssel verstoßen würde.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
