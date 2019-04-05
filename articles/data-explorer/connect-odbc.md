---
title: Herstellen einer Verbindung mit Azure Data Explorer mithilfe der ODBC
description: In dieser Schrittanleitung erfahren Sie, wie Sie eine ODBC-Verbindung mit Azure Data Explorer einrichten, und wie Sie diese Verbindung anschließend zur Visualisierung von Daten mit Tableau verwenden.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: 0ec1c2f4fc412ca6c81e179d0ad22f781b896357
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757622"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Herstellen einer Verbindung mit Azure Data Explorer mithilfe der ODBC

Die Open Database Connectivity ([ODBC](/sql/odbc/reference/odbc-overview)) ist eine weit verbreitete Anwendungsprogrammierschnittstelle (API) für Datenbankzugriff. Verwenden Sie die ODBC zum Herstellen einer Verbindung mit Azure Data Explorer aus Anwendungen ohne dedizierten Connector.

Im Hintergrund rufen Anwendungen Funktionen in der ODBC-Schnittstelle ab, die in datenbankspezifischen Modulen (*Treibern*) implementiert sind. Azure Data Explorer unterstützt eine Teilmenge des SQL Server-Kommunikationsprotokolls ([MS-TDS](/azure/kusto/api/tds/)) und kann daher den ODBC-Treiber für SQL Server verwenden.

In diesem Artikel erfahren Sie, wie Sie den SQL Server-ODBC-Treiber verwenden, damit Sie aus jeder Anwendung, die die ODBC unterstützt, eine Verbindung mit Azure Data Explorer herstellen können. Sie können optional eine Verbindung mit Azure Data Explorer aus Tableau herstellen und Daten aus einem Beispielcluster importieren.

## <a name="prerequisites"></a>Voraussetzungen

Folgendes ist zum Abschließen dieser Vorgehensweise erforderlich:

* [Microsoft ODBC Driver for SQL Server, Version 17.2.0.1 oder höher](/sql/connect/odbc/download-odbc-driver-for-sql-server) für Ihr Betriebssystem.

* Wenn Sie unserem Tableau-Beispiel folgen möchten, benötigen Sie außerdem Folgendes:

  * Tableau Desktop, Vollversion oder [Testversion](https://www.tableau.com/products/desktop/download).

  * Ein Cluster, der die StormEvents-Beispieldaten enthält Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Azure Data Explorer-Clusters und einer Datenbank](create-cluster-database-portal.md) und [Erfassen von Beispieldaten in Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-odbc-data-source"></a>Konfigurieren der ODBC-Datenquelle

Gehen Sie wie folgt vor, um eine ODBC-Datenquelle mithilfe des ODBC-Treibers für SQL Server zu konfigurieren.

1. Suchen Sie in Windows nach *ODBC Data Sources*, und öffnen Sie die ODBC Data Sources-Desktop-App.

1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen einer Datenquelle](media/connect-odbc/add-data-source.png)

1. Wählen Sie **ODBC Driver 17 for SQL Server** und dann **Fertig stellen** aus.

    ![Treiber auswählen](media/connect-odbc/select-driver.png)

1. Geben Sie einen Namen ein sowie eine Beschreibung für die Verbindung und den Cluster, mit dem Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Weiter**. Die Form der Cluster-URL sollte wie folgt aussehen: *\<ClusterName\>.\<Region\>.kusto.windows.net*.

    ![Server auswählen](media/connect-odbc/select-server.png)

1. Wählen Sie **Active Directory Integrated** aus, und klicken Sie dann auf **Weiter**.

    ![Active Directory Integrated](media/connect-odbc/active-directory-integrated.png)

1. Wählen Sie die Datenbank mit den Beispieldaten aus, und klicken Sie auf **Weiter**.

    ![Standarddatenbank ändern](media/connect-odbc/change-default-database.png)

1. Lassen Sie auf dem nächsten Bildschirm alle Optionen als Standardeinstellungen, und wählen Sie dann **Fertig stellen** aus.

1. Wählen Sie **Datenquelle testen** aus.

    ![Datenquelle testen](media/connect-odbc/test-data-source.png)

1. Stellen Sie sicher, dass der Test erfolgreich war, und klicken Sie dann auf **OK**. Wenn der Test nicht erfolgreich war, überprüfen Sie die Werte, die Sie in vorherigen Schritten angegeben haben, und stellen Sie sicher, dass Sie über ausreichende Berechtigungen für das Herstellen einer Verbindung mit dem Cluster verfügen.

    ![Test erfolgreich](media/connect-odbc/test-succeeded.png)

## <a name="visualize-data-in-tableau-optional"></a>Visualisieren von Daten in Tableau (optional)

Nachdem Sie die Konfiguration der ODBC abgeschlossen haben, können Sie Beispieldaten in Tableau importieren.

1. Wählen Sie im linken Menü in Tableau Desk **Andere Datenbanken (ODBC)** aus.

    ![Herstellen einer Verbindung mit ODBC](media/connect-odbc/connect-odbc.png)

1. Wählen Sie für **DSN** die Datenquelle aus, die Sie für ODBC erstellt haben, und wählen Sie dann **Anmelden** aus.

    ![Anmeldung bei ODBC](media/connect-odbc/odbc-sign-in.png)

1. Wählen Sie für **Datenbank** die Datenbank auf Ihrem Beispielcluster aus, beispielsweise *TestDatabase*. Für **Schema** wählen Sie *dbo* aus, und für **Tabelle** wählen Sie die Beispieltabelle *StormEvents* aus.

    ![Datenbank und Tabelle auswählen](media/connect-odbc/select-database-table.png)

1. Tableau zeigt nun das Schema für die Beispieldaten an. Wählen Sie **Jetzt aktualisieren** aus, um die Daten in Tableau zu importieren.

    ![Aktualisieren von Daten](media/connect-odbc/update-data.png)

    Wenn die Daten importiert sind, zeigt Tableau Zeilen von Daten an, ähnlich der folgenden Abbildung.

    ![Resultset](media/connect-odbc/result-set.png)

1. Sie können nun basierend auf den Daten, die Sie aus Azure Data Explorer eingebunden haben, in Tableau Visualisierungen erstellen. Weitere Informationen finden Sie unter [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Nächste Schritte

[Schreiben von Abfragen für den Azure-Daten-Explorer](write-queries.md)

[Tutorial: Visualisieren von Daten über Azure Data Explorer in Power BI](visualize-power-bi.md)