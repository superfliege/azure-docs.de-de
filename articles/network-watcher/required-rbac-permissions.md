---
title: Erforderliche Berechtigungen für die Verwendung von Azure Network Watcher-Funktionen | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, welche Azure RBAC-Berechtigungen (Role-Based Access Control) zum Arbeiten mit Network Watcher-Funktionen erforderlich sind.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jdial
ms.openlocfilehash: 09f3a1e1d9c6796cb55ae8f0ab18bf8e1b3fa198
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077755"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Erforderliche RBAC-Berechtigungen (Role-Based Access Control) für die Verwendung von Network Watcher-Funktionen

Durch Azure RBAC-Berechtigungen (Role-Based Access Control) können Sie Mitgliedern Ihrer Organisation nur bestimmte Aktionen zuweisen, die sie benötigen, um die ihnen übertragenen Aufgaben durchzuführen. Um die Network Watcher-Funktionen verwenden zu können, muss das Konto, mit dem Sie sich bei Azure anmelden, den internen Rollen [Besitzer](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Mitwirkender](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor) oder [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) zugewiesen werden, die den Aktionen für die einzelnen Network Watcher-Funktionen in den folgenden Abschnitten zugeordnet sind. Weitere Informationen zu Network Watcher-Funktionen finden Sie unter [Was ist Azure Network Watcher?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Network Watcher

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Network Watcher abrufen                                          |
| Microsoft.Network/networkWatchers/write                             | Network Watcher erstellen oder aktualisieren                             |
| Microsoft.Network/networkWatchers/delete                            | Network Watcher löschen                                       |

## <a name="nsg-flow-logs"></a>NSG-Datenflussprotokolle

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Ein Datenflussprotokoll konfigurieren                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Status für ein Datenflussprotokoll abfragen                                    |

## <a name="connection-troubleshoot"></a>Problembehandlung für Verbindungen

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Ergebnisse von einem Test zur Problembehandlung für Verbindungen abfragen                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Einen Test zur Problembehandlung für Verbindungen ausführen                             |

## <a name="connection-monitor"></a>Verbindungsmonitor

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Verbindungsmonitor starten                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Verbindungsmonitor anhalten                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Verbindungsmonitor abfragen                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Verbindungsmonitor abrufen                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Erstellen eines Verbindungsmonitors                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Verbindungsmonitor löschen                                    |

## <a name="packet-capture"></a>Paketerfassung

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Status einer Paketerfassung abfragen                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Beenden einer Paketerfassung                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Abrufen einer Paketerfassung                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Eine Paketerfassung erstellen                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Löschen einer Paketerfassung                                        |

## <a name="ip-flow-verify"></a>IP-Datenflussüberprüfung

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Einen IP-Fluss überprüfen                                              |

## <a name="next-hop"></a>Nächster Hop

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Nächsten Hop von einer VM abrufen                                     |

## <a name="network-security-group-view"></a>Netzwerksicherheitsgruppen-Ansicht

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Sicherheitsgruppen anzeigen                                           |

## <a name="topology"></a>Topologie

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topologie abrufen                                                   |

## <a name="reachability-report"></a>Erreichbarkeitsbericht

| anzuzeigen.                                                              | NAME                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Einen Azure-Erreichbarkeitsbericht abrufen                               |

## <a name="additional-actions"></a>Zusätzliche Aktionen

Network Watcher-Funktionen erfordern folgende Aktionen:

- Microsoft.Storage/Read
- Microsoft.Authorization/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*