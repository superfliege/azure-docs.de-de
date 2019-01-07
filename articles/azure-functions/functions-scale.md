---
title: Skalierung und Hosting von Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie zwischen einem Azure Functions-Verbrauchstarif und App Service-Plan wählen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, Functions, Verbrauchstarif, App Service-Plan, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1940908cc0120ed5a69ae6603ec101d020b0ef3f
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722073"
---
# <a name="azure-functions-scale-and-hosting"></a>Skalierung und Hosting von Azure Functions

Azure Functions wird in zwei verschiedenen Modi ausgeführt: Verbrauchsplan oder Azure App Service-Plan. Der Verbrauchsplan weist bei Ausführung Ihres Codes automatisch Computeleistung zu. Ihre App wird horizontal hochskaliert, wenn dies zur Verarbeitung der Last erforderlich ist, und herunterskaliert, wenn der Code nicht ausgeführt wird. Für virtuelle Computer im Leerlauf müssen Sie nichts bezahlen und auch keine Kapazitäten im Voraus reservieren.

> [!NOTE]  
> [Linux-Hosting](functions-create-first-azure-function-azure-cli-linux.md) ist derzeit nur im Rahmen eines App Service-Plans verfügbar.

Wenn Sie mit Azure Functions nicht vertraut sind, informieren Sie sich in der [Übersicht über Azure Functions](functions-overview.md).

Wenn Sie eine Funktions-App erstellen, wählen Sie den Hostingplan für Funktionen in der App aus. In beiden Plänen führt eine Instanz des *Azure Functions-Hosts* die Funktionen aus. Der Plantyp steuert:

* Wie Hostinstanzen horizontal hochskaliert werden.
* Die Ressourcen, die für jeden Host verfügbar sind.

> [!IMPORTANT]
> Sie müssen den Typ des Hostingplans während der Erstellung der Funktions-App auswählen. Sie können ihn anschließend nicht ändern.

In einem App Service-Plan können Sie zwischen verschiedenen Stufen skalieren, um verschiedene Ressourcenmengen zuzuordnen. Im Verbrauchsplan verarbeitet Azure Functions automatisch alle Ressourcenzuordnungen. 

## <a name="consumption-plan"></a>Verbrauchsplan

Bei Verwendung eines Verbrauchsplans werden Instanzen des Azure Functions-Hosts dynamisch, basierend auf der Anzahl der eingehenden Ereignisse hinzugefügt und entfernt. Dieser serverlose Plan wird automatisch skaliert, sodass Ihnen nur dann Computeressourcen berechnet werden, wenn Ihre Funktionen ausgeführt werden. In einem Verbrauchsplan tritt für eine Funktionsausführung nach einem konfigurierbaren Zeitraum ein Timeout auf.

