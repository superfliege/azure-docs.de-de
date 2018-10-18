---
title: Grundlegendes zur Anwendung des Rabatts für Reservierungen auf Azure Cosmos DB | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Rabatt für Reservierungen auf bereitgestellten Durchsatz (RU/s) in Azure Cosmos DB angewendet wird.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: cwatson
ms.reviewer: sngun
ms.openlocfilehash: adcd91a8f1b3368d03f4b634e7aef40104d953e3
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393637"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Grundlegendes zur Anwendung des Rabatts für Reservierungen auf Azure Cosmos DB

Nachdem Sie eine reservierte Azure Cosmos DB-Kapazität erworben haben, wird der Reservierungsrabatt automatisch auf Azure Cosmos DB-Ressourcen angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt den für Azure Cosmos DB-Ressourcen bereitgestellten Durchsatz ab. Sie deckt jedoch keine Gebühren für Software, Netzwerke, Speicher oder vordefinierte Container ab.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>Auf Azure Cosmos DB-Konten angewendeter Rabatt für Reservierungen

Auf den [bereitgestellten Durchsatz](../cosmos-db/request-units.md) wird ein Rabatt für Reservierungen in Form von Anforderungseinheiten pro Sekunde (RU/s) auf Stundenbasis angewendet. Bei Azure Cosmos DB-Ressourcen, die nicht über die gesamte Stunde hinweg ausgeführt werden, wird der Reservierungsrabatt automatisch auf andere Cosmos DB-Ressourcen angewendet, die den Reservierungsattributen entsprechen. Der Rabatt kann auf Azure Cosmos DB-Ressourcen angewendet werden, die gleichzeitig ausgeführt werden. Wenn die den Reservierungsattributen entsprechenden Cosmos DB-Ressourcen nicht über die gesamte Stunde hinweg ausgeführt werden, profitieren Sie nicht im vollen Umfang vom Reservierungsrabatt für diese Stunde.

