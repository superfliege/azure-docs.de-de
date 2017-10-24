---
title: "Leistung und Skalierbarkeit in Durable Functions – Azure"
description: "Einführung in die Durable Functions-Erweiterung für Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 10ce74097388a0283797e4692126c5039e8d4dd0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Leistung und Skalierbarkeit in Durable Functions (Azure Functions)

Um die Leistung und Skalierbarkeit zu optimieren, ist es wichtig die eindeutigen Merkmale von [Durable Functions](durable-functions-overview.md) zu verstehen.

Um das Skalierungsverhalten zu verstehen, müssen Sie einige Details der zugrunde liegenden Azure Storage-Anbieter verstehen, die von Durable Functions verwendet werden.

## <a name="history-table"></a>Verlaufstabelle

Die Verlaufstabelle ist eine Azure Storage-Tabelle, die die Verlaufsereignisse für alle Orchestrierungsinstanzen enthält. Während Instanzen ausgeführt werden, werden dieser Tabelle neue Zeilen hinzugefügt. Der Partitionsschlüssel dieser Tabelle wird von der Instanzen-ID der Orchestrierung abgeleitet. Diese Werte werden in den meisten Fällen auf Zufallsbasis generiert, wodurch die optimale Verteilung von internen Partitionen in Azure Storage sichergestellt wird.

## <a name="internal-queue-triggers"></a>Interne Warteschlangentrigger

Orchestrator-und Aktivitätsfunktionen werden beide durch interne Warteschlangen im Standardspeicherkonto der Funktions-App ausgelöst. Es gibt zwei Arten von Warteschlangen in Durable Functions: die **control queue** (Steuerelement-Warteschlange) und die **work item queue** (Warteschlange für Arbeitsaufgaben).

### <a name="the-work-item-queue"></a>Die Warteschlange für Arbeitsaufgaben

In Durable Functions gibt es eine Warteschlange für Arbeitsaufgaben pro Aufgabenhub. Dies ist eine grundlegende Warteschlange, und sie verhält sich ähnlich wie jede andere `queueTrigger`-Warteschlange in Azure Functions. Diese Warteschlange wird verwendet, um zustandslose *Aktivitätsfunktionen* auszulösen. Wenn eine Durable Functions-Anwendung für mehrere virtuelle Computer horizontal hochskaliert wird, konkurrieren alle diese virtuellen Computer miteinander, um Arbeitsaufgaben aus der Warteschlange für Arbeitsaufgaben abzurufen.

### <a name="control-queues"></a>Steuerelement-Warteschlange(n)

Die *control queue* (Steuerelement-Warteschlange) ist komplexer als die einfachere Warteschlange für Arbeitsaufgaben. Sie wird verwendet, um zustandsbehaftete Orchestratorfunktionen auszulösen. Da die Orchestratorfunktionsinstanzen zustandsbehaftete Singletons sind, ist es nicht möglich, ein konkurrierendes Consumermodell zu verwenden, um die Last zwischen virtuellen Computern zu verteilen. Vielmehr wird bei Orchestratornachrichten die Last auf mehrere Steuerelement-Warteschlangen verteilt. Weitere Informationen zu diesem Thema finden Sie in den folgenden Abschnitten.

Steuerelement-Warteschlangen enthalten eine Vielzahl von Orchestrierung-Lebenszyklus-Nachrichtentypen. Beispiele hierfür sind [orchestrator control messages](durable-functions-instance-management.md) (Orchestratorsteuerungsmeldungen), Aktivitätsfunktion-*Antwort*-Nachrichten und Timernachrichten.

## <a name="orchestrator-scale-out"></a>Horizontales Skalieren des Orchestrators

Aktivitätsfunktionen sind zustandslos, und die horizontale Hochskalierung wird automatisch durch Hinzufügen von virtuellen Computern ausgeführt. Orchestratorfunktionen dagegen, sind über mindestens eine oder mehr Steuerelement-Warteschlangen *partitioniert*. Die Anzahl von Steuerelement-Warteschlangen ist, nachdem Sie begonnen haben die Last zu erstellen, unveränderlich und kann nicht geändert werden.

Wenn mehrere Funktionshostinstanzen (in der Regel auf verschiedenen virtuellen Computern) horizontal hochskaliert werden, erhält jede Instanz eine Sperre auf einer der Steuerelement-Warteschlangen. Diese Sperre stellt sicher, dass eine Orchestrierungsinstanz nur auf einem einzelnen virtuellen Computer zu einer Zeit ausgeführt wird. Dies bedeutet, dass wenn ein Aufgabenhub mit drei Steuerelement-Warteschlangen konfiguriert ist, bei Orchestrierungsinstanzen die Last auf bis zu drei virtuelle Computer verteilt werden kann. Zusätzliche virtuelle Computer können zum Erhöhen der Kapazität für die Ausführung der Aktivitätsfunktion hinzugefügt werden.  Jedoch werden die zusätzlichen Ressourcen nicht dazu verwendet, um Orchestratorfunktionen auszuführen.

