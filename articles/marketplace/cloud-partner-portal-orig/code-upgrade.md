---
title: Upgraden des Codes auf die neueste Plattform | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie Ihre Microsoft Dynamics 365 for Operations-Plattform auf die neueste Plattformversion upgraden.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 787d9ef509bcafeb6a21d4b0ec952ea1eb4fd52b
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806398"
---
# <a name="upgrading-code-to-the-latest-platform"></a>Upgraden des Codes auf die neueste Plattform

In diesem Artikel erfahren Sie, wie Sie Ihre Microsoft Dynamics 365 for Operations-Plattform auf die neueste Plattformversion upgraden.

## <a name="overview"></a>Übersicht

Die Microsoft Dynamics 365 for Operations-Plattform umfasst folgende Komponenten:

Binärdateien der Dynamics 365 for Operations-Plattform wie Application Object Server (AOS), das Datenverwaltungsframework, das Berichts- und BI-Framework (Business Intelligence), Entwicklungstools und Analysedienste. Folgende AOT-Pakete (Application Object Tree):

1. Anwendungsplattform
2. Anwendungsgrundlage
3. Grundlegende Testkomponenten

**Wichtig:** Wenn Sie ein Upgrade auf die neueste Dynamics 365 for Operations-Plattform durchführen möchten, dürfen für Ihre Dynamics 365 for Operations-Implementierung keine Anpassungen (Überlagerungen) von AOT-Paketen vorhanden sein, die zu der Plattform gehören. Diese Einschränkung wurde im Plattformupdate 3 eingeführt, um nahtlose kontinuierliche Plattformupdates zu ermöglichen. Lesen Sie bei Verwendung einer älteren Plattform (vor dem Plattformupdate 3) den Abschnitt „Upgrading to platform update 3 from an earlier build“ (Upgraden eines älteren Builds auf Plattformupdate 3) am Ende dieses Artikels.

Weitere Informationen zum Codeupgrade finden Sie [hier](https://docs.microsoft.com/dynamics365/operations/dev-itpro/migration-upgrade/upgrade-latest-platform-update).