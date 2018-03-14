---
title: Importieren einer Power BI Desktop-Datei in Azure Analysis Services | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie über das Azure-Portal eine Power BI Desktop-Datei (pbix) importieren."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: e0be0c69b501d7e93c65bcf23d4dd1b6bfa89caf
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importieren einer Power BI Desktop-Datei

Sie können in Azure AS ein neues Model erstellen, indem Sie eine Power BI Desktop-Datei (pbix) importieren. Modellmetadaten, zwischengespeicherte Daten und Datenquellenverbindungen werden importiert. Berichte und Visualisierungen werden nicht importiert.

**Einschränkungen**   
- Das pbix-Modell eignet sich nur zum Herstellen einer Verbindung mit [in Analysis Services unterstützten Datenquellen](analysis-services-datasource.md). 
- Das pbix-Modell eignet sich nicht für Live- oder DirectQuery-Verbindungen. 
- Wenn für das pbix-Modell eine Verbindung mit lokalen Datenquellen hergestellt wird, muss für Ihren Analysis Services-Server ein [lokales Gateway](analysis-services-gateway.md) konfiguriert werden.
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
