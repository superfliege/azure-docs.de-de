---
title: "Vergleich von Hostingplänen für Azure Functions | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie zwischen einem Azure Functions-Verbrauchstarif und App Service-Plan wählen."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Verbrauchstarif, App Service-Plan, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09bb662e30a97e2741303e2e4630582625954909
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="azure-functions-hosting-plans-comparison"></a>Vergleich von Hostingplänen für Azure Functions

Sie können Azure Functions in zwei verschiedenen Modi ausführen: als Verbrauchsplan und als App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wird der Code nicht mehr ausgeführt, wird die Leistung wieder herunterskaliert. Deshalb müssen Sie für VMs im Leerlauf nicht bezahlen und auch keine Kapazitäten im Voraus reservieren. Dieser Artikel konzentriert sich auf den Verbrauchsplan, ein [serverloses](https://azure.microsoft.com/overview/serverless-computing/) App-Modell. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

>[!NOTE]  
> Linux-Hosting ist derzeit nur im Rahmen eines App Service-Plans verfügbar.

Wenn Sie mit Azure Functions nicht vertraut sind, informieren Sie sich in der [Übersicht über Azure Functions](functions-overview.md).

Wenn Sie eine Funktions-App erstellen, müssen Sie einen Hostingplan für die in der App enthaltenen Funktionen konfigurieren. In beiden Modi führt eine Instanz des *Azure Functions-Hosts* die Funktionen aus. Der Plantyp steuert:

* Wie Hostinstanzen horizontal hochskaliert werden.
* Die Ressourcen, die für jeden Host verfügbar sind.

Derzeit müssen Sie den Typ des Hostingplans während der Erstellung der Funktions-App auswählen. Sie können ihn anschließend nicht ändern. 

In einem App Service-Plan können Sie zwischen verschiedenen Stufen skalieren, um verschiedene Ressourcenmengen zuzuordnen. Im Verbrauchsplan verarbeitet Azure Functions automatisch alle Ressourcenzuordnungen.

## <a name="consumption-plan"></a>Verbrauchsplan

Bei Verwendung eines Verbrauchsplans werden Instanzen des Azure Functions-Hosts dynamisch, basierend auf der Anzahl der eingehenden Ereignisse hinzugefügt und entfernt. Dieser Plan wird automatisch skaliert, sodass Ihnen nur dann Computeressourcen berechnet werden, wenn Ihre Funktionen ausgeführt werden. In einem Verbrauchsplan kann eine Funktion für maximal 10 Minuten ausgeführt werden. 

> [!NOTE]
> Das Standardtimeout für Funktionen in einem Verbrauchsplan beträgt 5 Minuten. Der Wert kann durch Ändern der Eigenschaft `functionTimeout` in [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) für die Funktions-App auf 10 Minuten erhöht werden.

Die Abrechnung erfolgt auf der Grundlage der Anzahl von Ausführungen, der Ausführungszeit und des verwendeten Arbeitsspeichers. Die Abrechnung wird für alle Funktionen innerhalb einer Funktions-App aggregiert. Weitere Informationen finden Sie unter [Preisseite für Azure Functions].

Der Verbrauchsplan ist der Standardhostingplan. Er bietet folgende Vorteile:
- Sie bezahlen nur, wenn Ihre Funktionen ausgeführt werden.
- Das horizontale Hochskalieren erfolgt automatisch – selbst in Zeiten hoher Lasten.

## <a name="app-service-plan"></a>App Service-Plan

In einem App Service-Plan werden Ihre Funktions-Apps ähnlich wie Web-Apps, API-Apps und mobile Apps auf dedizierten virtuellen Computern für Basic-, Standard- oder Premium-SKUs oder isolierte SKUs ausgeführt. Die dedizierten virtuellen Computer werden Ihren App Service-Apps zugeordnet, sodass der Functions-Host immer ausgeführt wird. App Service-Pläne unterstützen Linux.

Entscheiden Sie sich in den folgenden Fällen für einen App Service-Plan:
- Sie verfügen über nicht ausgelastete virtuelle Computer, auf denen bereits andere App Service-Instanzen ausgeführt werden.
- Sie gehen davon aus, dass Ihre Funktionen-Apps kontinuierlich oder nahezu kontinuierlich ausgeführt werden. In diesem Fall kann ein App Service-Plan kostengünstiger sein.
- Sie benötigen weitere CPU- oder Arbeitsspeicheroptionen zusätzlich zu den für den Verbrauchsplan bereitgestellten.
- Sie benötigen eine längere Ausführungsdauer als die für den Verbrauchsplan zulässige Höchstdauer (10 Minuten).
- Sie benötigen Funktionen, die nur mit einem App Service-Plan verfügbar sind, etwa Unterstützung für App Service-Umgebung, VNET/VPN-Konnektivität und größere virtuelle Computer. 
- Sie möchten Ihre Funktions-App unter Linux ausführen oder ein benutzerdefiniertes Image zum Ausführen Ihrer Funktionen bereitstellen.

Mit einem virtuellen Computer werden Kosten von der Ausführungsanzahl, der Ausführungszeit und vom verwendeten Arbeitsspeicher entkoppelt. Daher wird Ihnen nicht mehr berechnet als die Kosten für die VM-Instanz, die Sie belegen. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Mit einem App Service-Plan können Sie manuell horizontal hochskalieren, indem Sie weitere Instanzen von virtuellen Computern hinzufügen, oder Sie können die automatische Skalierung aktivieren. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Sie können auch zentral hochskalieren, indem Sie einen anderen App Service-Plan auswählen. Weitere Informationen finden Sie unter [Zentrales Hochskalieren einer App in Azure](../app-service/web-sites-scale.md). 

Wenn Sie beabsichtigen, JavaScript-Funktionen im Rahmen eines App Service-Plans auszuführen, sollten Sie einen Plan mit weniger vCPUs wählen. Weitere Informationen finden Sie unter [Auswählen von Einzelkern-App Service-Plänen](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### Always On

Wenn Sie einen App Service-Plan verwenden, sollten Sie die Einstellung **Always On** aktivieren, damit Ihre Funktions-App ordnungsgemäß ausgeführt wird. Bei einem App Service-Plan geht die Functions-Runtime nach wenigen Minuten der Inaktivität in den Leerlauf über, sodass nur HTTP-Trigger Ihre Funktionen tatsächlich „reaktivieren“ können. Dies ist ähnlich wie bei WebJobs, bei denen Always On aktiviert sein muss. 

Always On ist nur bei einem App Service-Plan verfügbar. Bei einem Verbrauchsplan aktiviert die Plattform Funktions-Apps automatisch.

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Sowohl bei einem Verbrauchs- als auch bei einem App Service-Plan erfordert eine Funktions-App ein allgemeines Azure Storage-Konto, das Azure Blob, Queue, Files und Table Storage unterstützt. Intern verwendet Azure Functions Azure Storage für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten (darunter Storage Premium) und allgemeine Speicherkonten mit zonenredundanter Speicherreplikation. Diese Konten werden aus dem Blatt **Speicherkonto** herausgefiltert, wenn Sie eine Funktions-App erstellen.

Weitere Informationen zu Speicherkontentypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Funktionsweise des Verbrauchsplans

Im Verbrauchsplan skaliert der Skalierungscontroller CPU- und Arbeitsspeicherressourcen automatisch, indem dem Functions-Host basierend auf der Anzahl der Ereignisse, nach denen die Funktionen ausgelöst werden, weitere Instanzen hinzugefügt werden. Jede Instanz des Functions-Hosts ist auf 1,5 GB Arbeitsspeicher beschränkt.

Wenn Sie den verbrauchsbasierten Hostingplan verwenden, werden die Funktionscodedateien in Azure Files-Freigaben im Hauptspeicherkonto der Funktion gespeichert. Wenn Sie das Hauptspeicherkonto der Funktions-App löschen, werden die Funktionscodedateien gelöscht und können nicht wiederhergestellt werden.

> [!NOTE]
> Bei Verwendung eines Blobtriggers in einem Verbrauchsplan kann es möglicherweise bis zu 10 Minuten dauern, bis neue Blobs verarbeitet werden, nachdem eine Funktions-App in den Leerlauf gewechselt ist. Sobald die Funktions-App ausgeführt wird, werden die Blobs sofort verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, sollten Sie eine der folgenden Möglichkeiten erwägen:
> - Hosten Sie die Funktions-App in einem App Service-Plan mit aktivierter Option „Always On“.
> - Verwenden Sie einen anderen Mechanismus zum Auslösen der Blobverarbeitung, z.B. eine Warteschlangennachricht, die den Blobnamen enthält. Ein Beispiel finden Sie in den [C#-Skript- und JavaScript-Beispielen für Blobeingabe- und Blobausgabebindungen](functions-bindings-storage-blob.md#input--output---example).

### <a name="runtime-scaling"></a>Laufzeitskalierung

Azure Functions verwendet die Komponente *Skalierungscontroller*, um die Rate der Ereignisse zu überwachen und zu bestimmen, ob eine horizontale Hoch- oder Herunterskalierung erforderlich ist. Der Skalierungscontroller verwendet Heuristik für jeden Triggertyp. Bei Verwendung eines Triggers für Azure Queue Storage beispielsweise basieren die Skalen auf der Länge der Warteschlange und dem Alter der ältesten Nachricht in der Warteschlange.

Die Skalierungseinheit ist die Funktionen-App. Bei einer horizontalen Hochskalierung der Funktions-App werden zusätzliche Ressourcen zugeordnet, um mehrere Instanzen des Azure Functions-Hosts auszuführen. Umgekehrt entfernt der Skalierungscontroller bei abnehmenden Computeanforderungen Instanzen des Functions-Hosts. Die Anzahl der Instanzen wird schließlich zentral auf null herunterskaliert, wenn in einer Funktionen-App keine Funktionen ausgeführt werden.

![Skalierungscontroller: Überwachen von Ereignissen und Erstellen von Instanzen](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Abrechnungsmodell

Die Abrechnung des Verbrauchsplans wird detailliert auf der [Preisseite für Azure Functions] beschrieben. Der Verbrauch wird auf Ebene der Funktions-App zusammengefasst, wobei nur die Zeit gezählt wird, für die der Funktionscode ausführt wurde. Folgende Einheiten werden für die Abrechnung verwendet: 
* **Ressourcenverbrauch in Gigabytesekunden (GB-s)** – berechnet als Kombination aus Arbeitsspeichergröße und Ausführungsdauer für alle Funktionen in einer Funktions-App. 
* **Ausführungen** – werden bei jeder Ausführung einer Funktion als Antwort auf einen Ereignisauslöser gezählt.

[Preisseite für Azure Functions]: https://azure.microsoft.com/pricing/details/functions
