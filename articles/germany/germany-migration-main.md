---
title: Migration von Azure Deutschland zu Azure weltweit
description: Einführung in die Migration von Azure Deutschland zu Azure weltweit
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 750f58f825071e9b84b4fa40404088544af72f15
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346267"
---
# <a name="introduction-to-migration-guidance-for-azure-germany"></a>Einführung in die Migrationsanleitung für Azure Deutschland

Diese Artikel bieten Anleitungen, um Ihre Workloads von Azure Deutschland zu Azure weltweit zu migrieren. Obwohl Azure Tools für die Migration von Ressourcen im [Azure Migration Center](https://azure.microsoft.com/migration/) bereitstellt, sind einige dieser Tools nur für Migrationen innerhalb desselben Mandanten oder derselben Region konzipiert.

Die beiden Regionen in Deutschland sind streng von Azure weltweit getrennt, einschließlich separater Azure Active Directory-Verzeichnisse für jede Cloud. Daher sind die Azure-Mandanten für Azure weltweit und Azure Deutschland immer unterschiedlich. Einige der Standardmigrationstools basieren auf dem Verschieben von Ressourcen innerhalb des *gleichen* Mandanten. Für die Migration zwischen *verschiedenen* Mandanten finden Sie unten eine Liste der verfügbaren Tools.

## <a name="migration-process"></a>Migrationsvorgang

Ihr Weg zur Migration der Workload von Azure Deutschland nach Azure weltweit folgt in der Regel ähnlichen Prozessen, wie sie für die Migration von Anwendungen in die Cloud verwendet werden.

![Bewerten > Planen > Migrieren > Überprüfen](./media/germany-migration-main/migration-steps.png)

### <a name="assess"></a>Bewerten

- Verstehen Sie den Fußabdruck Ihres Unternehmens bezüglich Azure Deutschland, indem Sie Azure Kontobesitzer, Abonnementadministratoren, Mandantenadministratoren und Finanz-/Buchhaltungsteams zusammenbringen. Zusammen bieten diese ein vollständiges Bild der Azure-Nutzung für große Unternehmen.

- Erfassen Sie den Ressourcenbestand.
  - Jeder Abonnementadministrator und Mandantenadministrator führt eine Reihe von Skripts aus, um Ressourcengruppen, die Ressourcen innerhalb jeder der Gruppen und deren Bereitstellungseinstellungen aufzulisten.
  - Dokumentieren Sie anwendungsübergreifende Abhängigkeiten in Azure und mit externen Systemen.
  - Dokumentieren Sie die Anzahl jeder Azure-Ressource und die Menge der Daten für jede Instanz, die migriert werden müssen.
  - Stellen Sie sicher, dass die Anwendungsarchitekturdokumente mit der Liste der Azure-Ressourcen konsistent sind.

Am Ende dieser Phase sind die folgenden Informationen bekannt:

- Eine vollständige Liste der Azure-Ressourcen, die verwendet werden,
- Abhängigkeiten innerhalb dieser Ressourcen und
- die Komplexität des Migrationsaufwands.

### <a name="plan"></a>Plan

- Verwenden Sie die Ausgabe der Abhängigkeitsanalyse aus der Bewertungsphase, um verwandte Komponenten zu definieren. Ziehen Sie die gemeinsame Migration in einem „**Migrationspaket**“ in Betracht.
- [Optional] Nutzen Sie diese Migrationsmöglichkeit, um [Gartner 5-R-Kriterien](https://www.gartner.com/newsroom/id/1684114) anzuwenden und Ihre Workload zu optimieren.
- Bestimmen Sie die Zielumgebung in Azure weltweit.
  - Identifizieren Sie den Zielmandanten für Azure weltweit (wenn Ihre Organisation noch keine Präsenz besitzt, erstellen Sie eine solche). 
  - Erstellen Sie Abonnements.
  - Wählen Sie den Standort für Azure weltweit aus, zu dem Sie migrieren möchten.
  - Führen Sie Testmigrationsszenarien von Azure Deutschland zu Azure weltweit aus, die mit Ihrer Architektur übereinstimmen.
- Bestimmen Sie die geeignete Zeitachse bzw. den geeigneten Zeitplan der Migration sowie einen Benutzerakzeptanz-Testplan für jedes Migrationspaket.

### <a name="migrate"></a>Migrieren

- Verwenden Sie die Tools, Techniken und Empfehlungen in diesem Dokument, um neue Ressourcen in Azure weltweit zu erstellen und Anwendungen zu konfigurieren.

### <a name="validate"></a>Überprüfen

- Führen Sie Benutzerakzeptanztests aus.
- Stellen Sie sicher, dass Anwendungen wie erwartet ausgeführt werden.
- Synchronisieren Sie aktuelle Daten in der Zielumgebung, falls zutreffend.
- Führen Sie die Umstellung auf die neue Anwendungsinstanz in Azure weltweit aus.
- Bestätigen Sie, dass die Produktionsumgebung wie erwartet funktioniert.
- Nehmen Sie Ressourcen in Azure Deutschland außer Betrieb.

## <a name="terms"></a>Begriffe

Diese Begriffe werden in den folgenden Migrationsartikeln verwendet:

*Quelle* beschreibt Ihren Ausgangspunkt (z.B. Azure Deutschland):

- Quellmandantenname: Der Name des Mandanten in Azure Deutschland (alles nach dem @-Zeichen im Kontonamen). Die Mandantennamen in Azure Deutschland enden alle auf *microsoftazure.de*.
- Quellmandanten-ID: Die ID des Mandanten in Azure Deutschland. Die Mandanten-ID wird im Azure-Portal angezeigt, wenn Sie den Mauszeiger über den Kontonamen in der oberen rechten Ecke bewegen.
- Quellabonnement-ID: Sie können mehrere Abonnements unter dem gleichen Mandanten besitzen. Stellen Sie immer sicher, dass Sie das richtige Abonnement verwenden.
- Quellregion: „**Deutschland, Mitte**“ („**germanycentral**“) oder „**Deutschland, Nordosten** („**germanynortheast**“) abhängig davon, wo sich die zu migrierende Ressource befindet.

*Ziel* beschreibt, wohin Sie migrieren:

- Zielmandantenname: wie der Quellmandantenname, aber in Azure, öffentlich.
- Zielmandanten-ID: wie Quellmandanten-ID.
- Zielabonnement-ID: wie Quellabonnement-ID.
- Zielregion: Sie können nahezu jede Region in Azure weltweit verwenden, aber in den meisten Fällen möchten Sie wahrscheinlich zu „**Europa, Westen**“ („**westeurope**“) oder „**Europa, Norden**“ („**northeurope**“) migrieren.

> [!NOTE]
> Stellen Sie sicher, dass der Dienst, den Sie migrieren, in der Zielregion angeboten wird. Alle Azure-Dienste, die in Azure Deutschland verfügbar sind, stehen in **Europa, Westen** zur Verfügung. Alle Azure-Dienste, die in Azure Deutschland verfügbar sind, sind auch in **Europa, Norden** verfügbar, ausgenommen die G/GS VM-Serie und Machine Learning Studio.

Fügen Sie außerdem das Portal für Azure weltweit zu Ihren Favoritenlinks im Browser hinzu. Das Azure Deutschland-Portal steht unter [https://portal.microsoftazure.de/](https://portal.microsoftazure.de/) zur Verfügung, das Portal für Azure weltweit erreichen Sie unter [https://portal.azure.com/](https://portal.azure.com/).

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Tools, Techniken und Empfehlungen zur Migration von Ressourcen der folgenden Dienstkategorien:

- [Compute](./germany-migration-compute.md)
- [Netzwerk](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Datenbanken](./germany-migration-databases.md)
- [Analyse](./germany-migration-analytics.md)
- [Internet der Dinge (IoT)](./germany-migration-iot.md)
- [Integration](./germany-migration-integration.md)
- [Identität](./germany-migration-identity.md)
- [Sicherheit](./germany-migration-security.md)
- [Verwaltungstools](./germany-migration-management-tools.md)
- [Medien](./germany-migration-media.md)
