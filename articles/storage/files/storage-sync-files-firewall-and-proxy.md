---
title: Lokale Firewall- und -Proxyeinstellungen von Azure File Sync | Microsoft-Dokumentation
description: Lokale Netzwerkkonfiguration von Azure File Sync
services: storage
documentationcenter: ''
author: fauhse
manager: klaasl
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 979897e3cb703b36a46e96848a9176d6d4c6cc6a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Proxy- und Firewalleinstellungen von Azure File Sync
Azure File Sync verbindet Ihre lokalen Server mit Azure Files, wodurch Synchronisierung für mehrere Standorte und Cloudtiering-Funktionalität ermöglicht werden. Daher muss ein lokaler Server eine Verbindung mit dem Internet haben. Ein IT-Administrator muss den besten Weg festlegen, auf dem der Server zu den Azure-Clouddiensten gelangt.

Dieser Artikel gibt einen Einblick in die speziellen Anforderungen und Optionen, die verfügbar sind, um Ihren Server erfolgreich und sicher mit Azure File Sync zu verbinden.

> [!Important]
> Firewalls und virtuelle Netzwerke werden von Azure File Sync noch nicht für ein Speicherkonto unterstützt. 

## <a name="overview"></a>Übersicht
Azure File Sync fungiert als Orchestrierungsdienst zwischen Ihrem Windows-Server, Ihrer Azure-Dateifreigabe und mehreren anderen Azure-Diensten, um Daten so zu synchronisieren, wie dies in Ihrer Synchronisierungsgruppe beschrieben ist. Damit Azure File Sync korrekt funktioniert, müssen Sie Ihre Server so konfigurieren, dass sie mit den folgenden Azure-Diensten kommunizieren:

- Azure Storage
- Azure-Dateisynchronisierung
- Azure Resource Manager
- Authentifizierungsdienste

> [!Note]  
> Der Azure File Sync Agent unter Windows Server initiiert alle Anforderungen an die Clouddienste, was dazu führt, dass aus Sicht einer Firewall nur ausgehender Datenverkehr berücksichtigt werden muss. <br /> Keiner der Azure-Dienste initiiert eine Verbindung mit dem Azure File Sync-Agent.


## <a name="ports"></a>Ports
Azure File Sync bewegt Dateidaten und Metadaten ausschließlich über HTTPS und erfordert, dass Port 443 für ausgehenden Datenverkehr geöffnet ist.
Daher wird der gesamte Datenverkehr verschlüsselt.

## <a name="networks-and-special-connections-to-azure"></a>Netzwerke und spezielle Verbindungen mit Azure
Der Azure File Sync-Agent hat keine Anforderungen hinsichtlich spezieller Kanäle wie [ExpressRoute](../../expressroute/expressroute-introduction.md) usw. zu Azure.

Azure File Sync nutzt alle verfügbaren Mittel, die Zugriff in Azure ermöglichen, passt sich automatisch an verschiedene Netzwerkeigenschaften wie Bandbreite, Wartezeit an und bietet Administratorsteuerung zur Feinabstimmung. Derzeit sind nicht alle Funktionen verfügbar. Wenn Sie ein bestimmtes Verhalten konfigurieren möchten, können Sie uns dies über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670) mitteilen.

## <a name="proxy"></a>Proxy
Azure File Sync unterstützt derzeit computerweite Proxyeinstellungen. Diese Proxyeinstellung ist für den Azure File Sync-Agent transparent, da der gesamte Datenverkehr des Servers über diesen Proxy geleitet wird.

App-spezifische Proxyeinstellungen werden derzeit entwickelt und werden in einer zukünftigen Version des Azure File Sync-Agents unterstützt. Dies wird es ermöglichen, einen Proxy speziell für Azure File Sync-Datenverkehr zu konfigurieren.

## <a name="firewall"></a>Firewall
Wie in einem vorhergehenden Abschnitt erwähnt, muss Port 443 für ausgehenden Datenverkehr geöffnet sein. Entsprechend den Richtlinien in Ihrem Rechenzentrum, Ihrer Niederlassung oder Ihrer Region kann eine weitere Beschränkung des Datenverkehrs über diesen Port auf bestimmte Domänen erwünscht oder erforderlich sein.

In der folgenden Tabelle sind die für eine Kommunikation erforderlichen Domänen beschrieben:

