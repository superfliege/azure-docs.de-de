---
title: Bewährte Methode zum Verwalten von U-SQL-Assemblys in der CI/CD-Pipeline für Azure Data Lake
description: Lernen Sie die bewährte Methode zum Verwalten von U-SQL-C#-Assemblys in der CI/CD-Pipeline mit Azure DevOps kennen.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670287"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>Bewährte Methode zum Verwalten von U-SQL-Assemblys in der CI/CD-Pipeline

In diesem Artikel erfahren Sie, wie Sie den Quellcode der U-SQL-Assembly mit dem neu eingeführten U-SQL-Datenbankprojekt verwalten können. Sie können sich auch damit vertraut machen, wie Sie mit Azure DevOps eine CI/CD-Pipeline (Continuous Integration und Continuous Deployment) für die Registrierung von Assemblys mit Azure DevOps einrichten.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Verwenden des U-SQL-Datenbankprojekts zum Verwalten von Assemblyquellcode

Das [U-SQL-Datenbankprojekt](data-lake-analytics-data-lake-tools-develop-usql-database.md) ist ein Projekttyp in Visual Studio, mit dem Entwickler U-SQL-Datenbanken schnell und einfach entwickeln, verwalten und bereitstellen können. Alle U-SQL-Datenbankobjekte (mit Ausnahme der Anmeldeinformationen) können mit dem U-SQL-Datenbankprojekt verwaltet werden. 

Dies ist die empfohlene Vorgehensweise für die Verwaltung des Quellcodes der C#-Assembly und der DDL U-SQL-Skripts für die Assemblyregistrierung :

* Verwenden Sie das **U-SQL Datenbankprojekt**, um U-SQL-Skripts zur Registrierung von Assemblys zu verwalten.
* Verwenden Sie die **Klassenbibliothek (für U-SQL-Anwendungen)**, um den C#-Quellcode und Abhängigkeiten für benutzerdefinierte Operatoren, Funktionen und Aggregatoren (UDOs/UDFs/UDAGs) zu verwalten.
* Verwenden Sie das U-SQL-Datenbankprojekt, um auf das Klassenbibliothekprojekt zu verweisen. 

Ein U-SQL-Datenbankprojekt kann auf ein Klassenbibliothekprojekt (für U-SQL-Anwendungen) verweisen. Assemblys, die in der U-SQL-Datenbank registriert sind, können mit referenziertem C#-Quellcode aus diesem Klassenbibliothekprojekt (für U-SQL-Anwendungen) erstellt werden.

