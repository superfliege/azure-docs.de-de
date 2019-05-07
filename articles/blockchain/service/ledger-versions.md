---
title: Von Azure Blockchain unterstützte Ledgerversionen, Patchen und Upgraden
description: Übersicht der unterstützten Ledgerversionen in Azure Blockchain, einschließlich Richtlinien bezüglich Systempatches und Upgrades, die vom System bzw. von Benutzern verwaltet werden.
services: azure-blockchain
keywords: Blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 63c9a8b9e266dacbb0fb6faba50fb44ac9a4b46e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027614"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Von Azure Blockchain unterstützte Ledgerversionen

Azure Blockchain verwendet den auf Ethereum basierenden Ledger [Quorum](https://github.com/jpmorganchase/quorum/wiki), der dafür konzipiert ist, private Transaktionen innerhalb einer Gruppe bekannter Teilnehmer zu verarbeiten. Diese werden in Azure Blockchain als Konsortium bezeichnet.

Momentan unterstützt Azure Blockchain [Quorum Version 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) und den [Transaktions-Manager Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades

Die Versionsverwaltung in Quorum erfolgt über Haupt-, Neben- und Patchversionen. Wenn die Quorum-Version z. B. 2.0.1 ist, ist 2 die Hauptversion, 0 die Nebenversion und 1 ist die Patchversion. Der Dienst patcht Patchversionen des Ledgers automatisch. Momentan wird das Upgraden von Haupt- und Nebenversionen nicht unterstützt.

## <a name="availability-of-new-ledger-versions"></a>Verfügbarkeit neuer Ledgerversionen

Azure Blockchain macht die aktuellste Version des Ledgers innerhalb von 60 Tagen nach dessen Veröffentlichung durch den Hersteller verfügbar. Wenn Sie ein neues Mitglied und Konsortium bereitstellen, stehen maximal vier Nebenversionen für Konsortien zur Auswahl.

## <a name="next-steps"></a>Nächste Schritte

[Einschränkungen des Azure Blockchain-Diensts](limits.md)