Das folgende Diagramm veranschaulicht, wie der Azure Functions-Host mit den Speicherentitäten in einer horizontal skalierten Umgebung interagiert.

![Skalierungsdiagramm](media/durable-functions-perf-and-scale/scale-diagram.png)

Wie Sie sehen können, können alle virtuellen Computer in der Warteschlange für Arbeitsaufgaben um Nachrichten konkurrieren. Allerdings können nur drei virtuelle Computer Nachrichten aus Steuerelement-Warteschlangen abrufen, und jeder virtuelle Computer sperrt eine einzige Steuerelement-Warteschlange.

Orchestrierungsinstanzen sind auf Steuerelement-Warteschlangeninstanzen verteilt, durch Ausführen einer internen Hashfunktion mit der Instanz-ID der Orchestrierung. Instanz-IDs werden automatisch generiert und sind standardmäßig zufällig, wodurch sicherstellt ist, dass Instanzen gleichmäßig auf alle verfügbaren Steuerelement-Warteschlangen verteilt werden. Die aktuelle Standardanzahl von unterstützten Steuerelement-Warteschlangen-Partitionen ist **4**.

> [!NOTE]
> Es ist aktuell nicht möglich, die Anzahl von Partitionen in Azure Functions zu konfigurieren. [Work to support this configuration option is being tracked (Arbeit im Hinblick auf die Unterstützung der Nachverfolgung dieser Konfigurationsoption)](https://github.com/Azure/azure-functions-durable-extension/issues/73).

Im Allgemeinen sollten Orchestratorfunktionen einfach sein und nicht viel Rechenleistung in Anspruch nehmen. Aus diesem Grund ist es nicht erforderlich, eine große Anzahl von Steuerelement-Warteschlange-Partitionen zu erstellen, um einen erhöhten Durchsatz zu erzielen. Stattdessen erfolgt der größte Teil der schweren Arbeit in zustandslosen Aktivitätsfunktionen, die unbegrenzt horizontal skaliert werden können.

## <a name="auto-scale"></a>Automatische Skalierung

Wie bei allen Azure Functions, die im Nutzungsplan ausgeführt werden, unterstützen Durable Functions die automatische Skalierung über den [Azure Functions-Skalierungscontroller](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). Der Skalierungscontroller überwacht die Länge der Warteschlange für Arbeitsaufgaben und jede der Steuerelement-Warteschlangen, indem er VM-Ressourcen entsprechend hinzufügt oder entfernt. Wenn die Längen der Steuerelement-Warteschlangen mit der Zeit zunehmen, wird der Skalierungscontroller das Hinzufügen von Instanzen so lange fortsetzen, bis die Anzahl von Steuerelement-Warteschlangenpartitionen erreicht ist. Wenn die Längen der Warteschlangen für Arbeitsaufgaben mit der Zeit zunehmen, wird der Skalierungscontroller das Hinzufügen von VM-Ressourcen so lange fortsetzen, bis eine Übereinstimmung mit der Last erreicht ist, unabhängig von der Anzahl von Steuerelement-Warteschlangenpartitionen.

## <a name="thread-usage"></a>Verwendung von Threads

Orchestratorfunktionen werden in einem einzelnen Thread ausgeführt. Dies ist erforderlich, um sicherzustellen, dass die Ausführung der Orchestratorfunktion deterministisch ist. Daher ist es wichtig, die Orchestratorfunktion nie unnötig auszulasten, mit Aufgaben wie E/A (die aus vielerlei Gründen nicht zulässig ist), Blockierungs-, oder Rotiervorgängen. Jede Arbeit, die möglicherweise E/A, die Blockierung oder mehrere Threads erfordert, sollte in die Aktivitätsfunktion verschoben werden.

Aktivitätsfunktionen haben alle dasselbe Verhalten, als reguläre, durch Warteschlangen ausgelöste Funktionen. Dies bedeutet, dass sie problemlos E/A-Vorgänge und CPU-intensive Vorgänge ausführen sowie mehrere Threads verwenden können. Da Aktivitätsauslöser zustandslos sind, können sie jederzeit auf eine unbegrenzte Anzahl von virtuellen Computern horizontal hochskaliert werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Install the Durable Functions extension and samples (Installieren der Erweiterung Durable Functions und Beispiele)](durable-functions-install.md)
