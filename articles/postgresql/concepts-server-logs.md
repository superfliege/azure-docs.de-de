---
title: "Serverprotokolle in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Generieren von Abfragen und Fehlerprotokollen in Azure-Datenbank für PostgreSQL"
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 2f75af62df0284a609169acd9d2c5bb6eaf36fbe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Serverprotokolle in Azure-Datenbank für PostgreSQL 
Azure-Datenbank für PostgreSQL generiert Abfragen und Fehlerprotokolle. Der Zugriff auf Transaktionsprotokolle wird jedoch nicht unterstützt. Diese Protokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung. Weitere Informationen finden Sie unter [Fehlerberichterstattung und -protokollierung](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Zugreifen auf Serverprotokolle
Über das Azure-Portal, die [Azure CLI](howto-configure-server-logs-using-cli.md) und Azure-REST-APIs können Sie Azure PostgreSQL-Serverfehlerprotokolle auflisten und herunterladen.

## <a name="log-retention"></a>Protokollaufbewahrung
Mithilfe des mit Ihrem Server verknüpften Parameters **log\_retention\_period** können Sie die Beibehaltungsdauer für Systemprotokolle festlegen. Die Einheit für diesen Parameter wird in Tagen gemessen. Der Standardwert beträgt drei Tage. Der Maximalwert beträgt 7 Tage. Ihrem Server muss genügend Speicher zugewiesen sein, damit die beibehaltenen Protokolldateien gespeichert werden können.
Die Protokolldateien rotieren jede Stunde bzw. bei einer Größe von 100 MB, je nachdem, welcher Fall zuerst eintritt.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurieren der Protokollierung für den Azure PostgreSQL-Server
Sie können die Abfrage- und Fehlerprotokollierung für Ihren Server aktivieren. Fehlerprotokolle können Informationen über „auto-vacuum“, Verbindungen und Prüfpunkte enthalten.

Sie können die Abfrageprotokollierung für Ihre PostgreSQL-Datenbankinstanz durch Festlegung der folgenden zwei Serverparameter aktivieren: „log\_statement“ und „log\_min\_duration\_statement“.

Der Parameter **log\_statement** steuert, welche SQL-Anweisungen protokolliert werden. Es wird empfohlen, diesen Parameter auf ***Alle*** zu setzen, damit alle Anweisungen protokolliert werden. Der Standardwert lautet „Keine“.

Der Parameter **log\_min\_duration\_statement** legt das Limit einer zu protokollierenden Anweisung in Millisekunden fest. Alle SQL-Anweisungen, die länger ausgeführt werden als die Parametereinstellung, werden protokolliert. Dieser Parameter ist deaktiviert und standardmäßig auf minus 1 (–1) festgelegt. Die Aktivierung dieses Parameters kann bei der Identifizierung nicht optimierter Abfragen in Ihren Anwendungen hilfreich sein.

Mit dem Parameter **log\_min\_messages** können Sie steuern, welche Meldungsebenen in das Serverprotokoll geschrieben werden. Die Standardeinstellung lautet „WARNUNG“ 

Weitere Informationen zu diesen Einstellungen finden Sie in der Dokumentation unter [Fehlerberichterstattung und -protokollierung](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html). Informationen speziell zur Konfiguration von Parametern für Azure-Datenbank für PostgreSQL-Server finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Zugriff auf Protokolle über die Befehlszeilenschnittstelle Azure CLI finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-using-cli.md).
- Weitere Informationen zu Serverparametern finden Sie unter [Anpassen der Serverkonfigurationsparameter über die Azure CLI](howto-configure-server-parameters-using-cli.md).
