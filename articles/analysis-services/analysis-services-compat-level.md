---
title: Kompatibilitätsgrad von Datenmodellen in Azure Analysis Services | Microsoft-Dokumentation
description: Grundlegendes zum Kompatibilitätsgrad von tabellarischen Datenmodellen
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 17a75e86d5539427c8837b3077aacf85b4681ad6
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447522"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Kompatibilitätsgrad für tabellarische Analysis Services-Modelle

Der *Kompatibilitätsgrad* bezieht sich auf releasespezifische Verhalten in der Analysis Services-Engine. Änderungen am Kompatibilitätsgrad werden üblicherweise im Zuge der Hauptreleases von SQL Server vorgenommen. Diese Änderungen werden auch in Azure Analysis Services implementiert, um die Parität zwischen beiden Plattformen aufrechtzuerhalten. Änderungen am Kompatibilitätsgrad wirken sich auch auf die in Ihren tabellarischen Modellen verfügbaren Funktionen aus. Beispielsweise weisen DirectQuery und tabellarische Objektmetadaten je nach Kompatibilitätsgrad verschiedene Implementierungen auf. Der Kompatibilitätsgrad wird im tabellarischen Modellprojekt in Visual Studio (SSDT) angegeben. Tabellarische Modelle, die in Power BI Desktop erstellt und daraus importiert werden, haben lediglich den Kompatibilitätsgrad 1400.

Azure Analysis Services unterstützt tabellarische Modelle mit den Kompatibilitätsgraden 1200 und 1400. 

Der aktuelle Kompatibilitätsgrad ist 1400. Der jeweilige Grad stimmt mit SQL Server 2017 Analysis Services überein. Zu den wesentlichen Features im Kompatibilitätsgrad 1400 zählen Folgende:

*  Neue Features für Datenkonnektivität und Import mit Unterstützung von TOM-APIs und TMSL-Skripts. 
*  Datentransformations- und Datenmashupfunktionen mithilfe von Get Data- und M-Ausdrücken
*  Measures bieten Unterstützung für eine Detailzeileneigenschaft mit einem DAX-Ausdruck. Diese Eigenschaft aktiviert Clienttools wie Microsoft Excel, um ein Drilldown auf detaillierte Daten aus einem aggregierten Bericht auszuführen. Wenn Benutzer beispielsweise den monatlichen Gesamtumsatz für eine Region anzeigen, können sie die zugehörigen Auftragsdetails einsehen. 
*  Sicherheit auf Objektebene für Tabellen- und Spaltennamen sowie die darin enthaltenen Daten
*  Verbesserte Unterstützung für unregelmäßige Hierarchien
*  Verbesserungen an der Leistung und Überwachung
  
## <a name="set-compatibility-level"></a>Festlegen des Kompatibilitätsgrads 
 Beim Erstellen eines neuen Projekts für tabellarische Modelle in SSDT können Sie im Dialogfeld **Designer für tabellarische Modelle** den Kompatibilitätsgrad angeben. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Wenn Sie die Option **Diese Meldung nicht mehr anzeigen** auswählen, wird bei allen nachfolgenden Projekten der als Standard angegebene Kompatibilitätsgrad verwendet. Sie können den Standardkompatibilitätsgrad in SSDT unter **Extras** > **Optionen** ändern.  
  
 Um für ein vorhandenes Projekt für tabellarische Modelle ein Upgrade in SSDT durchzuführen, legen Sie im Fenster **Eigenschaften** des Modells die Eigenschaft **Kompatibilitätsgrad** fest. Denken Sie daran, dass ein Upgrade des Kompatibilitätsgrads nicht rückgängig gemacht werden kann.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Überprüfen des Kompatibilitätsgrads einer tabellarischen Modelldatenbank in SQL Server Management Studio 
 Klicken Sie in SSMS mit der rechten Maustaste auf den Datenbanknamen und dann auf **Eigenschaften** > **Kompatibilitätsgrad**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Überprüfen des unterstützten Kompatibilitätsgrads eines Servers in SSMS  
 Klicken Sie in SSMS mit der rechten Maustaste auf den Servernamen und dann auf **Eigenschaften** > **Unterstützter Kompatibilitätsgrad**.  
  
 Diese Eigenschaft gibt den höchsten Kompatibilitätsgrad einer Datenbank an, die auf dem Server (ausgenommen der Vorschau) ausgeführt wird. Der unterstützte Kompatibilitätsgrad kann nicht geändert werden.  

## <a name="next-steps"></a>Nächste Schritte
  [Erstellen eines Modells im Azure-Portal](analysis-services-create-model-portal.md)   
  [Verwalten von Analysis Services](analysis-services-manage.md)  
