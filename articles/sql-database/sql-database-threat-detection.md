---
title: Bedrohungserkennung – Azure SQL-Datenbank | Microsoft-Dokumentation
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
ms.date: 01/30/2019
ms.openlocfilehash: 032146742c1a49fc8cdbda24d5c732add3cc5ea7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465349"
---
# <a name="azure-sql-database-threat-detection-for-standalone-or-pooled-database"></a>Warnungen der Bedrohungserkennung von Azure SQL-Datenbank für eine eigenständige Datenbank oder in einem Pool zusammengefasste Datenbanken

Die [Bedrohungserkennung](sql-database-threat-detection-overview.md) von Azure SQL für eigenständige und in einem Pool zusammengefasste Datenbanken identifiziert anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese missbräuchlich zu verwenden. Mithilfe der Bedrohungserkennung können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Warnungen der Bedrohungserkennung](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Sie können sich über [E-Mail-Benachrichtigungen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) oder im [Azure-Portal](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

Die [Bedrohungserkennung](sql-database-threat-detection-overview.md) ist Bestandteil des [SQL Advanced Data Security](sql-advanced-threat-protection.md)-Angebots (ADS). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf die Bedrohungserkennung und ihre Verwaltung sind über das zentrale SQL ADS-Portal möglich. Für das Advanced Data Security-Paket werden pro logischem Server 15 USD/Monat berechnet, hierbei sind die ersten 30 Tage kostenlos.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank im Azure-Portal

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite des Azure SQL-Datenbank-Servers, den Sie schützen möchten. Wählen Sie in den Sicherheitseinstellungen **Advanced Data Security** aus.
3. Wählen Sie auf der Seite für die **Advanced Data Security**-Konfiguration

   - die Option „Advanced Data Security auf dem Server aktivieren“ aus.
   - Geben Sie unter **Einstellungen für Bedrohungserkennung** im Textfeld **Warnungen senden an** eine Liste von E-Mail-Adressen an, die Sicherheitswarnungen bei der Erkennung von anomalen Datenbankaktivitäten empfangen sollen.
  
   ![Einrichten der Bedrohungserkennung](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Einrichten der Bedrohungserkennung über PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Bedrohungserkennung](sql-database-threat-detection-overview.md).
- Erfahren Sie mehr über die [Bedrohungserkennung bei der verwalteten Instanz](sql-database-managed-instance-threat-detection.md).  
- Erfahren Sie mehr über [SQL Advanced Data Security](sql-advanced-threat-protection.md).
- Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md)
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
