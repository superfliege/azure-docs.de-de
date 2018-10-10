---
title: Grundlegendes zum Lebenszyklus von Azure Blueprint
description: Erfahren Sie mehr über den Lebenszyklus eines Blueprints und die einzelnen Phasen.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e0790168a8b9590aaa440a04cd99f26c2ece2818
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991538"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Grundlegendes zum Lebenszyklus von Azure Blueprint

Wie viele Ressourcen in Azure weist ein Blueprint in Azure Blueprint einen typischen und natürlichen Lebenszyklus auf. Sie werden erstellt, bereitgestellt und schließlich gelöscht, wenn sie nicht mehr benötigt werden oder nicht mehr von Relevanz sind.
Blueprints unterstützen konventionelle CRUD-Lebenszyklusvorgänge (Create, Read, Update, Delete – Erstellen, lesen, aktualisieren, löschen), ziehen diese jedoch auch als Grundlage heran, um zusätzliche Statusebenen zu erstellen, die allgemeine CI/CD-Pipelines (Continuous Integration/Continuous Deployment) für die Verwendung durch Pipelines unterstützen, die die Infrastruktur durch Code verwalten – ein wichtiges Element in DevOps, das als Infrastructure as Code (IaC) bezeichnet wird.

Um einen Überblick über Blueprints und die Phasen zu vermitteln, wird ein Standardlebenszyklus vorgestellt:

> [!div class="checklist"]
> - Erstellen und Bearbeiten eines Blueprints
> - Veröffentlichen des Blueprints
> - Erstellen und Bearbeiten einer neuen Blueprintversion
> - Veröffentlichen einer neuen Blueprintversion
> - Löschen einer bestimmten Blueprintversion
> - Löschen des Blueprints

## <a name="creating-and-editing-a-blueprint"></a>Erstellen und Bearbeiten eines Blueprints

Wenn Sie einen Blueprint erstellen, fügen Sie Artefakte hinzu, speichern Sie sie in einer Verwaltungsgruppe und geben Sie einen eindeutigen Namen und eine eindeutige Version an. An dieser Stelle befindet sich der Blueprint im Modus **Entwurf** und kann noch nicht zugewiesen werden. Er kann im Modus **Entwurf** jedoch weiterhin aktualisiert und geändert werden.

Ein Blueprint im Modus **Entwurf**, der noch nicht veröffentlicht wurde, ist auf der Seite **Blueprintdefinitionen** mit einem anderen Symbol versehen als Blueprints, die sich im Modus **Veröffentlicht** befinden. Die **Neueste Version** wird ebenfalls als **Entwurf** für Blueprints angezeigt, die noch nicht veröffentlicht wurden.

