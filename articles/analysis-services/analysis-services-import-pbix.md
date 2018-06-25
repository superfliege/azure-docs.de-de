---
title: Importieren einer Power BI Desktop-Datei in Azure Analysis Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie über das Azure-Portal eine Power BI Desktop-Datei (pbix) importieren.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aea6f3efcf3740527c43b75a30caadf6b2a8b623
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601070"
---
# <a name="import-a-power-bi-desktop-file"></a>Importieren einer Power BI Desktop-Datei

Sie können ein Datenmodell in einer Power BI Desktop-Datei (PBIX-Datei) in Azure Analysis Services importieren. Modellmetadaten, zwischengespeicherte Daten und Datenquellenverbindungen werden importiert. Berichte und Visualisierungen werden nicht importiert. Importierte Datenmodelle aus Power BI Desktop haben den Kompatibilitätsgrad 1400.

**Einschränkungen**   
- Das PBIX-Modell kann nur Verbindungen mit Datenquellen vom Typ **Azure SQL-Datenbank** und **Azure SQL Data Warehouse** herstellen. 
- Das pbix-Modell eignet sich nicht für Live- oder DirectQuery-Verbindungen. 
- Beim Import kann ein Fehler auftreten, wenn Ihr pbix-Datenmodell Metadaten enthält, die in Analysis Services nicht unterstützt werden.

## <a name="to-import-from-pbix"></a>So führen Sie den Import aus PBIX durch

1. Klicken Sie auf dem Server unter **Übersicht** > **Web-Designer** auf **Öffnen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Klicken Sie in **Web-Designer** > **Modelle** auf **+ Hinzufügen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Geben Sie unter **Neues Modell** einen Modellnamen ein, und wählen Sie die Power BI Desktop-Datei aus.

    ![Dialogfeld „Neues Modell“ im Azure-Portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. Suchen Sie unter **Importieren** nach Ihrer Datei, und wählen Sie sie aus.

     ![Verbinden des Dialogfelds im Azure-Portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Weitere Informationen

[Erstellen eines Modells im Azure-Portal](analysis-services-create-model-portal.md)   
[Herstellen einer Verbindung mit Azure Analysis Services](analysis-services-connect.md)  
