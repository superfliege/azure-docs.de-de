---
title: Einführung in Azure-Warteschlangen | Microsoft-Dokumentation
description: Einführung in Azure-Warteschlangen
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/06/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 2ae0d3993df54e1c9e5a9bf93619e8f9faa8a917
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873596"
---
# <a name="what-are-azure-queues"></a>Was sind Azure-Warteschlangen?

Die Warteschlangenspeicherung in Azure ist ein Dienst zur Speicherung großer Anzahlen von Nachrichten, auf die von überall auf der Welt mit authentifizierten Anrufen über HTTP oder HTTPS zugegriffen werden kann. Eine einzelne Warteschlangennachricht kann bis zu 64 KB groß sein, und eine Warteschlange kann Millionen von Nachrichten enthalten. Deren Anzahl ist nur durch die Kapazität des Speicherkontos begrenzt.

## <a name="common-uses"></a>Gängige Nutzungsszenarien

Warteschlangenspeicherungen werden hauptsächlich für folgende Zwecke verwendet:

* Erstellung eines Arbeits-Backlogs zur asynchronen Verarbeitung
* Weiterleitung von Nachrichten von einer Azure-Webrolle an eine Azure-Workerrolle

## <a name="queue-service-concepts"></a>Konzepte des Warteschlangendiensts

Der Warteschlangendienst umfasst die folgenden Komponenten:

![Konzepte des Warteschlangendiensts](./media/storage-queues-introduction/queue1.png)

* **URL-Format:** Warteschlangen können über das folgende URL-Format aufgerufen werden:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Mit der folgenden URL kann eine der Warteschlangen im Diagramm adressiert werden:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Speicherkonto:** Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Weitere Informationen zur Kapazität der Speicherkonten finden Sie unter [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Skalierbarkeits- und Leistungsziele für Microsoft Azure-Speicher, in englischer Sprache).

* **Warteschlange:** Eine Warteschlange enthält einen Satz von Nachrichten. Alle Nachrichten müssen sich in Warteschlangen befinden. Beachten Sie, dass der Warteschlangenname nur aus Kleinbuchstaben bestehen darf. Informationen zum Benennen von Warteschlangen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Nachricht:** Eine Nachricht in einem beliebigen Format und mit einer Größe von bis zu 64 KB. Eine Nachricht kann maximal sieben Tage in der Warteschlange verbleiben.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Speicherkontos](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
