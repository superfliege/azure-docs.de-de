---
title: Übersicht über Azure Resource Graph
description: Hier wird erläutert, wie der Azure Resource Graph-Dienst das komplexe bedarfsabhängige Abfragen von Ressourcen ermöglicht.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/29/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 28efdabc024fd32c83ba966b15284ec6ff368d4d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59269287"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Übersicht über den Azure Resource Graph-Dienst

Azure Resource Graph ist ein Dienst in Azure, der die Azure-Ressourcenverwaltung mithilfe effizienter und leistungsstarker Ressourcenuntersuchung befähigen soll, alle Abonnements und Verwaltungsgruppen übergreifend nach Bedarf Abfragen durchzuführen, sodass Sie Ihre Umgebung effektiv beherrschen können. Diese Abfragen verfügen über die folgenden Features:

- Die Fähigkeit zum Abfragen von Ressourcen mit komplexem Filtern, Gruppieren und Sortieren nach Eigenschaften der Ressource.
- Möglichkeit zum iterativen Untersuchen von Ressourcen basierend auf den Governanceanforderungen und Konvertieren des resultierenden Ausdrucks in eine Richtliniendefinition.
- Möglichkeit zum Bewerten der Auswirkungen der Anwendung von Richtlinien in einer großen Cloudumgebung.

In dieser Dokumentation wird jedes Feature ausführlich beschrieben.

> [!NOTE]
> Azure Resource Graph wird von der neuen Funktion zum „Alle Ressourcen“-Durchsuchen des Azure-Portals verwendet. Sie dient zur Unterstützung von Kunden, die umfangreiche Umgebungen verwalten müssen.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Wie Resource Graph Azure Resource Manager ergänzt

Azure Resource Manager sendet Daten derzeit an einen begrenzten Ressourcencache, der mehrere Ressourcenfelder verfügbar macht: Ressourcenname, ID, Typ, Ressourcengruppe, Abonnements und Speicherort. Bisher waren für die Verwendung von unterschiedlichen Ressourceneigenschaften Aufrufe für jeden einzelnen Ressourcenanbieter erforderlich, und für jede Ressource mussten Details zu den Anforderungseigenschaften vorhanden sein.

Mit Azure Resource Graph können Sie auf diese Eigenschaften zugreifen, die die Ressourcenanbieter zurückgeben, ohne jeden Ressourcenanbieter einzeln anrufen zu müssen. Die unterstützten Ressourcentypen sind in der Tabelle unter [Löschen von Azure-Ressourcen für Bereitstellungen im vollständigen Modus](../../azure-resource-manager/complete-mode-deletion.md) mit **Ja** gekennzeichnet.

## <a name="the-query-language"></a>Die Abfragesprache

Da Sie nun besser verstehen, was Azure Resource Graph ist, erfahren Sie nun, wie Sie Abfragen erstellen können.

Es ist wichtig zu wissen, dass die Abfragesprache von Azure Resource Graph auf der von Azure Data Explorer verwendeten [Kusto-Abfragesprache](../../data-explorer/data-explorer-overview.md) basiert.

Informationen zu Vorgängen und Funktionen, die mit Azure Resource Graph verwendet werden können, finden Sie unter [Grundlegendes zur Abfragesprache von Azure Resource Graph](./concepts/query-language.md). Wie Sie Ressourcen durchsuchen, erfahren Sie unter [Untersuchen Ihrer Azure-Ressourcen mit Resource Graph](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Berechtigungen in Azure Resource Graph

Um Resource Graph verwenden zu können, müssen Sie über die richtigen Rechte für die [rollenbasierte Zugriffssteuerung](../../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) und mindestens über Lesezugriff auf die abzufragenden Ressourcen verfügen. Wenn Sie nicht mindestens `read`-Berechtigungen für das Azure-Objekt bzw. die Objektgruppe haben, werden keine Ergebnisse zurückgegeben.

> [!NOTE]
> Ressource Graph verwendet die Abonnements, die für einen Prinzipal während der Anmeldung verfügbar sind. Um Ressourcen eines neuen Abonnements anzuzeigen, die während einer aktiven Sitzung hinzugefügt wurden, muss der Prinzipal den Kontext aktualisieren. Diese Aktion erfolgt nach dem Abmelden und erneuten Anmelden automatisch.

## <a name="throttling"></a>Drosselung

Abfragen an Ressource Graph werden gedrosselt, um für alle Kunden die beste Erfahrung und Antwortzeit bereitzustellen. Wenn Ihre Organisation die Resource Graph-API für umfangreiche und häufige Abfragen verwenden möchte, verwenden Sie das Feedback-Portal auf der Resource Graph-Seite. Geben Sie Ihr Geschäftsszenario an, und aktivieren Sie das Kontrollkästchen „Microsoft darf mich bezüglich meines Feedbacks per E-Mail kontaktieren“, damit das Team Sie kontaktiert.

## <a name="running-your-first-query"></a>Ausführen Ihrer ersten Abfrage

Resource Graph unterstützt die Azure-Befehlszeilenschnittstelle (CLI), Azure PowerShell und das Azure SDK für .NET. Die Abfrage ist für alle Sprachen gleich strukturiert. Informationen zum Aktivieren von Resource Graph in [Azure-Befehlszeilenschnittstelle](first-query-azurecli.md#add-the-resource-graph-extension) und [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Nächste Schritte

- Führen Sie Ihre erste Abfrage mit der [Azure CLI](first-query-azurecli.md) aus.
- Führen Sie Ihre erste Abfrage mit [Azure PowerShell](first-query-powershell.md) aus.
- Beginnen Sie mit [einfachen Abfragen](./samples/starter.md).
- Verbessern Sie Ihre Kenntnisse mit [Erweiterten Abfragen](./samples/advanced.md).