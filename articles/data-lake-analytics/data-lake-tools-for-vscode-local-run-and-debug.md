---
title: "Azure Data Lake Tools: Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Data Lake Tools für Visual Studio Code zum lokalen Ausführen und lokalen Debuggen verwenden können."
Keywords: "VSCode, Azure Data Lake Tools, lokales Ausführen, lokales Debuggen, lokales Debugging, Vorschau Speicherdatei, Hochladen in Speicherpfad"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 78a5efb19f73192dcc95103abc70c14a3ce2e29a
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="u-sql-local-run-and-local-debug-for-windows-with-visual-studio-code"></a>Lokales Ausführen und lokales Debuggen für Windows von U-SQL mit Visual Studio Code
In diesem Dokument erfahren Sie, wie Sie U-SQL-Aufträge auf einem lokalen Entwicklungscomputer ausführen, um frühe Codierungsphasen zu beschleunigen oder Code lokal in Visual Studio Code zu debuggen. Weitere Informationen zur Verwendung von Azure Data Lake Tools für Visual Studio Code finden Sie unter [Verwenden von Azure Data Lake Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md). 


## <a name="set-up-the-u-sql-local-run-environment"></a>Einrichten der Umgebung zum lokalen Ausführen von U-SQL

1. Öffnen Sie die Befehlspalette durch Drücken von STRG+UMSCHALT+P, und geben Sie **ADL: Download LocalRun Dependency** ein, um die Pakete herunterzuladen.  

   ![Herunterladen der ADL-Abhängigkeitspakete für die lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Suchen Sie die Abhängigkeitspakete unter dem im Bereich **Ausgabe** angezeigten Pfad, und installieren Sie dann BuildTools und Win10SDK 10240. Beispielpfad:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Suchen der Abhängigkeitspakete](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 Klicken Sie zum Installieren von **BuildTools** auf „visualcppbuildtools_full.exe“ in dem Ordner „LocalRunDependency“, und folgen Sie anschließend den Anweisungen des Assistenten.   

    ![Installieren der BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 Klicken Sie zum Installieren von **Win10SDK 10240** auf „sdksetup.exe“ in dem Ordner „LocalRunDependency/Win10SDK_10.0.10240_2“, und folgen Sie anschließend den Anweisungen des Assistenten.  

    ![Installieren von Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Legen Sie die Umgebungsvariable fest. Legen Sie die Umgebungsvariable **SCOPE_CPP_SDK** auf folgenden Wert fest:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Starten Sie das Betriebssystem neu, damit die Umgebungsvariableneinstellungen wirksam werden.  

   ![Überprüfen der Umgebungsvariable SCOPE_CPP_SDK](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Starten des Diensts für die lokale Ausführung und Übermitteln eines U-SQL-Auftrags an ein lokales Konto 
Anweisung für Erstbenutzer: Verwenden Sie **ADL: Download Local Run Dependency**, um lokal ausgeführte Pakete herunterzuladen, wenn [die Umgebung zum lokalen Ausführen von U-SQL](#set-up-the-u-sql-local-run-environment) nicht eingerichtet ist.

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, und geben Sie **ADL: Start Local Run Service** ein.   
2. Wählen Sie **Accept** (Zustimmen) aus, um die Microsoft Software-Lizenzbedingungen erstmalig zu akzeptieren. 

   ![Annehmen der Microsoft-Software-Lizenzbedingungen](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Die Befehlskonsole wird geöffnet. Als Erstbenutzer müssen Sie **3** eingeben. Ermitteln Sie dann den lokalen Ordnerpfad für die Eingabe und Ausgabe Ihrer Daten. Für die anderen Optionen können Sie die Standardwerte verwenden. 

   ![Data Lake Tools für Visual Studio Code – Befehlskonsole für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, geben Sie **ADL: Submit Job** ein, und wählen Sie dann **Local** aus, um den Auftrag an Ihr lokales Konto zu übermitteln.

   ![Data Lake Tools für Visual Studio Code – Auswählen von „local“](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Nach dem Übermitteln des Auftrags können Sie Übermittlungsdetails anzeigen. Wählen Sie zum Anzeigen der Übermittlungsdetails im Fenster **Ausgabe** die Option **jobUrl** aus. Sie können den Übermittlungsstatus des Auftrags auch über die Befehlskonsole anzeigen. Geben Sie an der Befehlskonsole **7** ein, wenn Sie weitere Auftragsdetails anzeigen möchten.

   ![Data Lake Tools für Visual Studio Code – Ausgabe für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools für Visual Studio Code – Status für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Starten des lokalen Debuggens für den U-SQL-Auftrag  
Anweisung für Erstbenutzer:

1. Verwenden Sie **ADL: Download Local Run Dependency**, um lokal ausgeführte Pakete herunterzuladen, wenn [die Umgebung zum lokalen Ausführen von U-SQL](#set-up-the-u-sql-local-run-environment) nicht eingerichtet ist.
2. Installieren Sie, falls noch nicht geschehen, wie empfohlen .NET Core SDK 2.0 im Nachrichtenfeld.
 
  ![Erinnerung installiert Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Installieren Sie, falls noch nicht geschehen, wie empfohlen C# für Visual Studio Code im Nachrichtenfeld. Klicken Sie auf **Installieren**, um fortzufahren, und starten Sie VSCode anschließend neu.

    ![Erinnerung an die Installation von C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Folgen Sie den untenstehenden Schritten, um lokal zu debuggen:
  
1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, und geben Sie **ADL: Start Local Run Service** ein. Die Befehlskonsole wird geöffnet. Stellen Sie sicher, dass **DataRoot** festgelegt ist.
2. Legen Sie einen Breakpoint im zugrunde liegenden C#-Code fest.
3. Gehen Sie zurück zum Skript-Editor, klicken Sie mit der rechten Maustaste, und wählen Sie **ADL: Local Debug** aus.
    
   ![Data Lake Tools für Visual Studio Code – Ergebnis des lokales Debuggens](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Verwendung von Azure Data Lake Tools für Visual Studio Code finden Sie unter [Verwenden von Azure Data Lake Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Weitere Informationen zu den ersten Schritten mit Data Lake Analytics finden Sie unter [Tutorial: Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Weitere Informationen zu Data Lake Tools für Visual Studio finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).
