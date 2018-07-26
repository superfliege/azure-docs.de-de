---
title: Entwickeln einer U-SQL-Datenbank für Azure Data Lake mithilfe eines U-SQL-Datenbankprojekts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine U-SQL-Datenbank mithilfe von Azure Data Lake Tools für Visual Studio entwickeln.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a8099a8c9bcec8ed5a7bb480dae58d2c077b8fe0
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37890820"
---
# <a name="use-u-sql-database-project-to-develop-u-sql-database-for-azure-data-lake"></a>Entwickeln einer U-SQL-Datenbank für Azure Data Lake mithilfe eines U-SQL-Datenbankprojekts

Eine U-SQL-Datenbank ermöglicht strukturierte Ansichten der unstrukturierten Daten und die Verwaltung von strukturierten Daten in Tabellen und umfasst ein allgemeines Metadatenkatalogsystem zum Organisieren von strukturierten Daten und benutzerdefiniertem Code. Über die Datenbank werden diese verbundenen Objekte konzeptionell gruppiert.

Erfahren Sie mehr über die [U-SQL-Datenbank und Data Definition Language (DDL)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

Ein U-SQL-Datenbankprojekt ist ein Projekttyp in Visual Studio, mit dem Entwickler U-SQL-Datenbanken schnell und einfach entwickeln, verwalten und bereitstellen können.

## <a name="create-a-u-sql-database-project"></a>Erstellen eines U-SQL-Datenbankprojekts

In Azure Data Lake Tools für Visual Studio nach Version 2.3.3000.0 wurde eine neue Projektvorlage für U-SQL-Datenbankprojekte hinzugefügt. Zum Erstellen eines U-SQL-Projekts navigieren Sie zu **Datei > Neu > Projekt**. Die Vorlage für U-SQL-Datenbankprojekte finden Sie dann unter **Azure Data Lake > Knoten „U-SQL“**.

![Data Lake Tools für Visual Studio, Erstellen eines U-SQL-Datenbankprojekts](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-using-database-project"></a>Entwickeln von U-SQL-Datenbankobjekten mithilfe eines Datenbankprojekts

Klicken Sie mit der rechten Maustaste auf das U-SQL-Datenbankprojekt, und klicken Sie auf **Hinzufügen > Neues Element**. Im Assistenten „Neues Element hinzufügen“ werden alle unterstützten Objekttypen angezeigt. 

1.  Für ein Nicht-Assemblyobjekt, z.B. eine Tabellenwertfunktion, wird nach dem Hinzufügen eines neuen Elements ein neues U-SQL-Skript erstellt. Dann können Sie die DDL-Anweisung für das Objekt im Editor entwickeln.
2.  Für ein Assemblyobjekt umfasst das Tool einen benutzerfreundlichen Benutzeroberflächen-Editor, in dem Sie die Assembly registrieren und DLL- und zusätzliche Dateien bereitstellen können. Führen Sie die nachfolgenden Schritte aus, um dem U-SQL-Datenbankprojekt eine Assemblyobjektdefinition hinzuzufügen:

1.  Fügen Sie Verweise des C#-Projekts hinzu, das UDO/UDAG/UDF für das U-SQL-Datenbankprojekt enthält.

    ![Data Lake Tools für Visual Studio, Hinzufügen eines Verweises des U-SQL-Datenbankprojekts](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools für Visual Studio, Hinzufügen eines Verweises des U-SQL-Datenbankprojekts](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Wählen Sie in der Entwurfsansicht der Assembly in der Dropdownliste **Assembly aus Verweis erstellen** die referenzierte Assembly aus.

    ![Data Lake Tools für Visual Studio, Assembly aus Verweis erstellen](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Fügen Sie **verwaltete Abhängigkeiten** und **zusätzliche Dateien** hinzu, sofern vorhanden. Beim Hinzufügen zusätzlicher Dateien verwendet das Tool den relativen Pfad, um sicherzustellen, dass die Assemblys später auf Ihrem lokalen Computer und dem Buildcomputer gefunden werden. @_DeployTempDirectory ist eine vordefinierte Variable, die das Tool auf den Buildausgabeordner verweist. In der Buildausgabe befindet sich für jede Assembly ein Unterordner mit dem jeweiligen Assemblynamen. Der Unterordner enthält alle DLLs und zusätzlichen Dateien. 
 
## <a name="build-u-sql-database-project"></a>Erstellen des U-SQL-Datenbankprojekts

Die Buildausgabe für das U-SQL-Datenbankprojekt ist ein U-SQL-Datenbankbereitstellungspaket, das mit dem Suffix `.usqldbpack` benannt ist. Das `.usqldbpack`-Paket ist eine ZIP-Datei und enthält alle DDL-Anweisungen in einem einzelnen U-SQL-Skript im Ordner **DDL** sowie alle DLLs und zusätzlichen Dateien für Assemblys im Ordner **Temp**.

Erfahren Sie mehr zum [Erstellen eines U-SQL-Datenbankprojekts mit der MSBuild-Befehlszeile und Visual Studio Team Service-Buildaufgabe](data-lake-analytics-cicd-overview.md#build-u-sql-database-project).

## <a name="deploy-u-sql-database"></a>Bereitstellen einer U-SQL-Datenbank

Das .usqldbpack-Paket kann mit Visual Studio oder dem Bereitstellungs-SDK im lokalen Konto oder im Azure Data Lake Analytics-Konto bereitgestellt werden. 

### <a name="deploy-u-sql-database-in-visual-studio"></a>Bereitstellen einer U-SQL-Datenbank in Visual Studio

Sie können eine U-SQL-Datenbank über ein U-SQL-Datenbankprojekt oder ein .usqldbpack-Paket in Visual Studio bereitstellen.

#### <a name="deploy-through-u-sql-database-project"></a>Bereitstellen über ein U-SQL-Datenbankprojekt

1.  Klicken Sie mit der rechten Maustaste auf das U-SQL-Datenbankprojekt, und wählen Sie **Bereitstellen** aus.
2.  Wählen Sie im Assistenten „U-SQL-Datenbank bereitstellen“ das **ADLA-Konto** aus, in dem Sie die Datenbank bereitstellen möchten. Sowohl das lokale Konto als auch das ADLA-Konto werden unterstützt.
3.  **Datenbankquelle** wird automatisch ausgefüllt und verweist auf das .usqldbpack-Paket im Buildausgabeordner des Projekts.
4.  Geben Sie den **Datenbanknamen** zum Erstellen einer Datenbank ein. Wenn eine Datenbank mit demselben Namen im Azure Data Lake Analytics-Zielkonto vorhanden ist, werden alle im Datenbankprojekt definierten Objekte erstellt, ohne dass die Datenbank neu erstellt wird.
5.  Klicken Sie auf **Senden**, um die U-SQL-Datenbank bereitzustellen. Alle Ressourcen (Assemblys und zusätzliche Dateien) werden hochgeladen, und ein U-SQL-Auftrag umfasst alle gesendeten DDL-Anweisungen.

    ![Data Lake Tools für Visual Studio, Bereitstellen eines U-SQL-Datenbankprojekts](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools für Visual Studio, Assistent zum Bereitstellen eines U-SQL-Datenbankprojekts](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-u-sql-database-deployment-package"></a>Bereitstellen über das U-SQL-Datenbankbereitstellungspaket

1.  Öffnen Sie den **Server-Explorer**, und erweitern Sie das **Azure Data Lake Analytics-Konto**, in dem Sie die Datenbank bereitstellen möchten.
2.  Klicken Sie mit der rechten Maustaste auf „U-SQL-Datenbanken“, und wählen Sie **Datenbank bereitstellen** aus.
3.  Legen Sie **Datenbankquelle** auf den Pfad des U-SQL-Datenbankbereitstellungspakets (.usqldbpack-Datei) fest.
4.  Geben Sie den **Datenbanknamen** zum Erstellen einer Datenbank ein. Wenn eine Datenbank mit demselben Namen im Azure Data Lake Analytics-Zielkonto vorhanden ist, werden alle im Datenbankprojekt definierten Objekte erstellt, ohne dass die Datenbank neu erstellt wird.

    ![Data Lake Tools für Visual Studio, Bereitstellen eines U-SQL-Datenbankpakets](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Data Lake Tools für Visual Studio, Assistent zum Bereitstellen eines U-SQL-Datenbankpakets](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-using-sdk"></a>Bereitstellen einer U-SQL-Datenbank mit dem SDK

`PackageDeploymentTool.exe` enthält die Programmier- und Befehlszeilenschnittstellen, über die U-SQL-Datenbanken bereitgestellt werden können. Das SDK ist im [U-SQL SDK-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) enthalten und befindet sich unter `build/runtime/PackageDeploymentTool.exe`.

[Erfahren Sie mehr über das SDK und zum Einrichten einer CI/CD-Pipeline für die Bereitstellung von U-SQL-Datenbanken](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>Verweisen auf ein U-SQL-Datenbankprojekt

Ein U-SQL-Projekt kann auf ein U-SQL-Datenbankprojekt verweisen. Der Verweis wirkt sich auf zwei Workloads aus:

- Projekterstellung: Die referenzierten Datenbankumgebungen werden vor dem Erstellen der U-SQL-Skripts eingerichtet. 
- Lokale Ausführung im Konto „(Local-project)“: Die referenzierten Datenbankumgebungen werden vor dem Erstellen der U-SQL-Skripts im Konto „(Local-project)“ bereitgestellt. [Erfahren Sie mehr über die lokale Ausführung und den Unterschied zwischen dem Konto „(Local-machine)“ und dem Konto „(Local-project)“](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-u-sql-database-reference"></a>So fügen Sie einen U-SQL-Datenbankverweis hinzu

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das U-SQL-Projekt, und wählen Sie **U-SQL-Datenbankverweis hinzufügen** aus.

    ![Data Lake Tools für Visual Studio, Hinzufügen eines Datenbankprojektverweises](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Konfigurieren Sie den Datenbankverweis aus einem U-SQL-Datenbankprojekt in der aktuellen Projektmappe oder einer U-SQL-Datenbankpaketdatei.
3. Geben Sie den Namen für die Datenbank an.

    ![Data Lake Tools für Visual Studio, Assistent zum Hinzufügen eines Datenbankprojektverweises](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten einer CI/CD-Pipeline für Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Testen des Azure Data Lake Analytics-Codes](data-lake-analytics-cicd-test.md)
- [Ausführen von U-SQL-Skripts auf dem lokalen Computer](data-lake-analytics-data-lake-tools-local-run.md)
