---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: dfb76a14f7e177211e5e8891005544e20f19d3f3
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638442"
---
Die Rechnungsstellung für Azure Storage basiert auf der Nutzung Ihres Speicherkontos. Alle Objekte in einem Speicherkonto werden zusammen als Gruppe abgerechnet. 

Speicherkosten werden gemäß den folgenden Faktoren berechnet: Region/Standort, Kontotyp, Zugriffsebene, Speicherkapazität, Replikationsschema, Speichertransaktionen und Datenausgang.

* **Region** bezieht sich auf die geografische Region, in der sich Ihr Konto befindet.
* **Kontotyp** verweist auf den Typ des Speicherkontos, das Sie verwenden. 
* **Zugriffsebene** bezieht sich auf das Datenverwendungsmuster, das Sie für Ihr Konto vom Typ „general-purpose v2“ oder Ihr Blobspeicherkonto angegeben haben.
* **Speicherkapazität** bezieht sich darauf, wie viel von Ihrer Speicherkontozuweisung zum Speichern von Daten verwendet wird.
* Die **Replikation** bestimmt, wie viele Kopien Ihrer Daten jeweils an welchen Standorten unterhalten werden.
* **Transaktionen** beziehen sich auf alle Lese- und Schreibvorgänge in Azure Storage.
* **Datenausgang** bezieht sich alle Daten, die aus einer Azure-Region übertragen werden. Wenn eine Anwendung, die nicht in der gleichen Region ausgeführt wird, auf die Daten in Ihrem Speicherkonto zugreift, fallen Gebühren für den Datenausgang an. Weitere Informationen zur Verwendung von Ressourcengruppen zum Gruppieren von Daten und Diensten in der gleichen Region zur Beschränkung vom Gebühren für den Datenausgang, finden Sie unter [Was ist eine Azure-Ressourcengruppe?](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/azure-resource-access#what-is-an-azure-resource-group). 

Die Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) bietet detaillierte Preisinformationen basierend auf Kontotyp, Speicherkapazität, Replikation und Transaktionen. In der [Datenübertragungs-Preisübersicht](https://azure.microsoft.com/pricing/details/data-transfers/) finden Sie detaillierte Preisinformationen für den Datenausgang. Sie können den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) verwenden, um Ihre Kosten zu bestimmen.

