---
title: Includedatei
description: Includedatei
services: azure-digital-twins
author: adamgerard
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 075587df445b46c8b03c5448cebf8cd8b12f1179
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323785"
---
Rollenbasierte Zugriffssteuerung ist eine vererbungsbasierte Sicherheitsstrategie zum Verwalten von Zugriffen, Berechtigungen und Rollen. Untergeordnete Rollen erben Berechtigungen von übergeordneten Rollen. Berechtigungen können auch zugewiesen werden, ohne dass sie von einer übergeordneten Rolle geerbt wurden. Sie können auch zugewiesen werden, um eine Rolle nach Bedarf anzupassen.

Beispielsweise könnte es sein, dass ein **Space Administrator** (Raumadministrator) globalen Zugriff benötigt, um alle Vorgänge für einen angegebenen Raum (einschließlich aller unter oder in ihm befindlichen Knoten) ausführen zu können, während ein **Device Installer** (Geräteinstallierer) nur *Lese*- und *Aktualisierungs*-Berechtigung für Geräte und Sensoren benötigt.

In jedem Fall sollten Rollen **genau die Zugriffsberechtigungen** erteilt werden, die sie zum Erfüllen ihrer Aufgaben benötigen und die dem **Prinzip der geringsten Rechte** entsprechen, gemäß dem einer Identität *nur* Folgendes gewährt wird:

* Der Umfang an Zugriff, den sie zum Ausführen ihrer Aufgabe benötigt.
* Eine Rolle, die für das Ausführen ihrer Aufgabe geeignet und darauf beschränkt ist.

>[!IMPORTANT]
> **Beachten Sie immer das Prinzip der geringsten Rechte**.

Zwei weitere wichtige Aspekte einer rollenbasierten Zugriffssteuerung, die zu beachten sind:

> [!div class="checklist"]
> * Überprüfen Sie in regelmäßigen Abständen Rollenzuweisungen, um sicherzustellen, dass jede Rolle die richtigen Berechtigungen hat.
> * Rollen und Zuweisungen sollten bereinigt werden, sobald Rollen oder Zuweisungen von Personen geändert wurden.