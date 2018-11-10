---
title: Importieren einer Power BI Desktop-Datei in Azure Analysis Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie über das Azure-Portal eine Power BI Desktop-Datei (pbix) importieren.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3adf0c9c2e2b264904e66b82716447d634aaeee7
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209648"
---
# <a name="import-a-power-bi-desktop-file"></a>Importieren einer Power BI Desktop-Datei

Sie können ein Datenmodell in einer Power BI Desktop-Datei (PBIX-Datei) in Azure Analysis Services importieren. Modellmetadaten, zwischengespeicherte Daten und Datenquellenverbindungen werden importiert. Berichte und Visualisierungen werden nicht importiert. Importierte Datenmodelle aus Power BI Desktop haben den Kompatibilitätsgrad 1400.

> [!IMPORTANT]
> Dieses Feature ist veraltet. Es kann in einem zukünftigen Update entfernt oder erheblich geändert werden. Es wird empfohlen, dieses Feature in neuen und vorhandenen Projekten nicht mehr zu verwenden, um die Kompatibilität mit kommenden Updates aufrechtzuerhalten. Bei komplexeren Modellentwicklungen und -tests ist es sinnvoll, Visual Studio (SSDT) und SQL Server Management Studio (SSMS) zu verwenden.

**Einschränkungen**   


- Wenn Ihr Datenmodell mit Power BI Desktop-Update vom Juli 2018 (2.60.5169.3201) oder danach erstellt wurde, stellen Sie sicher, dass keine Previewfunktionen aktiviert sind. Previewfunktionen werden noch nicht in Azure Analysis Services unterstützt. Wenn der folgende Fehler beim Importieren ausgelöst wird, sind für die PBIX-Datei Previewfunktionen aktiviert, die noch nicht in Azure Analysis Services unterstützt werden.

    ![Kompatibilitätsgradwarnung](./media/analysis-services-import-pbix/aas-import-pbix-cl-warning.png)   
- Sie benötigen Serveradministratorberechtigungen zum Importieren aus einer PBIX-Datei.
- Das PBIX-Modell kann nur Verbindungen mit Datenquellen vom Typ **Azure SQL-Datenbank** und **Azure SQL Data Warehouse** herstellen.
- Das pbix-Modell eignet sich nicht für Live- oder DirectQuery-Verbindungen. 


## <a name="to-import-from-pbix"></a>So führen Sie den Import aus PBIX durch

1. Klicken Sie auf dem Server unter **Übersicht** > **Web-Designer** auf **Öffnen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Klicken Sie in **Web-Designer** > **Modelle** auf **+ Hinzufügen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Geben Sie unter **Neues Modell** einen Modellnamen ein, und wählen Sie die Power BI Desktop-Datei aus.

    ![Dialogfeld „Neues Modell“ im Azure-Portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. Suchen Sie unter **Importieren** nach Ihrer Datei, und wählen Sie sie aus.

     ![Verbinden des Dialogfelds im Azure-Portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>Ändern von Anmeldeinformationen

Wenn Sie ein Datenmodell aus einer PBIX-Datei importieren, werden die Anmeldeinformationen für die Verbindung mit einer Datenquelle standardmäßig auf ServiceAccount festgelegt. Nachdem ein Modell aus einer PBIX-Datei importiert wurde, können Sie die Anmeldeinformationen mit den folgenden Methoden ändern:

- Verwenden Sie die Version von SSMS aus Juli 2018 (Version 17.8.1) oder eine höhere Version, um Anmeldeinformationen zu bearbeiten. Dies ist die einfachste Möglichkeit.
- Verwenden Sie den TMSL [Alter-Befehl](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl) für das [DataSources-Objekt](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl), um die Verbindungszeichenfolgen-Eigenschaft zu ändern. 
- Öffnen Sie das Modell in Visual Studio, bearbeiten Sie die Anmeldeinformationen für die Datenquellenverbindung, und stellen Sie das Modell dann erneut bereit.

So ändern Sie die Anmeldeinformationen mithilfe von SSMS: 

1. Erweitern Sie in SSMS „Datenbank“ > **Verbindungen**. 
2. Klicken Sie mit der rechten Maustaste auf die Datenbankverbindung, und klicken Sie dann auf **Anmeldeinformationen aktualisieren**. 

    ![Aktualisieren der Anmeldeinformationen](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. Wählen Sie im Dialogfeld „Anmeldeinformationen“ einen Anmeldeinformationentyp aus, und geben Sie Anmeldeinformationen ein. Wählen Sie für die SQL-Authentifizierung „Datenbank“ aus. Wählen Sie für ein Unternehmenskonto (OAuth) „Microsoft-Konto“ aus.
    ![Bearbeiten von Anmeldeinformationen](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

Die Version von Power BI Desktop aus Juli 2018 enthält ein neues Feature für das Ändern von Datenquellenberechtigungen. Klicken Sie auf der Registerkarte **Start** auf **Abfragen bearbeiten**  > **Datenquelleneinstellungen**. Wählen Sie die Datenquellenverbindung aus, und klicken Sie dann auf **Berechtigungen bearbeiten**.


## <a name="see-also"></a>Weitere Informationen

[Erstellen eines Modells im Azure-Portal](analysis-services-create-model-portal.md)   
[Herstellen einer Verbindung mit Azure Analysis Services](analysis-services-connect.md)  