| Dienst | Domäne | Verwendung |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Jeder Benutzeraufruf (etwa PowerShell) geht an/über diese URL, dazu gehört auch der erstmalige Serverregistrierungsaufruf. |
| **Azure Active Directory** | https://login.windows.net | Azure Resource Manager-Aufrufe müssen von einem authentifizierten Benutzer vorgenommen werden. Um erfolgreich zu sein, wird diese URL für die Benutzerauthentifizierung verwendet. |
| **Azure Active Directory** | https://graph.windows.net/ | Im Rahmen der Bereitstellung von Azure File Sync wird ein Dienstprinzipal in Azure Active Directory des Abonnements erstellt. Diese URL wird dazu verwendet. Dieser Prinzipal wird dazu verwendet, einen minimalen Satz von Berechtigungen an den Azure File Sync-Dienst zu delegieren. Der Benutzer, der die erste Einrichtung von Azure File Sync ausführt, muss ein authentifizierter Benutzer mit Abonnementbesitzerberechtigungen sein. |
| **Azure Storage** | &ast;.core.windows.net | Wenn der Server eine Datei herunterlädt, führt er diese Datenbewegung effizienter aus, wenn er eine direkte Verbindung mit der Azure-Dateifreigabe im Speicherkonto hat. Der Server hat einen SAS-Schlüssel, der nur gezielten Dateifreigabezugriff zulässt. |
| **Azure File Sync**  | &ast;.one.microsoft.com | Nach der erstmaligen Serverregistrierung erhält der Server eine regionale URL für die Azure File Sync-Dienstinstanz in dieser Region. Der Server kann über die URL direkt und effizient mit der Instanz kommunizieren, die seine Synchronisierung verwaltet. |

> [!Important]
> Wenn Datenverkehr über „&ast;.one.microsoft.com“ zugelassen wird, kann Datenverkehr vom Server nicht nur an den Synchronisierungsdienst, sondern auch an weitere Stellen gesendet werden. Es gibt viele weitere Microsoft-Dienste unter Unterdomänen.

Wenn „&ast;.one.microsoft.com“ zu ausgedehnt ist, können Sie die Kommunikation des Servers begrenzen, indem Sie nur explizite regionale Instanzen des Azure File Sync-Diensts zulassen. Welche Instanzen ausgewählt werden müssen, hängt von der Region des Speichersynchronisierungsdienst ab, für den Sie den Server bereitgestellt und registriert haben. Dies ist die Region, die Sie für den Server zulassen müssen. Bald wird es mehr URLs geben, um neue Funktionen für Geschäftskontinuität zu ermöglichen. 

| Region | Regionale Endpunkt-URL für Azure File Sync |
|--------|---------------------------------------|
| Australien (Osten) | https://kailani-aue.one.microsoft.com |
| Kanada, Mitte | https://kailani-cac.one.microsoft.com |
| USA (Ost) | https://kailani1.one.microsoft.com |
| Asien, Südosten | https://kailani10.one.microsoft.com |
| UK, Süden | https://kailani-uks.one.microsoft.com |
| Europa, Westen | https://kailani6.one.microsoft.com |
| USA (Westen) | https://kailani.one.microsoft.com |

> [!Important]
> Wenn Sie diese detaillierten Firewallregeln definieren, sollten Sie dieses Dokument regelmäßig überprüfen und Ihre Firewallregeln aktualisieren, um Dienstunterbrechungen aufgrund veralteter oder unvollständiger URL-Listen in Ihren Firewalleinstellungen zu vermeiden.

## <a name="summary-and-risk-limitation"></a>Zusammenfassung und Risikobegrenzung
Die Listen weiter oben in diesem Dokument enthalten die URLs, mit denen Azure File Sync derzeit kommuniziert. Firewalls müssen in der Lage sein, ausgehenden Datenverkehr zu diesen Domänen sowie Antworten von ihnen zuzulassen. Microsoft ist bestrebt, diese Liste auf dem neuesten Stand zu halten.

Ein Einrichten von domäneneinschränkenden Firewallregeln kann eine Maßnahme sein, die Sicherheit zu verbessern. Wenn Sie diese Firewallkonfigurationen verwenden, müssen Sie bedenken, dass URLs hinzugefügt und im Verlauf der Zeit geändert werden. Daher ist es ratsam, die Tabellen in diesem Dokument als Bestandteil eines Change Management-Prozesses von einer Azure File Sync-Agent-Version zu einer anderen in einer Testbereitstellung des neuesten Agents zu überprüfen. Auf diese Weise können Sie sicherstellen, dass Ihre Firewall so konfiguriert ist, dass sie Datenverkehr zu Domänen zulässt, die für den neuesten Agent erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md)
