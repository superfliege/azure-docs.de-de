---
title: Schnellstart für Fivetran mit Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Schneller Einstieg in Fivetran und Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 8e738becfe356908af5baffc0ebf225916b2616e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355207"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Schneller Einstieg in Fivetran und SQL Data Warehouse

Bei diesem Schnellstart wird vorausgesetzt, dass Sie bereits über eine vorhandene Instanz von SQL Data Warehouse verfügen.

## <a name="setup-connection"></a>Einrichten der Verbindung

1. Suchen Sie den vollqualifizierten Servernamen und Datenbanknamen für die Verbindung mit Azure SQL Data Warehouse.

   [Wie kann ich den Servernamen und Datenbanknamen im Portal suchen?](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connect-overview)

2. Legen Sie im Setup-Assistenten fest, ob Sie eine direkte Verbindung mit der Datenbank oder eine Verbindung über einen SSH-Tunnel herstellen möchten.

   Wenn Sie sich für eine direkte Verbindung mit der Datenbank entscheiden, müssen Sie eine Firewallregel für den Zugriff erstellen. Dies ist die einfachste und sicherste Methode.

   Wenn Sie sich für eine Verbindung über einen SSH-Tunnel entscheiden, stellt Fivetran eine Verbindung mit einem separaten Server in Ihrem Netzwerk her, der einen SSH-Tunnel zu Ihrer Datenbank bereitstellt. Diese Methode ist erforderlich, wenn sich Ihre Datenbank in einem nicht zugänglichen Subnetz in einem virtuellen Netzwerk befindet.

3. Fügen Sie der Firewall auf Serverebene die IP-Adresse „52.0.2.4“ hinzu, um eingehende Verbindungen mit Azure SQL Data Warehouse von Fivetran zuzulassen.

   [Wie wird eine Firewall auf Serverebene hinzugefügt?](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal#create-a-server-level-firewall-rule)

## <a name="setup-user-credentials"></a>Einrichten von Benutzeranmeldeinformationen

Stellen Sie mit SQL Server Management Studio oder einem Tool Ihrer Wahl eine Verbindung mit Azure SQL Data Warehouse als Serveradministrator her, und führen Sie die folgenden SQL-Befehle aus, um einen Benutzer für Fivetran zu erstellen:

In der Masterdatenbank: ` CREATE LOGIN fivetran WITH PASSWORD = '<password>'; `

In der SQL Data Warehouse-Datenbank:

```
CREATE USER fivetran_user_without_login without login;
CREATE USER fivetran FOR LOGIN fivetran;
GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
```

Nachdem der Benutzer „fivetran“ erstellt wurde, gewähren sie ihm die folgenden Berechtigungen für das Warehouse:

```
GRANT CONTROL to fivetran;
```

Fügen Sie dem erstellten Benutzer eine geeignete Ressourcenklasse hinzu, und zwar je nach Arbeitsspeicherbedarf für das Erstellen eines Columnstore-Index. Beispielsweise benötigen Integrationen wie Marketo und Salesforce aufgrund der hohen Anzahl von Spalten/größeren Datenmenge, die mehr Arbeitsspeicher zum Erstellen von Columnstore-Indizes erfordern, eine höhere Ressourcenklasse.

Es wird empfohlen, statische Ressourcenklassen zu verwenden. Sie können mit der Ressourcenklasse `staticrc20` beginnen, mit der 200 MB für den Benutzer unabhängig von der verwendeten Leistungsstufe zugeordnet werden. Wenn die Columnstore-Indizierung mit der aktuellen Ressourcenklasse fehlschlägt, muss die Ressourcenklasse erhöht werden.

```
EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
```

Weitere Informationen finden Sie in den Dokumenten zu [Speicher- und Parallelitätsgrenzwerten](https://docs.microsoft.com/azure/sql-data-warehouse/memory-and-concurrency-limits#data-warehouse-limits) und [Ressourcenklassen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression#ways-to-allocate-more-memory).

Die Berechtigung CONTROL ist erforderlich, um datenbankweit gültige Anmeldeinformationen zu erstellen, die beim Laden von Dateien aus Blob Storage mit PolyBase verwendet werden.

Geben Sie die Anmeldeinformationen für den Zugriff auf Azure SQL Data Warehouse ein:

1. Host (Ihr Servername)
2. Port
3. Datenbank
4. Benutzer (Benutzername muss `fivetran@<server_name>` sein, wobei `<server_name>` Teil Ihrer Azure-Host-URI ist: `<server_name>.database.windows.net`)
5. Kennwort