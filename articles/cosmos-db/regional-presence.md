---
title: Regionale Präsenz von Azure Cosmos DB
description: In diesem Artikel werden die regionale Präsenz von Azure Cosmos DB und die verschiedenen verfügbaren Cloudumgebungen beschrieben.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: rimman
ms.openlocfilehash: 7c060fae389766e89a84e2f6779209ad31edb031
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52166718"
---
# <a name="regional-presence-of-azure-cosmos-db"></a>Regionale Präsenz von Azure Cosmos DB

Azure ist derzeit in [54 Regionen](https://azure.microsoft.com/global-infrastructure/regions/) weltweit verfügbar. Azure Cosmos DB ist ein grundlegender Dienst von Azure und in allen Regionen verfügbar, in denen auch Azure verfügbar ist.

![Regionale Verfügbarkeit von Azure Cosmos DB](./media/regional-presence/regional-presence.png)

Cosmos DB ist in allen fünf Azure-Cloudumgebungen verfügbar, die für Kunden bereitgestellt werden:

* **Öffentliche Azure-Cloud**, die global verfügbar ist.

* **Azure China** ist dank einer exklusiven Partnerschaft zwischen Microsoft und 21Vianet, einem der größten Internetanbieter des Landes, verfügbar.

* Für **Azure Deutschland** werden Dienste basierend auf einem Datentreuhänder-Modell bereitgestellt. Hiermit wird sichergestellt, dass Kundendaten in Deutschland und unter der Kontrolle von T-Systems International GmbH, einem Subunternehmen von Deutsche Telekom, verbleiben. Dieses Unternehmen fungiert als Datentreuhänder in Deutschland.

* **Azure Government** ist in vier Regionen der USA für US-Regierungsbehörden und ihre Partner verfügbar. 

* **Azure Government für das US-Verteidigungsministerium** ist in zwei Regionen der USA für das US-Verteidigungsministerium verfügbar.

## <a name="regional-presence-with-global-distribution"></a>Regionale Präsenz mit globaler Verteilung

Verschiedene APIs, die von Azure Cosmos DB verfügbar gemacht werden (z.B. SQL, MongoDB, Cassandra, Gremlin und Azure Table Storage), stehen in allen Azure-Regionen zur Verfügung. Beispielsweise können Sie MongoDB- und Cassandra-APIs von Azure Cosmos DB nicht nur in allen globalen Azure-Regionen verwenden, sondern auch in Azure-Regionen mit Datenhoheit, z.B. China, Deutschland, Government und Verteidigungsministerium (USA).

Azure Cosmos DB ist eine [global verteilte](distribute-data-globally.md) Datenbank. Sie können Ihrem Azure Cosmos-Konto eine beliebige Anzahl von Azure-Regionen zuordnen, und Ihre Daten werden automatisch und auf transparente Weise repliziert. Sie können für Ihr Azure Cosmos-Konto jederzeit eine Region hinzufügen oder entfernen. Mit der Funktion für die „schlüsselfertige“ globale Verteilung und dem Multimaster-Replikationsprotokoll ermöglicht Azure Cosmos DB Lese- und Schreiblatenzen von weniger als 10 ms (99. Perzentil), eine Lese- und Schreibverfügbarkeit von 99,999% und die Möglichkeit zum elastischen Skalieren des bereitgestellten Durchsatzes für Lese- und Schreibvorgänge über alle Regionen hinweg, die Ihrem Azure Cosmos-Konto zugeordnet sind. Azure Cosmos DB umfasst außerdem fünf gut definierte Konsistenzmodelle, und Sie können auswählen, dass ein spezifisches Konsistenzmodell auf Ihre Daten angewendet werden soll. Azure Cosmos DB ist zudem der einzige Datenbankdienst der Branche mit einer umfassenden Vereinbarung zum Servicelevel (SLA), die den bereitgestellten Durchsatz, die Latenz im 99. Perzentil, Hochverfügbarkeit und Konsistenz abdeckt.

## <a name="next-steps"></a>Nächste Schritte

Sie können sich in den folgenden Artikeln nun über unterschiedliche Konzepte von Azure Cosmos DB informieren:

* [Globale Datenverteilung](distribute-data-globally.md)
* [Verwalten eines Azure Cosmos DB-Kontos](manage-account.md)
* [Bereitstellen von Durchsatz für Cosmos DB-Container und -Datenbanken](set-throughput.md)
* [Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
