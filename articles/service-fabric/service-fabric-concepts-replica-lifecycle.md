---
title: Replikate und Instanzen in Azure Service Fabric | Microsoft-Dokumentation
description: Grundlegendes zu Replikaten, Instanzen, deren Funktion und Lebenszyklus
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: 
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: aprameyr
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 794cbed0f5072bcc1c18343b9896aad464861c45
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="replicas-and-instances"></a>Replikate und Instanzen 
Dieser Artikel bietet einen Überblick über den Lebenszyklus von Replikaten zustandsbehafteter Dienste und Instanzen zustandsloser Dienste.

## <a name="instances-of-stateless-services"></a>Instanzen zustandsloser Dienste
Eine Instanz eines zustandslosen Diensts ist eine Kopie der Dienstlogik, die auf einem der Knoten des Clusters ausgeführt wird. Eine Instanz innerhalb einer Partition wird durch die Instanz-ID eindeutig identifiziert. Der Lebenszyklus einer Instanz kann durch folgendes Diagramm im Modell dargestellt werden:

![Lebenszyklus einer Instanz](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Sobald der Cluster Resource Manager eine Platzierung für die Instanz bestimmt, erhält sie diesen Lebenszykluszustand. Zu diesem Zeitpunkt wird die Instanz auf dem Knoten gestartet (der Anwendungshost wird gestartet, die Instanz erstellt und dann geöffnet). Nach dem Start geht die Instanz in den Zustand „Bereit“ über. 

Wenn der Anwendungshost oder Knoten für diese Instanz abstürzt, geht sie in den Zustand „Verworfen“ über.

### <a name="ready-rd"></a>Bereit (Ready, RD)
Im Zustand „Bereit“ wird die Instanz auf dem Knoten ausgeführt. Wenn es sich um einen zuverlässigen Dienst handelt, wurde RunAsync aufgerufen. 

Wenn der Anwendungshost oder Knoten für diese Instanz abstürzt, geht sie in den Zustand „Verworfen“ über.

### <a name="closing-cl"></a>Schließen (Closing, CL)
Im Zustand „Schließen“ fährt Service Fabric die Instanz auf diesem Knoten herunter. Dafür kommen zahlreiche Ursachen infrage – z.B. Anwendungsupgrades, Lastenausgleich oder das Löschen des Diensts. Sobald das Herunterfahren abgeschlossen ist, geht sie in den Zustand „Verworfen“ über.

### <a name="dropped-dd"></a>Verworfen (Dropped, DD)
Im Zustand „Verworfen“ wird die Instanz nicht mehr auf dem Knoten ausgeführt. An diesem Punkt behält Service Fabric Metadaten über diese Instanz bei (die verzögert gelöscht werden).

> [!NOTE]
> Mithilfe der Option ForceRemove unter `Remove-ServiceFabricReplica` ist jederzeit der Übergang von einem beliebigen Zustand in den Zustand „Verworfen“ möglich.
>

## <a name="replicas-of-stateful-services"></a>Replikate zustandsbehafteter Dienste
Eine Instanz eines zustandsbehafteten Diensts ist eine Kopie der Dienstlogik, die auf einem der Knoten des Clusters ausgeführt wird. Darüber hinaus behält das Replikat eine Kopie des Zustands dieses Diensts bei. Es gibt zwei verwandte Konzepte, die den Lebenszyklus und das Verhalten zustandsbehafteter Replikate beschreiben:
- Replikatlebenszyklus
- Replikatrolle

Im Folgenden werden persistierte zustandsbehaftete Dienste erläutert. Für flüchtige (oder im Arbeitsspeicher befindliche) zustandsbehaftete Dienste sind die Zustände „Ausgefallen“ und „Verworfen“ äquivalent.

![Replikatlebenszyklus](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Ein InBuild-Replikat ist ein Replikat, das gerade erstellt oder für das Hinzufügen zur Replikatgruppe vorbereitet wird. Abhängig von der Replikatrolle unterscheidet sich die Semantik von IB. 

Wenn der Anwendungshost oder der Knoten für ein InBuild-Replikat abstürzt, geht es in den Zustand „Ausgefallen“ über.

####<a name="primary-inbuild-replicas"></a>Primäre InBuild-Replikate 
Primäre InBuild-Replikate sind die ersten Replikate für eine Partition. Dies geschieht in der Regel dann, wenn die Partition erstellt wird. Primäre InBuild-Replikate entstehen auch, wenn alle Replikate einer Partition neu gestartet oder verworfen werden.

####<a name="idlesecondary-inbuild-replicas"></a>IdleSecondary-InBuild-Replikate
Hierbei kann es sich zum einen um neue Replikate handeln, die vom Cluster Resource Manager erstellt werden. Sie können auch vorhandene Replikate sein, die ausgefallen sind und wieder der Gruppe hinzugefügt werden müssen. Diese Replikate werden vom primären Replikat mit Anfangsdaten gefüllt oder erstellt, bevor sie der Replikatgruppe als ActiveSecondary hinzugefügt werden und an der Quorumsbestätigung von Vorgängen teilnehmen können.

####<a name="activesecondary-inbuild-replicas"></a>ActiveSecondary-InBuild-Replikate
Dieser Zustand kann möglicherweise bei einigen Abfragen beobachtet werden. Er ist eine Optimierung, bei der die Replikatgruppe nicht geändert wird, aber ein Replikat erstellt werden muss. Das Replikat selbst folgt den normalen Zustandscomputerübergängen (wie im Abschnitt für Replikatrollen beschrieben).

### <a name="ready-rd"></a>Bereit (Ready, RD)
Ein Bereit-Replikat ist ein Replikat, das an der Replikation und an Quorumsbestätigungen von Vorgängen beteiligt ist. Der Zustand „Bereit“ gilt für primäre und aktive sekundäre Replikate.

Wenn der Anwendungshost oder der Knoten für ein Bereit-Replikat abstürzt, geht es in den Zustand „Ausgefallen“ über.

### <a name="closing-cl"></a>Schließen (Closing, CL)
Ein Replikat wechselt in den folgenden Szenarien in den Zustand „Schließen“:

- **Herunterfahren des Codes für das Replikat**: Service Fabric muss möglicherweise den ausgeführten Code für ein Replikat herunterfahren. Hierfür kommen viele Gründe infrage, z.B. Anwendungs-, Fabric- oder Infrastrukturupgrade oder ein vom Replikat gemeldeter Fehler etc. Wenn das Replikat geschlossen ist, geht es in den Zustand „Ausgefallen“ über. Der auf dem Datenträger gespeicherte, diesem Replikat zugeordnete persistierte Zustand ist nicht bereinigt.

- **Entfernen des Replikats aus dem Cluster**: Service Fabric muss möglicherweise den persistierten Zustand entfernen und den ausgeführten Code für ein Replikat herunterfahren. Hierfür kommen viele Gründe infrage, z.B. Lastenausgleich

### <a name="dropped-dd"></a>Verworfen (Dropped, DD)
Im Zustand „Verworfen“ wird die Instanz nicht mehr auf dem Knoten ausgeführt. Es ist auch kein Zustand auf dem Knoten verblieben. An diesem Punkt behält Service Fabric Metadaten über diese Instanz bei (die schließlich verzögert gelöscht werden).

### <a name="down-d"></a>Ausgefallen (Down, D)
Im Zustand „Ausgefallen“ wird der Replikatscode nicht ausgeführt, aber der persistierte Zustand für dieses Replikat ist auf diesem Knoten vorhanden. Ein Replikat kann aus zahlreichen Gründen ausgefallen sein, z.B. Knotenausfall, Abstürze im Replikatscode, Anwendungsupgrade, Replikatfehler usw.

Ein ausgefallenes Replikat wird nach Bedarf von Service Fabric geöffnet, z.B. wenn das Upgrade auf dem Knoten abgeschlossen ist.

Die Replikatrolle ist im Zustand „Ausgefallen“ nicht relevant.

### <a name="opening-op"></a>Öffnen (Opening, OP)
Ein ausgefallenes Replikat wechselt in den Zustand „Öffnen“, wenn Service Fabric das Replikat wieder aufrufen muss. Dies kann z.B. nach Abschluss eines Codeupgrades für die Anwendung auf einem Knoten usw. der Fall sein. 

Wenn der Anwendungshost oder der Knoten für ein sich öffnendes Replikat abstürzt, geht es in den Zustand „Ausgefallen“ über.

Die Replikatrolle ist im Zustand „Öffnen“ nicht relevant.

### <a name="standby-sb"></a>StandBy (SB)
Ein StandBy-Replikat ist ein Replikat eines persistierten Diensts, das ausgefallen ist und dann geöffnet wurde. Dieses Replikat könnte von Service Fabric verwendet werden, wenn der Replikatgruppe ein anderes Replikat hinzugefügt werden muss (da es bereits einen Teil des Zustands hat und der Erstellungsprozess schneller ist). Nach Ablauf von StandByReplicaKeepDuration wird das StandBy-Replikat verworfen.

Wenn der Anwendungshost oder der Knoten für ein StandBy-Replikat abstürzt, geht es in den Zustand „Ausgefallen“ über.

Die Replikatrolle ist im Zustand „StandBy“ nicht relevant.

> [!NOTE]
> Alle Replikate, die nicht ausgefallen oder verworfen sind, gelten als *aktiv*.
>

> [!NOTE]
> Mithilfe der Option ForceRemove unter `Remove-ServiceFabricReplica` ist jederzeit der Übergang von einem beliebigen Zustand in den Zustand „Verworfen“ möglich.
>

## <a name="replica-role"></a>Replikatrolle 
Die Rolle des Replikats bestimmt seine Funktion in der Replikatgruppe.

- **Primär (P)**: Es gibt ein primäres Replikat in der Replikatgruppe, das für die Ausführung von Lese- und Schreibvorgängen verantwortlich ist. 
- **ActiveSecondary (S)**: Hierbei handelt es sich um Replikate, die Zustandsupdates vom primären Replikat empfangen, anwenden und Bestätigungen senden. In der Replikatgruppe sind mehrere aktive sekundäre Replikate vorhanden, und deren Anzahl bestimmt die Anzahl von Fehlern, die der Dienst behandeln kann.
- **IdleSecondary (I)**: Diese Replikate werden von dem primären Replikat erstellt, d.h. sie empfangen den Zustand vom primären Replikat, bevor sie zum aktiven sekundären Replikat heraufgestuft werden können. 
- **Kein (N)**: Diese Replikate haben in der Replikatgruppe keine Zuständigkeit.
- **Unbekannt (U)**: Dies ist die erste Rolle eines Replikats, bevor es den ChangeRole-API-Aufruf von Service Fabric empfängt.

Der folgende Abschnitt beschreibt Replikatrollenübergänge und einige Beispielszenarien, in denen sie möglicherweise auftreten:

![Replikatrolle](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U -> P: Erstellung eines neuen primären Replikats
- U -> I: Erstellung eines neuen, im Leerlauf befindlichen Replikats
- U -> N: Löschen eines StandBy-Replikats
- I -> S: Heraufstufen des im Leerlauf befindlichen sekundären Replikats zum aktiven sekundären Replikat, sodass es die Teilnahme gegenüber dem Quorum bestätigt.
- I -> P: Heraufstufen des im Leerlauf befindlichen sekundären Replikats zum primären Replikat. Dies kann bei speziellen Neukonfigurationen vorkommen, wenn das im Leerlauf befindliche sekundäre Replikat der richtige Kandidat für das primäre Replikat ist.
- I -> N: Löschen eines im Leerlauf befindlichen sekundären Replikats.
- S -> P: Heraufstufen des aktiven sekundären Replikats zum primären Replikat. Dies kann aufgrund eines Failovers des primären Replikats oder einer Verschiebung des primären Replikats der Fall sein, initiiert durch den Cluster Ressource Manager als Reaktion auf Anwendungsupgrade, Lastenausgleich usw.
- S -> N: Löschen des aktiven sekundären Replikats.
- P -> S: Herabstufung des primären Replikats. Dies kann aufgrund einer Verschiebung des primären Replikats der Fall sein, initiiert durch den Cluster Ressource Manager als Reaktion auf Anwendungsupgrade, Lastenausgleich usw.
- P -> N: Löschen des primären Replikats.

> [!NOTE]
> In komplexen Programmiermodellen, wie z.B. [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md), ist das Konzept der Replikatrolle vor den Entwicklern verborgen. Bei „Reliable Actors“ sind Rollen unnötig, während sie in „Reliable Services“ in den meisten Szenarien stark vereinfacht sind.
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie in den folgenden Artikeln:

- [Reliable Services-Lebenszyklus – C#](service-fabric-reliable-services-lifecycle.md)