Erstellen und bearbeiten Sie einen Blueprint mit dem [Azure-Portal](../create-blueprint-portal.md#create-a-blueprint) oder der [REST-API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Veröffentlichen eines Blueprints

Nachdem Sie alle gewünschten Änderungen an einem Blueprint im Modus **Entwurf** vorgenommen haben, kann er **veröffentlicht** und für die Zuweisung zur Verfügung gestellt werden. Die Blueprintversion mit dem Modus **Veröffentlicht** kann nicht geändert werden.
Einmal **veröffentlicht**, wird der Blueprint mit einem anderen Symbol als Blueprints im Modus **Entwurf** angezeigt. Die Versionsnummer wird in der Spalte **Neueste Version** angezeigt.

Veröffentlichen Sie einen Blueprint mit dem [Azure-Portal](../create-blueprint-portal.md#publish-a-blueprint) oder der [REST-API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Erstellen und Bearbeiten einer neuen Blueprintversion

Eine Blueprintversion im Modus **Veröffentlicht** kann zwar nicht geändert werden, es kann jedoch eine neue Blueprintversion zum vorhandenen Blueprint hinzugefügt und nach Bedarf geändert werden. Dies wird durch Änderungen an einen vorhandenen Blueprint erreicht. Wenn der Blueprint bereits beim Speichern dieser Änderungen den Modus **Veröffentlicht** aufweist, werden diese als **Nicht veröffentlichte Änderungen** in der Liste der Blueprintdefinitionen angezeigt. Durch Speichern der Änderungen wird die Blueprintversion im Modus **Entwurf** gespeichert.

Ein Blueprint kann im [Azure-Portal](../create-blueprint-portal.md#edit-a-blueprint) bearbeitet werden.

## <a name="publishing-a-new-version-of-the-blueprint"></a>Veröffentlichen einer neuen Blueprintversion

Wie bei der ersten Blueprintversion, die zur Zuweisung den Modus **Veröffentlicht** aufweisen muss, kann jede nachfolgende Version dieses Blueprints nur dann zugewiesen werden, wenn sie den Modus **Veröffentlicht** aufweist. Wenn **Nicht veröffentlichte Änderungen** an einem Blueprint vorgenommen wurden, aber dieser noch nicht den Status **Veröffentlicht** aufweist, ist auf der Seite zur Bearbeitung des Blueprints die Schaltfläche **Blueprint veröffentlichen** sichtbar. Wenn die Schaltfläche nicht angezeigt wird, weist der Entwurf bereits den Status **Veröffentlicht** auf, jedoch ohne **Nicht veröffentlichte Änderungen**.

> [!NOTE]
> Ein Blueprint kann mehrere Versionen mit dem Status **Veröffentlicht** aufweisen, die Abonnements zugewiesen werden können.

Die Schritte, um einen Blueprint, bei dem **Nicht veröffentlichte Änderungen** vorliegen, als neue Version eines vorhandenen Blueprints zu veröffentlichen, sind mit den Schritten zum Veröffentlichen eines neuen Blueprint identisch.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Löschen einer bestimmten Blueprintversion

Jede Version eines Blueprints ist ein eindeutiges Objekt und kann separat mit dem Status **Veröffentlicht** versehen werden. Dies bedeutet auch, dass jede Version eines Blueprints gelöscht werden kann. Das Löschen einer Blueprintversion hat keine Auswirkungen auf andere Versionen dieses Blueprints.

> [!NOTE]
> Es ist nicht möglich, Blueprints mit aktiven Zuweisungen zu löschen. Löschen Sie zuerst die Zuweisungen und dann die Version, die entfernt werden soll.

1. Starten Sie den Azure Blueprint-Dienst im Azure-Portal. Klicken Sie dazu auf **Alle Dienste**, und suchen Sie im linken Bereich nach **Richtlinie**. Klicken Sie auf der Seite **Richtlinie** auf **Blueprints**.

1. Wählen Sie auf der Seite auf der linken Seite **Blueprintdefinitionen** aus und verwenden Sie die Filteroptionen, um den Blueprint ausfindig zu machen, dessen Version gelöscht werden soll. Klicken Sie auf diese, um die Bearbeitungsseite zu öffnen.

1. Klicken Sie auf die Registerkarte **Veröffentlichte Versionen**, und suchen Sie die Version, die gelöscht werden soll.

1. Klicken Sie mit der rechten Maustaste auf die zu löschende Version, und wählen Sie **Diese Version löschen** aus.

## <a name="deleting-the-blueprint"></a>Löschen des Blueprints

Der Hauptblueprint kann ebenfalls gelöscht werden. Durch das Löschen des Hauptblueprints werden auch alle Blueprintversionen dieses Blueprints gelöscht, unabhängig davon, ob der Status **Entwurf** oder **Veröffentlicht** lautet. Wie beim Löschen einer Blueprintversion werden auch beim Löschen des Hauptblueprints nicht die vorhandenen Zuweisungen der Blueprintversionen entfernt.

> [!NOTE]
> Es ist nicht möglich, Blueprints mit aktiven Zuweisungen zu löschen. Löschen Sie zuerst die Zuweisungen und dann die Version, die entfernt werden soll.

Löschen Sie einen Blueprint mit dem [Azure-Portal](../create-blueprint-portal.md#delete-a-blueprint) oder der [REST-API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Assignments

Es gibt mehrere Stellen im Lebenszyklus, an denen einem Abonnement ein Blueprint zugewiesen werden kann.
Wenn der Modus einer Blueprintversion **Veröffentlicht** lautet, kann diese Version einem Abonnement zugewiesen werden. Selbst wenn es eine Blueprintversion mit dem Status **Entwurf** und mindestens eine Blueprintversion im Modus **Veröffentlicht** gibt, können diese Versionen mit dem Status **Veröffentlicht** zugewiesen werden. So können Versionen eines Blueprints verwendet und aktiv zugewiesen werden, während eine neuere Version entwickelt wird.

Wenn Blueprintversionen zugewiesen werden, sollte unbedingt beachtet werden, welchen Elementen sie zugewiesen werden und welche Parameter ihnen zugewiesen wurden. Die Parameter können entweder statisch oder dynamisch sein. Weitere Informationen finden Sie im Artikel zu [statischen und dynamischen Parametern](parameters.md).

### <a name="updating-assignments"></a>Aktualisieren von Zuweisungen

Wenn ein Blueprint zugewiesen ist, kann die Zuweisung aktualisiert werden. Es gibt verschiedene Gründe für das Aktualisieren einer vorhandenen Zuweisung:

- Hinzufügen oder Entfernen einer [Ressourcensperre](resource-locking.md)
- Ändern des Wertes von [dynamischen Parametern](parameters.md#dynamic-parameters)
- Durchführen eines Upgrades auf eine neuere Blueprintversion mit dem Status **Veröffentlicht**

Erfahren Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blueprints](sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blueprints sperren](resource-locking.md) können.
- Erfahren Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md) können.
- Beheben Sie Probleme bei der Blueprintzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).