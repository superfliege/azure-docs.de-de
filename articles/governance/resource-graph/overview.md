---
title: Übersicht über Azure Resource Graph
description: Azure Resource Graph ist ein Dienst in Azure, der komplexe bedarfsabhängige Abfragen von Ressourcen ermöglicht.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d68183f4d0a928ac72f3f73ea5225ad174820cb7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162099"
---
# <a name="what-is-azure-resource-graph"></a>Was ist Azure Resource Graph?

Azure Resource Graph ist ein Dienst in Azure, der die Azure-Ressourcenverwaltung mithilfe effizienter und leistungsstarker Ressourcenuntersuchung befähigen soll, alle Abonnements und Verwaltungsgruppen übergreifend nach Bedarf Abfragen durchzuführen, sodass Sie Ihre Umgebung effektiv beherrschen können. Diese Abfragen bieten die folgenden Funktionen:

- Die Fähigkeit zum Abfragen von Ressourcen mit komplexem Filtern, Gruppieren und Sortieren nach Eigenschaften der Ressource.
- Möglichkeit zum iterativen Untersuchen von Ressourcen basierend auf den Governanceanforderungen und Konvertieren des resultierenden Ausdrucks in eine Richtliniendefinition.
- Möglichkeit zum Bewerten der Auswirkungen der Anwendung von Richtlinien in einer großen Cloudumgebung.

In dieser Dokumentation wird jede Funktion im Detail besprochen.

> [!NOTE]
> Azure Resource Graph wird von der neuen Funktion zum „Alle Ressourcen“-Durchsuchen des Azure-Portals verwendet. Es dient zur Unterstützung von Kunden, die umfangreiche Umgebungen verwalten müssen.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Wie Resource Graph Azure Resource Manager ergänzt

Azure Resource Manager sendet zurzeit Daten an einen begrenzten Ressourcencache, der mehrere Ressourcenfelder verfügbar macht – genauer gesagt, Ressourcenname, ID, Typ, Ressourcengruppe, Abonnements und Speicherort. Falls Sie mit mehr Ressourceneigenschaften arbeiten möchten, müssten Sie heute noch jeden einzelnen Ressourcenanbieter anrufen und die Eigenschaftendetails für jede Ressource abfragen.

Mit Azure Resource Graph können Sie auf diese Eigenschaften zugreifen, die die Ressourcenanbieter zurückgeben, ohne jeden Ressourcenanbieter einzeln anrufen zu müssen.

## <a name="the-query-language"></a>Die Abfragesprache

Da Sie nun besser verstehen, was Azure Resource Graph ist, erfahren Sie nun, wie Sie Abfragen erstellen können.

Es ist wichtig zu wissen, dass die Abfragesprache von Azure Resource Graph auf der [Abfragesprache des Azure-Daten-Explorers](../../data-explorer/data-explorer-overview.md) basiert.

Informationen zu Vorgängen und Funktionen, die mit Azure Resource Graph verwendet werden können, finden Sie unter [Grundlegendes zur Abfragesprache von Azure Resource Graph](./concepts/query-language.md). Wie Sie Ressourcen durchsuchen, erfahren Sie unter [Untersuchen Ihrer Azure-Ressourcen mit Resource Graph](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Berechtigungen in Azure Resource Graph

Um Resource Graph verwenden zu können, müssen Sie über [Role-based Access Control, Rollenbasierte Zugriffssteuerung](../../role-based-access-control/overview.md) (RBAC) mindestens mit Lesezugriff auf die Ressourcen, die Sie abfragen möchten, autorisiert werden. Verwaltungsgruppen, Abonnements, Ressourcengruppen oder Einzelressourcen, für die Sie keine `read`-Berechtigungen besitzen, werden nicht in den Ergebnissen einer Resource Graph-Abfrage zurückgegeben.

## <a name="running-your-first-query"></a>Ausführen Ihrer ersten Abfrage

Resource Graph unterstützt die Azure-Befehlszeilenschnittstelle (CLI) und Azure PowerShell. Die Abfragekomponente ist in jeder verwendeten Sprache gleich strukturiert. Unterstützung für Azure Resource Graph ist noch nicht standardmäßig in jedem SDK verfügbar, sodass eine Erweiterung oder ein Modul geladen werden muss, um die erforderlichen Befehle bereitzustellen.
Informationen zum Aktivieren von Resource Graph in [Azure-Befehlszeilenschnittstelle](first-query-azurecli.md#add-the-resource-graph-extension) und [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Nächste Schritte

- Führen Sie Ihre erste Abfrage mit der [Azure CLI](first-query-azurecli.md) aus.
- Führen Sie Ihre erste Abfrage mit [Azure PowerShell](first-query-powershell.md) aus.
- Beginnen Sie mit [einfachen Abfragen](./samples/starter.md).
- Verbessern Sie Ihre Kenntnisse mit [Erweiterten Abfragen](./samples/advanced.md).