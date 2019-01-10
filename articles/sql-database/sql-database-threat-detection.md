---
title: Bedrohungserkennung – Azure SQL-Datenbank | Microsoft Docs
description: Die Bedrohungserkennung erkennt anormale Datenbankaktivitäten, die in einer einzelnen Datenbank oder in einem Pool für elastische Datenbanken auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 3f4a120e2aaf2925805bec26f721d5cfb4194bf1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041542"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>Warnungen der Bedrohungserkennung von Azure SQL-Datenbank für eine Einzeldatenbank

Die [Bedrohungserkennung](sql-database-threat-detection-overview.md) von Azure SQL für einzelne Datenbanken einer [SQL-Datenbank-Instanz](sql-database-technical-overview.md) identifiziert anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese missbräuchlich zu verwenden. Mithilfe der Bedrohungserkennung können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Warnungen der Bedrohungserkennung](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Sie können sich über [E-Mail-Benachrichtigungen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) oder im [Azure-Portal](sql-database-threat-detection.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

Die [Bedrohungserkennung](sql-database-threat-detection-overview.md) ist Bestandteil des Angebots [SQL Advanced Threat Protection](sql-advanced-threat-protection.md) (ATP). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf die Bedrohungserkennung und ihre Verwaltung sind über das zentrale SQL ATP-Portal möglich. Für den Bedrohungserkennungsdienst werden pro logischem Server 15 USD/Monat berechnet, hierbei sind die ersten 30 Tage kostenlos.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank im Azure-Portal

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite des Azure SQL-Datenbank-Servers, den Sie schützen möchten. Wählen Sie in den Sicherheitseinstellungen **Advanced Threat Protection** aus.
3. Gehen Sie auf der Konfigurationsseite **Advanced Threat Protection** folgendermaßen vor:

   - Aktivieren Sie Advanced Threat Protection auf dem Server.
   - Geben Sie unter **Einstellungen für Bedrohungserkennung** im Textfeld **Send alerts to** (Warnungen senden an) eine Liste von E-Mail-Adressen an, die Sicherheitswarnungen bei der Erkennung von anomalen Datenbankaktivitäten empfangen sollen.
  
   ![Einrichten der Bedrohungserkennung](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Einrichten der Bedrohungserkennung über PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Bedrohungserkennung](sql-database-threat-detection-overview.md).
- Erfahren Sie mehr über die [Bedrohungserkennung bei der verwalteten Instanz](sql-database-managed-instance-threat-detection.md).  
- Erfahren Sie mehr über [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
