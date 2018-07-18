---
title: Azure Service Fabric-Ereignisse | Microsoft-Dokumentation
description: Erfahren Sie, wie Service Fabric-Ereignisse vorkonfiguriert bereitgestellt werden, um Sie bei der Überwachung Ihres Azure Service Fabric-Clusters zu unterstützen.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: b9372c806eab1b0ca69ba078d972b076c8a7d6f6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212717"
---
# <a name="service-fabric-events"></a>Service Fabric-Ereignisse 

Die Service Fabric-Plattform schreibt mehrere strukturierte Ereignisse für die wichtigsten operativen Aktivitäten, die innerhalb des Clusters stattfinden. Diese reichen von Cluster-Upgrades bis hin zu Replikatplatzierungsentscheidungen. Jedes Ereignis, das Service Fabric verfügbar macht, ist einer der folgenden Entitäten im Cluster zugeordnet:
* Cluster
* Anwendung
* Dienst
* Partition
* Replikat 
* Container

Eine vollständige Liste der von der Plattform verfügbar gemachten Ereignisse finden Sie unter [Auflistung der Service Fabric-Ereignisse](service-fabric-diagnostics-event-generation-operational.md).

Nachfolgend sind Sie einige Beispiele für wichtige Szenarien aufgeführt, für die Sie Ereignisse in Ihrem Cluster anzeigen sollten. 
1. Lebenszyklusereignisse von Knoten: Ereignisse wie das Erstellen, Entfernen, Aktivieren, Deaktivieren oder Neustarten von Knoten werden verfügbar gemacht und geben Ihnen einen Überblick über das, was passiert ist. Mit ihrer Hilfe können Sie feststellen, ob es ein Problem mit dem Computer gibt, oder ob eine über Service Fabric aufgerufene API den Status eines Knotens geändert hat.
1. Clusterupgrade: Bei einem Clusterupgrade (SF-Version oder Konfigurationsänderung) sehen Sie, wie das Upgrade initiiert, in den einzelnen Upgradedomänen (UDs) angewendet und abgeschlossen wird (oder ein Rollback erfolgt). 
1. Anwendungsupgrades: Ähnlich wie bei Clusterupgrades gibt es eine komplexe Reihe von Ereignissen während der Anwendung des Upgrades. Es kann hilfreich sein, diese Ereignisse zu verstehen: wann ein Upgrade geplant wurde, den aktuellen Stand des Upgrades und die allgemeine Abfolge der Ereignisse. Damit können Sie auf praktische Weise zurückverfolgen, welche Upgrades erfolgreich angewandt wurden.
1. Bereitstellen/Löschen von Anwendungen/Diensten: Es gibt Ereignisse für jede Anwendung, jeden Dienst und jeden Container, die bzw. der erstellt oder gelöscht wird.
1. Partitionsverschiebungen (Neukonfiguration): Jedes Mal, wenn eine statusbehaftete Partition neu konfiguriert wird (Änderung in der Replikatgruppe), wird ein Ereignis protokolliert. Dies ist hilfreich, wenn Sie verstehen möchten, wie oft die Replikatgruppe der Partition geändert wird, bzw. um nachzuverfolgen, in welchem Knoten das primäre Replikat zu einem beliebigen Zeitpunkt ausgeführt wurde.
1. Chaos-Ereignisse: Wenn Sie den [Chaos](service-fabric-controlled-chaos.md)-Dienst von Service Fabric nutzen, werden bei jedem Start oder Anhalten des Diensts oder beim Einfügen eines Fehlers in das System Ereignisse protokolliert.
1. Integritätsereignisse: Service Fabric macht jedes Mal Integritätsereignisse verfügbar, wenn ein Integritätsbericht mit dem Ergebnis „Warning“ oder „Error“ erstellt wird, eine Entität wieder in den Integritätsstatus „OK“ wechselt oder ein Integritätsbericht abläuft. Diese Ereignisse sind zum Nachverfolgen von Integritätsverlaufsstatistiken für eine Entität sehr hilfreich. 

## <a name="how-to-access-events"></a>Zugriff auf Ereignisse

Für den Zugriff auf Service Fabric-Ereignisse gibt es einige unterschiedliche Möglichkeiten:
* Über den Betriebskanal Diese Ereignisse können über die Azure-Diagnoseerweiterung erfasst und zur Nutzung oder Einspeisung in ein Tool wie OMS Log Analytics an eine Speichertabelle gesendet werden. Wenn für einen Cluster „Diagnose“ aktiviert ist, wird der Azure-Diagnose-Agent in Ihrem Cluster bereitgestellt und standardmäßig so konfiguriert, dass er Protokolle aus dem Betriebskanal einliest. Erfahren Sie mehr dazu, wie Sie den [Azure-Diagnose-Agent](service-fabric-diagnostics-event-aggregation-wad.md) so konfigurieren, dass er die Diagnosekonfiguration des Clusters ändert, um mehr Protokolle oder Leistungsindikatoren aufzunehmen. 
* Über die REST-API des EventStore-Diensts, über die Sie den Cluster direkt abfragen können, oder über die Service Fabric-Clientbibliothek Lesen Sie hierzu [Abfragen von EventStore-APIs nach Clusterereignissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Überwachen Ihres Clusters finden Sie unter [Überwachen des Clusters und der Plattform](service-fabric-diagnostics-event-generation-infra.md).
* Unter [Übersicht über den EventStore-Dienst](service-fabric-diagnostics-eventstore.md) erfahren Sie mehr über den EventStore-Dienst.
