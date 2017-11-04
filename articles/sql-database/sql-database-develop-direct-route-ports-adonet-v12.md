---
title: "Andere Ports als 1433 für SQL-Datenbank | Microsoft Docs"
description: "Clientverbindungen von ADO.NET mit Azure SQL-Datenbank können den Proxy umgehen und über andere Ports als Port 1433 direkt mit der Datenbank interagieren."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: ea184cce4217e6c81c02740f0d6ccf79cc1c1c4a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Andere Ports als 1433 für ADO.NET 4.5
Dieses Thema beschreibt das Verbindungsverhalten von Azure SQL-Datenbank für Clients mit ADO.NET 4.5 oder einer höheren Version. 

> [!IMPORTANT]
> Informationen zur Verbindungsarchitektur finden Sie unter [Verbindungsarchitektur der Azure SQL-Datenbank](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Außerhalb im Vergleich zu innerhalb
Bei Verbindungen mit Azure SQL-Datenbank lautet die erste Frage, ob das Clientprogramm *außerhalb* oder *innerhalb* der Azure-Cloudgrenzen ausgeführt wird. In den Unterabschnitten werden zwei häufige Szenarien erläutert.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Außerhalb:* Client wird auf dem Desktopcomputer ausgeführt.
Port 1433 ist der einzige Port, der auf dem Desktopcomputer geöffnet sein muss, auf dem die Clientanwendung der SQL-Datenbank gehostet ist.

#### <a name="inside-client-runs-on-azure"></a>*Innerhalb:* Client wird in Azure ausgeführt.
Wenn der Client innerhalb der Azure-Cloudgrenzen ausgeführt wird, verwendet er die sogenannte *direkte Route* für die Interaktion mit dem Azure SQL-Datenbankserver. Nachdem eine Verbindung hergestellt wurde, ist bei weiteren Interaktionen zwischen dem Client und der Datenbank kein Middleware-Proxy eingebunden.

Der Ablauf ist wie folgt:

1. ADO.NET 4.5 (oder höher) initiiert eine kurze Interaktion mit der Azure-Cloud und empfängt eine dynamisch ermittelte Portnummer.
   
   * Die dynamisch ermittelte Portnummer liegt im Bereich von 11000 bis 11999 oder 14000 bis 14999.
2. ADO.NET stellt dann eine direkte Verbindung mit dem SQL-Datenbankserver ohne Einbindung von Middleware her.
3. Abfragen werden direkt an die Datenbank gesendet, und Ergebnisse werden direkt an den Client zurückgegeben.

Stellen Sie sicher, dass der Portbereich von 11000 bis 11999 und von 14000 bis 14999 auf Ihrem Azure-Clientcomputer für ADO.NET 4.5-Clientinteraktionen mit SQL-Datenbank verfügbar ist.

* Insbesondere dürfen Ports in diesem Bereich keine anderen ausgehenden Blockierungen aufweisen.
* Auf Ihrer Azure-VM steuert die **Windows-Firewall mit erweiterter Sicherheit** die Porteinstellungen.
  
  * Sie können die [Benutzeroberfläche der Firewall](http://msdn.microsoft.com/library/cc646023.aspx) verwenden, um eine Regel hinzuzufügen, für die Sie das **TCP**-Protokoll sowie einen Portbereich mit einer Syntax wie **11000–11999** angeben.

## <a name="version-clarifications"></a>Erläuterungen zu Versionen
In diesem Abschnitt werden die Moniker erläutert, die auf Produktversionen verweisen. Außerdem sind einige Kombinationen von Versionen zwischen Produkten aufgeführt.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 unterstützt das Protokoll TDS 7.3, aber nicht 7.4.
* ADO.NET 4.5 und höher unterstützt das Protokoll TDS 7.4.

## <a name="related-links"></a>Verwandte Links
* ADO.NET 4.6 wurde am 20. Juli 2015 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 wurde am 15. August 2012 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Ein Blogbeitrag zu ADO.NET 4.5.1 steht [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx)bereit.
* [Liste der TDS-Protokollversionen](http://www.freetds.org/userguide/tdshistory.htm)
* [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* [Firewall für die Azure SQL-Datenbank](sql-database-firewall-configure.md)
* [Konfigurieren der Firewalleinstellungen für Azure SQL-Datenbank](sql-database-configure-firewall-settings.md)