> [!NOTE]
> Das Standardtimeout für Funktionen in einem Verbrauchsplan beträgt 5 Minuten. Der Wert kann durch Ändern der Eigenschaft `functionTimeout` in der Projektdatei [host.json](functions-host-json.md#functiontimeout) für die Funktions-App auf maximal 10 Minuten erhöht werden.

Die Abrechnung erfolgt auf der Grundlage der Anzahl von Ausführungen, der Ausführungszeit und des verwendeten Arbeitsspeichers. Die Abrechnung wird für alle Funktionen innerhalb einer Funktions-App aggregiert. Weitere Informationen finden Sie unter [Azure Functions – Preise].

Der Verbrauchsplan ist der Standardhostingplan. Er bietet folgende Vorteile:

* Sie bezahlen nur, wenn Ihre Funktionen ausgeführt werden.
* Das horizontale Hochskalieren erfolgt automatisch – selbst in Zeiten hoher Lasten.

## <a name="app-service-plan"></a>App Service-Plan

In einem dedizierten App Service-Plan werden Ihre Funktions-Apps wie auch andere App Service-Apps auf dedizierten virtuellen Computern für Basic-, Standard- oder Premium-SKUs oder isolierte SKUs ausgeführt. Die dedizierten virtuellen Computer werden Ihrer Funktions-App zugeordnet, sodass der Functions-Host [immer ausgeführt werden kann](#always-on). App Service-Pläne unterstützen Linux.

Entscheiden Sie sich in den folgenden Fällen für einen App Service-Plan:

* Sie verfügen über nicht ausgelastete virtuelle Computer, auf denen bereits andere App Service-Instanzen ausgeführt werden.
* Ihre Funktions-Apps werden kontinuierlich oder nahezu kontinuierlich ausgeführt. In diesem Fall kann ein App Service-Plan kostengünstiger sein.
* Sie benötigen weitere CPU- oder Arbeitsspeicheroptionen zusätzlich zu den für den Verbrauchsplan bereitgestellten.
* Ihr Code benötigt eine längere Ausführungsdauer als die für den Verbrauchsplan zulässige Höchstdauer, die bis zu 10 Minuten beträgt.
* Sie benötigen Funktionen, die nur mit einem App Service-Plan verfügbar sind, etwa Unterstützung für App Service-Umgebung, VNET/VPN-Konnektivität und größere virtuelle Computer.
* Sie möchten Ihre Funktions-App unter Linux ausführen oder ein benutzerdefiniertes Image zum Ausführen Ihrer Funktionen bereitstellen.

Mit einem virtuellen Computer werden Kosten von der Ausführungsanzahl, der Ausführungszeit und vom verwendeten Arbeitsspeicher entkoppelt. Daher wird Ihnen nicht mehr berechnet als die Kosten für die VM-Instanz, die Sie belegen. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/overview-hosting-plans.md). 

Mit einem App Service-Plan können Sie manuell horizontal hochskalieren, indem Sie weitere Instanzen von virtuellen Computern hinzufügen, oder Sie können die automatische Skalierung aktivieren. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Sie können auch zentral hochskalieren, indem Sie einen anderen App Service-Plan auswählen. Weitere Informationen finden Sie unter [Zentrales Hochskalieren einer App in Azure](../app-service/web-sites-scale.md). 

Wenn Sie JavaScript-Funktionen im Rahmen eines App Service-Plans ausführen, sollten Sie einen Plan mit weniger vCPUs wählen. Weitere Informationen finden Sie unter [Auswählen von Einzelkern-App Service-Plänen](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### <a name="always-on"></a>Always On

Wenn Sie einen App Service-Plan verwenden, müssen Sie die Einstellung **Always On** aktivieren, damit Ihre Funktions-App ordnungsgemäß ausgeführt wird. Bei einem App Service-Plan geht die Functions-Runtime nach wenigen Minuten der Inaktivität in den Leerlauf über, sodass nur HTTP-Trigger Ihre Funktionen tatsächlich „reaktivieren“ können. Always On ist nur bei einem App Service-Plan verfügbar. Bei einem Verbrauchsplan aktiviert die Plattform Funktions-Apps automatisch.

## <a name="what-is-my-hosting-plan"></a>Informationen zum Hostingplan

Wenn Sie den von Ihrer Funktions-App verwendeten Hostingplan ermitteln möchten, sehen Sie im [Azure-Portal](https://portal.azure.com) auf der Registerkarte **Übersicht** für die Funktions-App unter **App Service-Plan/-Tarif** nach. Bei App Service-Plänen ist auch der Tarif angegeben. 

![Anzeigen des Skalierungsplans im Portal](./media/functions-scale/function-app-overview-portal.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um den Plan auf folgende Weise zu ermitteln:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Wenn die Ausgabe dieses Befehls `dynamic` lautet, ist Ihre Funktions-App im Verbrauchsplan enthalten. Alle anderen Werte geben Stufen eines App Service-Plans an.

Auch bei aktiviertem Always On wird das Ausführungstimeout für einzelne Funktionen durch die Einstellung `functionTimeout` in der [host.json](functions-host-json.md#functiontimeout)-Projektdatei gesteuert.

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Sowohl bei einem Verbrauchs- als auch bei einem App Service-Plan erfordert eine Funktions-App ein allgemeines Azure Storage-Konto, das Azure Blob, Queue, Files und Table Storage unterstützt. Der Grund dafür ist, dass Functions bei Vorgängen wie dem Verwalten von Triggern und Protokollieren von Funktionsausführungen auf Azure Storage basiert, einige Speicherkonten jedoch keine Warteschlangen und Tabellen unterstützen. Diese Konten, zu denen reine Blobspeicherkonten (einschließlich Storage Premium) und allgemeine Speicherkonten mit zonenredundanter Speicherreplikation gehören, werden aus den vorhandenen Auswahlmöglichkeiten für **Speicherkonto** herausgefiltert, wenn Sie eine Funktions-App erstellen.

<!-- JH: Does using a Premium Storage account improve perf? -->

Weitere Informationen zu Speicherkontentypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Funktionsweise des Verbrauchsplans

Im Verbrauchsplan skaliert der Skalierungscontroller CPU- und Arbeitsspeicherressourcen automatisch, indem dem Functions-Host basierend auf der Anzahl der Ereignisse, nach denen die Funktionen ausgelöst werden, weitere Instanzen hinzugefügt werden. Jede Instanz des Functions-Hosts ist auf 1,5 GB Arbeitsspeicher beschränkt.  Eine Instanz des Hosts ist die Funktions-App, d.h. alle Funktionen innerhalb einer Funktions-App verwenden innerhalb einer Instanz die gleiche Ressource und werden gleichzeitig skaliert. Funktions-Apps, die den gleichen Verbrauchsplan nutzen, werden unabhängig voneinander skaliert.  

Wenn Sie den verbrauchsbasierten Hostingplan verwenden, werden die Funktionscodedateien in Azure Files-Freigaben im Hauptspeicherkonto der Funktion gespeichert. Wenn Sie das Hauptspeicherkonto der Funktions-App löschen, werden die Funktionscodedateien gelöscht und können nicht wiederhergestellt werden.

> [!NOTE]
> Bei Verwendung eines Blobtriggers in einem Verbrauchsplan kann es möglicherweise bis zu 10 Minuten dauern, bis neue Blobs verarbeitet werden. Diese Verzögerung tritt auf, nachdem eine Funktions-App in den Leerlauf gewechselt ist. Sobald die Funktions-App ausgeführt wird, werden die Blobs sofort verarbeitet. Um diese Kaltstartverzögerung zu vermeiden, verwenden Sie einen App Service-Plan, für den **Always On** aktiviert ist, oder den Event Grid-Trigger. Weitere Informationen finden Sie im Referenzartikel zur [Blobtriggerbindung](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Laufzeitskalierung

Azure Functions verwendet die Komponente *Skalierungscontroller*, um die Rate der Ereignisse zu überwachen und zu bestimmen, ob eine horizontale Hoch- oder Herunterskalierung erforderlich ist. Der Skalierungscontroller verwendet Heuristik für jeden Triggertyp. Bei Verwendung eines Triggers für Azure Queue Storage beispielsweise basieren die Skalen auf der Länge der Warteschlange und dem Alter der ältesten Nachricht in der Warteschlange.

Die Skalierungseinheit ist die Funktionen-App. Bei einer horizontalen Hochskalierung der Funktions-App werden zusätzliche Ressourcen zugeordnet, um mehrere Instanzen des Azure Functions-Hosts auszuführen. Umgekehrt entfernt der Skalierungscontroller bei abnehmenden Computeanforderungen Instanzen des Functions-Hosts. Die Anzahl der Instanzen wird schließlich zentral auf null herunterskaliert, wenn in einer Funktionen-App keine Funktionen ausgeführt werden.

![Skalierungscontroller: Überwachen von Ereignissen und Erstellen von Instanzen](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Grundlegendes zum Verhalten von Skalierungen

Die Skalierung variiert ausgehend von verschiedenen Faktoren, und die Skalierung ist je nach dem ausgewählten Auslöser und der Sprache unterschiedlich. Das System umfasst heute einige Aspekte der Skalierung:

* Eine einzelne Funktions-App wird maximal auf 200 Instanzen hochskaliert. Eine einzelne Instanz kann mehrere Meldungen oder Anforderungen gleichzeitig verarbeiten, weshalb es keine Grenze für die Anzahl gleichzeitiger Ausführungen gibt.
* Neue Instanzen werden nur höchstens einmal alle 10 Sekunden zugeordnet.

Für verschiedene Auslöser gelten unter Umständen unterschiedliche Grenzwerte für die Skalierung sowie die folgenden Angaben:

* [Event Hub](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Bewährte Methoden und Muster für skalierbare Apps

Es gibt viele Aspekte einer Funktions-App, die sich auf die Skalierung auswirken. Dazu zählen beispielsweise die Hostkonfiguration, der Runtimespeicherbedarf und die Ressourceneffizienz.  Weitere Informationen finden Sie im [Abschnitt zur Skalierbarkeit im Artikel zum Thema Leistung](functions-best-practices.md#scalability-best-practices). Sie sollten auch das Verhalten von Verbindungen beim Skalieren Ihrer Funktions-App beachten. Weitere Informationen finden Sie unter [How to manage connections in Azure Functions](manage-connections.md) (Verwalten von Verbindungen in Azure Functions).

### <a name="billing-model"></a>Abrechnungsmodell

Die Abrechnung des Verbrauchsplans wird detailliert auf der [Azure Functions – Preise] beschrieben. Der Verbrauch wird auf Ebene der Funktions-App zusammengefasst, wobei nur die Zeit gezählt wird, für die der Funktionscode ausführt wurde. Folgende Einheiten werden für die Abrechnung verwendet:

* **Ressourcenverbrauch in Gigabytesekunden (GB-s)** – berechnet als Kombination aus Arbeitsspeichergröße und Ausführungsdauer für alle Funktionen in einer Funktions-App. 
* **Ausführungen** – werden bei jeder Ausführung einer Funktion als Antwort auf einen Ereignisauslöser gezählt.

[Azure Functions – Preise]: https://azure.microsoft.com/pricing/details/functions
