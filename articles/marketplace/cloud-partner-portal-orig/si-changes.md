---
title: Verkäufer-Insights – Anmerkungen zu dieser Version | Microsoft-Dokumentation
description: Enthält Informationen zu Änderungen an den Verkäufer-Insights.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ec3784a91f8aeb7f0fedd13c9ab86a844832a578
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806035"
---
<a name="seller-insights-release-notes"></a>Verkäufer-Insights – Anmerkungen zu dieser Version 
===============================
(Veröffentlichungsdatum: 28. Juli 2018)

Dieser Artikel enthält Informationen zu Änderungen am Feature für Verkäufer-Insights im [Cloud-Partnerportal](https://cloudpartner.azure.com/#insights).


<a name="release-highlights"></a>Highlights der Version
------------------

-   *Geschätzte Preise* bieten einen Überblick über die Kundengebühren mit Auswirkungen auf die Wechselkurse.
-   *Vorhergesagte Auszahlungen* bieten einen vorgezogenen Überblick über potenzielle Auszahlungen.
-  *Nutzungsreferenzbezeichner* sorgen für Datengenauigkeit zwischen Kundennutzung und Bestellungen mit Auszahlungen.
-   *Nutzung nach täglicher Struktur* bietet mehr Granularität und bessere Einblicke in die Nutzung der Kunden.


<a name="changes-to-data-structure-and-taxonomy"></a>Änderungen an Datenstruktur und Taxonomie
--------------------------------------

Die folgende Tabelle listet die Metriken auf, die mit dieser Version hinzugefügt oder wesentlich geändert wurden. 

| **Neuer Begriff**                   |    **Definition**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Preis (CC)                     | Der Preis für eine Verbrauchseinheit einer bestimmten SKU (in der Währung des Kunden).       |
| Geschätzte erweiterte Gebühren (CC) | Die geschätzten erweiterten Gebühren für die Menge der Verbrauchseinheiten für eine bestimmte SKU (in der Währung des Kunden). Dieser Wert kann aufgrund von Rundungs- oder Kürzungsfehlern nicht exakt sein.   |
| Währung des Herausgebers (Publisher Currency, PC)        | Die vom Herausgeber für Auszahlungen bevorzugte Währung.                               |
| Geschätzter Preis (PC)           | Der geschätzte Preis für eine Verbrauchseinheit für eine bestimmte SKU basierend auf dem Wechselkurs an dem Datum, an dem die Nutzung berechnet wird (in der Währung des Herausgebers). Dieser Wert kann aufgrund von Rundungs- oder Kürzungsfehlern nicht exakt sein.   |
| Geschätzte erweiterte Gebühren (PC) | Die geschätzten erweiterten Gebühren für die Menge der Verbrauchseinheiten für eine bestimmte SKU basierend auf dem Wechselkurs an dem Datum, an dem die Nutzung berechnet wird (in der Währung des Herausgebers). Dieser Wert kann aufgrund von Rundungs- oder Kürzungsfehlern nicht exakt sein. |
| Geschätzte Auszahlung (PC)          | Die geschätzte Zahlung für die Menge der Verbrauchseinheiten für eine bestimmte SKU basierend auf dem Wechselkurs an dem Datum, an dem die Nutzung berechnet wird (in der Währung des Herausgebers). Dieser Wert kann aufgrund von Rundungs- oder Kürzungsfehlern nicht exakt sein.   |
| Nutzungsreferenz                | Der Bezeichner für einen oder mehrere Tage der Nutzung einer bestimmten SKU durch den Kunden, der einem Eintrag im Auszahlungsbericht zugeordnet ist. |
|  |  |
