---
title: 'Einführung in Table Storage: Objektspeicher in Azure | Microsoft-Dokumentation'
description: Speichern Sie strukturierte Daten mit Azure Table Storage, einem NoSQL-Datenspeicher, in der Cloud.
services: storage
documentationcenter: .net
author: SnehaGunda
manager: kfile
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: overview
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 044f52cd1fa42653269649b92c7a06a5f623a501
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660573"
---
# <a name="introduction-to-table-storage-in-azure"></a>Einführung in Tabellenspeicher in Azure

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Der Azure Table Storage-Dienst speichert strukturierte NoSQL-Daten in der Cloud und bietet einen Schlüssel-/Attributspeicher mit einem schemalosen Design. Aufgrund der Schemalosigkeit von Table Storage ist es einfach, Ihre Daten an die Entwicklung Ihrer Anwendungen anzupassen. Viele Arten von Anwendungen können schnell und kostengünstig auf Table Storage-Daten zugreifen, und die Kosten liegen in der Regel unter den Kosten herkömmlicher SQL-Lösungen für vergleichbare Datenmengen.

Mit Table Storage können Sie flexible Datasets wie Benutzerdaten für Webanwendungen, Adressbücher, Geräteinformationen und andere Arten von Metadaten speichern, die Ihr Dienst benötigt. Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern, und ein Speicherkonto kann eine beliebige Anzahl von Tabellen enthalten (bis zur Speicherkapazitätsgrenze eines Speicherkontos).

[!INCLUDE [storage-table-concepts-include](../../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Beim [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.

* [Getting Started with Azure Table Storage in .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) (Erste Schritte mit Azure Table Storage in .NET)

* In der Referenzdokumentation für den Tabellenspeicherdienst finden Sie alle Details zu verfügbaren APIs:

    * [Referenz zur Storage-Clientbibliothek für .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)