Sie können die folgenden Schritte ausführen, um Projekte zu erstellen und Verweise hinzuzufügen:
1. Erstellen Sie ein Klassenbibliothekprojekt (für U-SQL-Anwendungen) über **Datei > Neu > Projekt**. Das Projekt befindet sich unter dem Knoten **Azure Data Lake > U-SQL**.
   ![Data Lake Tools für Visual Studio: Erstellen eines U-SQL-C#-Klassenbibliothekprojekts](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. Fügen Sie Ihren benutzerdefinierten C#-Code in das Klassenbibliothekprojekt (für U-SQL-Anwendungen) ein. 
3. Erstellen Sie ein U-SQL-Projekt über **Datei > Neu > Projekt**. Das Projekt befindet sich unter dem Knoten **Azure Data Lake > U-SQL**.
   ![Data Lake Tools für Visual Studio: Erstellen eines U-SQL-Datenbankprojekts](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. Fügen Sie einen Verweis auf das C#-Klassenbibliothekprojekt für das U-SQL-Datenbankprojekt hinzu.

    ![Data Lake Tools für Visual Studio: Hinzufügen eines Verweises des U-SQL-Datenbankprojekts](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools für Visual Studio: Hinzufügen eines Verweises des U-SQL-Datenbankprojekts](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. Erstellen Sie ein Assemblyskript im U-SQL-Datenbankprojekt durch **Klicken mit der rechten Maustaste auf das Projekt > Neues Element hinzufügen**.
   ![Data Lake Tools für Visual Studio: Hinzufügen eines Assemblyskripts](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. Öffnen Sie das Assemblyskript, und wählen Sie in der Entwurfsansicht der Assembly im Dropdownmenü **Assembly aus Verweis erstellen** die referenzierte Assembly aus.

    ![Data Lake Tools für Visual Studio: Erstellen einer Assembly aus einem Verweis](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Fügen Sie **verwaltete Abhängigkeiten** und **zusätzliche Dateien** hinzu, sofern vorhanden. Beim Hinzufügen zusätzlicher Dateien verwendet das Tool den relativen Pfad, um sicherzustellen, dass die Assemblys später auf Ihrem lokalen Computer und dem Buildcomputer gefunden werden. 

**@_DeployTempDirectory** unten im Editor-Fenster ist eine vordefinierte Variable, die das Tool auf den Buildausgabeordner verweist. Im Buildausgabeordner verfügt jede Assembly über einen Unterordner, der den gleichen Namen wie die Assembly besitzt. Alle DLLs und zusätzlichen Dateien befinden sich in diesem Unterordner. 

## <a name="build-a-u-sql-database-project"></a>Erstellen eines U-SQL-Datenbankprojekts

Die Buildausgabe für ein U-SQL-Datenbankprojekt ist ein U-SQL-Datenbankbereitstellungspaket, das mit dem Suffix `.usqldbpack` benannt ist. Das Paket `.usqldbpack` ist eine ZIP-Datei und enthält alle DDL-Anweisungen in einem einzelnen U-SQL-Skript im Ordner **DDL** sowie alle erstellten DLLs und zusätzlichen Dateien für Assemblys im Ordner **Temp**.

## <a name="deploy-a-u-sql-database"></a>Bereitstellen einer U-SQL-Datenbank

Das Paket vom Typ „`.usqldbpack`“ kann mit Visual Studio oder dem Bereitstellungs-SDK in einem lokalen Konto oder in einem Azure Data Lake Analytics-Konto bereitgestellt werden. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Bereitstellen einer U-SQL-Datenbank in Visual Studio

Sie können eine U-SQL-Datenbank über ein U-SQL-Datenbankprojekt oder ein `.usqldbpack`-Paket in Visual Studio bereitstellen.

#### <a name="deploy-through-a-u-sql-database-project"></a>Bereitstellen über ein U-SQL-Datenbankprojekt

1.  Klicken Sie mit der rechten Maustaste auf das U-SQL-Datenbankprojekt, und wählen Sie dann **Bereitstellen** aus.
2.  Wählen Sie im Assistenten **U-SQL-Datenbank bereitstellen** das **ADLA-Konto** aus, in dem Sie die Datenbank bereitstellen möchten. Sowohl lokale Konten als auch ADLA-Konten werden unterstützt.
3.  **Datenbankquelle** wird automatisch ausgefüllt und verweist auf das Paket vom Typ „.usqldbpack“ im Buildausgabeordner des Projekts.
4.  Geben Sie unter **Datenbankname** einen Namen ein, um eine Datenbank zu erstellen. Wenn bereits eine Datenbank mit demselben Namen im Azure Data Lake Analytics-Zielkonto vorhanden ist, werden alle im Datenbankprojekt definierten Objekte erstellt, ohne dass die Datenbank neu erstellt wird.
5.  Klicken Sie auf **Senden**, um die U-SQL-Datenbank bereitzustellen. Alle Ressourcen (Assemblys und zusätzliche Dateien) werden hochgeladen, und ein U-SQL-Auftrag mit allen gesendeten DDL-Anweisungen wird gesendet.

    ![Data Lake Tools für Visual Studio: Bereitstellen eines U-SQL-Datenbankprojekts](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools für Visual Studio: Assistent zum Bereitstellen eines U-SQL-Datenbankprojekts](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Bereitstellen einer U-SQL-Datenbank in Azure DevOps

`PackageDeploymentTool.exe` enthält die Programmier- und Befehlszeilenschnittstellen, über die U-SQL-Datenbanken bereitgestellt werden können. Das SDK ist im [U-SQL SDK-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) enthalten und befindet sich unter `build/runtime/PackageDeploymentTool.exe`.

In Azure DevOps können Sie einen Befehlszeilentask und dieses SDK verwenden, um eine Automatisierungspipeline für die Aktualisierung der U-SQL-Datenbank einzurichten. [Erfahren Sie mehr über das SDK und zum Einrichten einer CI/CD-Pipeline für die Bereitstellung von U-SQL-Datenbanken](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten einer CI/CD-Pipeline für Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
* [Testen des Azure Data Lake Analytics-Codes](data-lake-analytics-cicd-test.md)
* [Ausführen von U-SQL-Skripts auf dem lokalen Computer](data-lake-analytics-data-lake-tools-local-run.md)
