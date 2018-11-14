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
ms.openlocfilehash: a03f2b4e8d216db3764af03dc06b5188289ffc92
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964202"
---
Rollenbasierte Zugriffssteuerung ist eine vererbungsbasierte Sicherheitsstrategie zum Verwalten von Zugriffen, Berechtigungen und Rollen. Untergeordnete Rollen erben Berechtigungen von übergeordneten Rollen. Berechtigungen können auch zugewiesen werden, ohne dass sie von einer übergeordneten Rolle geerbt wurden. Sie können auch zugewiesen werden, um eine Rolle nach Bedarf anzupassen.

Beispielsweise benötigt ein Bereichsadministrator möglicherweise globalen Zugriff zum Ausführen aller Vorgänge für einen angegebenen Bereich. Der Zugriff schließt alle Knoten unterhalb oder innerhalb des Bereichs ein. Ein Geräte-Installationsprogramm benötigt möglicherweise nur Berechtigungen zum *Lesen* und *Aktualisieren* für Geräte und Sensoren.

In jedem Fall werden Rollen *genau die Zugriffsberechtigungen* erteilt, die sie zum Erfüllen ihrer Aufgaben benötigen und die dem Prinzip der geringsten Rechte entsprechen. Nach diesem Prinzip gilt für eine Identität *nur Folgendes*:

* Der Umfang an Zugriff, den sie zum Ausführen ihrer Aufgabe benötigt.
* Eine Rolle, die für das Ausführen ihrer Aufgabe geeignet und darauf beschränkt ist.

>[!IMPORTANT]
> Beachten Sie immer das Prinzip der geringsten Rechte.

Zwei weitere wichtige Aspekte einer rollenbasierten Zugriffssteuerung, die zu beachten sind:

> [!div class="checklist"]
> * Überprüfen Sie in regelmäßigen Abständen Rollenzuweisungen, um sicherzustellen, dass jede Rolle die richtigen Berechtigungen hat.
> * Bereinigen Sie Rollen und Zuweisungen, sobald Rollen oder Zuweisungen von Personen geändert wurden.