* Die Rabatte sind gestaffelt, was bedeutet, dass Reservierungen mit mehr Anforderungseinheiten höhere Rabatte ermöglichen.  
* Mit dem Erwerb von Reservierungen werden Rabatte auf alle Regionen im Verhältnis zu den regionalen On-Demand-Preisen angewendet. Informationen zu den Reservierungsrabattsätzen in den einzelnen Regionen finden Sie im Abschnitt [Reservierungsrabatte nach Region](#reservation-discount-per-region) dieses Artikels.

## <a name="reservation-discount-per-region"></a>Reservierungsrabatte nach Region
Reservierungsrabatte werden auf Azure Cosmos DB-Durchsatzkosten auf Stundenbasis pro Abonnement oder pro registriertem Umfang/Kontobereich angewendet. Der Reservierungsrabatt wird auf Verbrauchseinheiten in den verschiedenen Regionen mit folgenden Sätzen angewendet:

|Beschreibung der Verbrauchseinheit  |Region |Verhältnis  |
|---------|---------|---------|
|Azure Cosmos DB – 100 RU/s pro Stunde – Asien-Pazifik, Südosten  |  Asien-Pazifik, Südosten    |   1      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Asien-Pazifik, Osten |   Asien-Pazifik, Osten   |    1     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Europa, Norden|  Europa, Norden       |    1     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Südkorea, Süden|    Südkorea, Süden     |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – Europa, Westen|    Europa, Westen     |      1   |
|Azure Cosmos DB – 100 RU/s pro Stunde – Südkorea, Mitte|   Südkorea, Mitte    |       1  |
|Azure Cosmos DB – 100 RU/s pro Stunde – Vereinigtes Königreich, Süden|   UK, Süden      |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – Vereinigtes Königreich, Westen|   UK, Westen      |    1     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Vereinigtes Königreich, Norden |   Großbritannien, Norden    |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – Vereinigtes Königreich, Süden 2|   Großbritannien, Süden 2      |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Osten 2|  USA (Ost 2)     |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Norden-Mitte|   USA, Norden-Mitte      |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Westen|   USA, Westen      |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Mitte| USA, Mitte        |     1    |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Westen 2|   USA, Westen 2      |      1   |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Westen-Mitte|   USA, Westen-Mitte      |       1  |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Osten|   USA, Osten      |  1       |
|Azure Cosmos DB – 100 RU/s pro Stunde – Südafrika, Norden|     Südafrika, Norden    |   1      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Südafrika, Westen |    Südafrika, Westen      |    1     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Indien, Süden|    Indien, Süden     |    1,0375    |
|Azure Cosmos DB – 100 RU/s pro Stunde – Kanada, Osten|   Kanada, Osten      |    1.1      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Japan, Osten|   Japan, Osten      |    1,125     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Japan, Westen|     Japan, Westen    |   1,125       |
|Azure Cosmos DB – 100 RU/s pro Stunde – Indien, Westen|     Indien, Westen    |    1,1375     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Indien, Mitte|    Indien, Mitte     |  1,1375       |
|Azure Cosmos DB – 100 RU/s pro Stunde – Australien, Osten|     Australien, Osten    |   1,15       |
|Azure Cosmos DB – 100 RU/s pro Stunde – Kanada, Mitte|  Kanada, Mitte       |   1.2       |
|Azure Cosmos DB – 100 RU/s pro Stunde – Frankreich, Mitte|   Frankreich, Mitte      |    1,25      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Brasilien, Süden|  Brasilien, Süden       |   1.5      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Australien, Mitte|   Australien, Mitte      |   1.5      |
|Azure Cosmos DB – 100 RU/s pro Stunde – Australien, Mitte 2| Australien, Mitte 2        |    1.5     |
|Azure Cosmos DB – 100 RU/s pro Stunde – Frankreich, Süden|    Frankreich, Süden     |    1,625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Szenarios zur Anwendung des Rabatts für Reservierungen

Für eine Reservierung gelten folgende Anforderungen:

* Erforderlicher Durchsatz: 50.000 RU/s  
* Genutzte Regionen: 2. 

In diesem Fall werden Ihnen On-Demand-Gebühren für 500 Einheiten zu 100 RU/s Verbrauchseinheiten in diesen beiden Regionen berechnet. Der Gesamtverbrauch beträgt 100.000 RU/s pro Stunde. 

**Szenario 1**

Beispiel: Sie benötigen Azure Cosmos DB-Bereitstellungen in den Regionen „USA, Norden-Mitte“ und „USA, Westen“ und jede Region verbraucht einen Durchsatz von 50.000 RU/s. Ein Reservierungserwerb von 100.000 RU/s würde Ihre On-Demand-Gebühren vollständig ausgleichen.

Der durch eine Reservierung abgedeckte Rabatt wird berechnet als (verbrauchter Durchsatz * Reservierungsrabattsatz_für_diese_Region). Für die Regionen „USA, Norden-Mitte“ und „USA, Westen“ beträgt der Reservierungsrabattsatz „1“. Somit wird für insgesamt 100.000 RU/s ein Rabatt gewährt. (Dieser Wert berechnet sich wie folgt: 50.000 * 1 + 50.000 * 1 = 100,000 RU/s.) Sie müssen keine zusätzlichen Gebühren zu den üblichen Tarifen der nutzungsbasierten Bezahlung zahlen. 

|Beschreibung der Verbrauchseinheit | Region |Verbrauchter Durchsatz (RU/s) |Anwendung des Reservierungsrabatts auf Anforderungseinheiten pro Sekunde |
|---------|---------|---------|---------|
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Norden-Mitte  |   USA, Norden-Mitte  | 50.000  | 50.000  |
|Azure Cosmos DB – 100 RU/s pro Stunde – USA, Westen  |  USA, Westen   |  50.000  |  50.000 |

**Szenario 2**

Beispiel: Sie benötigen Azure Cosmos DB-Bereitstellungen in den Regionen „Australien, Mitte 2“ und „Frankreich, Süden“ und jede Region verbraucht einen Durchsatz von 50.000 RU/s. Hier würde der folgende Reservierungserwerb von 100.000 RU/s wie folgt gelten (vorausgesetzt, der Verbrauch für „Australien, Mitte 2“ wird zuerst rabattiert):

|Beschreibung der Verbrauchseinheit | Region |Verbrauchter Durchsatz (RU/s) |Anwendung des Reservierungsrabatts auf Anforderungseinheiten pro Sekunde |
|---------|---------|---------|---------|
|Azure Cosmos DB – 100 RU/s pro Stunde – Australien, Mitte 2  |  Australien, Mitte 2   |  50.000  |  50.000   |
|Azure Cosmos DB – 100 RU/s pro Stunde – Frankreich, Süden  |  Frankreich, Süden   |  50.000 |  15.384  |

50.000 Verbrauchseinheiten in der Region „Australien, Mitte 2“ entsprechen 75.000 RU/s abrechenbarem Verbrauch (oder normalisertem Verbrauch). Dieser Wert berechnet sich aus verbrauchtem Durchsatz * Reservierungsrabattsatz_für_diese_Region. Das Ergebnis entspricht einem abrechenbaren oder normaliserten Verbrauch von 75.000 RU/s. (Dieser Wert berechnet sich wie folgt: 50.000 * 1,5 = 75.000 RU/s.) 

Ein Reservierungserwerb von 100.000 RU/s würde die 75.000 RU/s in der Region „Australien, Mitte 2“ ausgleichen und 25.000 RU/s für die Region „Frankreich, Süden“ übrig lassen. Von den restlichen 25.000 RU/s für die Region „Frankreich, Süden“ wird auf 15.384 RU/s ein Reservierungsrabatt gewährt. (Dieser Wert berechnet sich wie folgt: 25.000 / 1,625 = 15.384 RU/s.) Die restlichen 34.616 RU/s in der Region „Frankreich, Süden“ werden mit den üblichen Tarifen der nutzungsbasierten Bezahlung berechnet. 

Vom Azure-Abrechnungssystem wird der Abrechnungsvorteil für Reservierungen der Instanz zugewiesen, die als erste verarbeitet wird und der Reservierungskonfiguration entspricht (in diesem Fall der Region „Australien, Mitte 2“).

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Azure-Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

* [Was sind Azure-Reservierungen?](../billing/billing-save-compute-costs-reservations.md)  
* [Vorauszahlen für Azure Cosmos DB-Ressourcen mit reservierter Azure Cosmos DB-Kapazität](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)  
* [Verwalten von Azure-Reservierungen](../billing/billing-manage-reserved-vm-instance.md)  
* [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../billing/billing-understand-reserved-instance-usage.md)  
* [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md)  
* [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

