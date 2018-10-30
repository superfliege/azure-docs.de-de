---
title: Erstellen eines Tabellenmodells mit dem Azure Analysis Services-Web-Designer | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Azure Analysis Services-Tabellenmodell mit dem Web-Designer im Azure-Portal erstellen.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 837601a55bbe97e904f3dece9f4c87563d6dd4e9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49092676"
---
# <a name="create-a-model-in-azure-portal"></a>Erstellen eines Modells im Azure-Portal

Das Feature Azure Analysis Services-Web-Designer (Vorschau) im Azure-Portal bietet eine schnelle und einfache Möglichkeit zum Erstellen und Bearbeiten von Tabellenmodellen und Abfragen von Modelldaten direkt im Browser. 

> [!IMPORTANT]
> Dieses Feature ist veraltet. Es kann in einem zukünftigen Update entfernt oder erheblich geändert werden. Wir empfehlen, dieses Feature in neuen und vorhandenen Projekten nicht mehr zu verwenden, um die Kompatibilität mit kommenden Updates aufrecht zu erhalten. Bei komplexeren Modellentwicklungen und -tests ist es sinnvoll, Visual Studio (SSDT) und SQL Server Management Studio (SSMS) zu verwenden.


## <a name="before-you-begin"></a>Voraussetzungen

- Ihr Azure Analysis Services-Server muss auf der Ebene „Standard“ oder „Developer“ ausgeführt werden. Neue, mit dem Web-Designer erstellte Modelle, sind DirectQuery, die nur von diesen Ebenen unterstützt werden.
- Eine Azure SQL-Datenbank, Azure SQL Data Warehouse oder eine Power BI Desktop-Datei (.pbix) als Datenquelle. Neue Modelle, die anhand von Power BI Desktop-Dateien erstellt wurden, unterstützen Azure SQL-Datenbank und Azure SQL Data Warehouse.
- Ein SQL Server-Konto und -Kennwort für das Herstellen einer Verbindung mit Azure SQL-Datenbank oder Azure SQL Data Warehouse-Datenquellen.
- Sie müssen über Serveradministratorberechtigungen verfügen, um ein neues Modell erstellen zu können. Datenbankadministratorrechte sind erforderlich, um ein Modell mithilfe des Designers bearbeiten und abfragen zu können.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="to-create-a-new-tabular-model"></a>Erstellen eines neuen tabellarischen Modellprojekts

1. Klicken Sie auf dem Server unter **Übersicht** > **Web-Designer** auf **Öffnen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Klicken Sie in **Web-Designer** > **Modelle** auf **+ Hinzufügen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Geben Sie einen Modellnamen in **Neues Modell** ein, und wählen Sie dann eine Datenquelle aus.

    ![Dialogfeld „Neues Modell“ im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. Geben Sie die Verbindungseigenschaften in **Verbinden** ein. Benutzername und Kennwort müssen ein SQL Server-Dienstkonto sein.

     ![Verbinden des Dialogfelds im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. Wählen Sie in **Tabellen und Ansichten** die Tabellen aus, die Sie in Ihr Modell einschließen möchten, und klicken Sie dann auf **Erstellen**. Es werden automatisch Beziehungen zwischen Tabellen mit einem Schlüsselpaar erstellt.

     ![Auswählen von Tabellen und Ansichten](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Ihr neues Modell wird in Ihrem Browser angezeigt. Mögliche nächste Schritte:   

- Fragen Sie Modelldaten ab, indem Sie Felder in den Abfrage-Designer ziehen und Filter hinzufügen.
- Erstellen Sie neue Kennzahlen in Tabellen.
- Bearbeiten Sie die enthaltenen Modellmetadaten mithilfe des JSON-Editors.
- Öffnen Sie das Modell in Visual Studio (SSDT), Power BI Desktop oder Excel.

![Auswählen von Tabellen und Ansichten](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Wenn Sie die darin enthaltenen Modellmetadaten bearbeiten oder neue Kennzahlen in Ihrem Browser erstellen, speichern Sie diese Änderungen Ihres Modells in Azure. Wenn Sie Ihr Modell auch in SSDT, Power BI Desktop oder Excel bearbeiten, ist Ihr Modell möglicherweise nicht mehr synchron.


## <a name="next-steps"></a>Nächste Schritte 
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)  
[Herstellen einer Verbindung mit Excel](analysis-services-connect-excel.md)  


