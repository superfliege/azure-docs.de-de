---
title: "Azure Data Lake Tools: Verwenden von Azure Data Lake Tools für Visual Studio Code | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Data Lake Tools für Visual Studio Code verwenden, um U-SQL-Skripts zu erstellen, zu testen und auszuführen. "
Keywords: VScode,Azure Data Lake Tools,Local run,Local debug,Local Debug,preview file,upload to storage path,download,upload
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: jejiang
ms.openlocfilehash: 60307b8b16718fdc947bde7616532fa6a0920cf0
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Verwenden von Azure Data Lake Tools für Visual Studio Code

Lernen Sie die Azure Data Lake Tools für Visual Studio Code (VS Code), um U-SQL-Skripts zu erstellen, zu testen und auszuführen. Diese Informationen sind auch im folgenden Video enthalten:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Voraussetzungen

Azure Data Lake Tools für VSCode unterstützt Windows, Linux und MacOS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).

Für MacOS und Linux:
- [.NET Core SDK 2.0](https://www.microsoft.com/net/download/core) 
- [Mono 5.2.x](http://www.mono-project.com/download/)

## <a name="install-data-lake-tools"></a>Installieren von Data Lake Tools

Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie Data Lake Tools für VS Code installieren.

**So installieren Sie Data Lake Tools**

1. Öffnen Sie Visual Studio Code.
2. Klicken Sie im linken Bereich auf **Erweiterungen**. Geben Sie in das Suchfeld **Azure Data Lake** ein.
3. Klicken Sie neben **Azure Data Lake Tools** auf **Installieren**. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Erneut laden** geändert.
4. Klicken Sie auf **Erneut Laden**, um die Erweiterung **Azure Data Lake Tools** zu aktivieren.
5. Klicken Sie auf **Fenster erneut laden**, um zu bestätigen. **Azure Data Lake Tools** wird im Bereich „Erweiterungen“ angezeigt.

    ![Bereich „Erweiterungen“ für Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Aktivieren der Azure Data Lake Tools
Erstellen Sie eine neue USQL-Datei, oder öffnen Sie eine vorhandene USQL-Datei, um die Erweiterung zu aktivieren. 

## <a name="open-the-sample-script"></a>Öffnen des Beispielskripts
Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **ADL: Open Sample Script** ein. Dadurch wird eine weitere Instanz dieses Beispiels geöffnet. In dieser Instanz können Sie das Skript auch bearbeiten, konfigurieren und übermitteln.

## <a name="work-with-u-sql"></a>Arbeiten mit U-SQL

Sie müssen eine U-SQL-Datei oder einen entsprechenden Ordner öffnen, um mit U-SQL zu arbeiten.

**So öffnen Sie einen Ordner für das U-SQL-Projekt**

1. Wählen Sie in Visual Studio Code im Menü **Datei** die Option **Ordner öffnen** aus.
2. Geben Sie einen Ordner an, und wählen Sie dann **Ordner auswählen** aus.
3. Wählen Sie Menü **Datei** die Option **Neu** aus. Eine Datei mit dem Namen „Unbenannt-1“ wird dem Projekt hinzugefügt.
4. Geben Sie den folgenden Code in die Datei „Unbenannt-1“ ein:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Das Skript erstellt eine Datei mit dem Namen „departments.csv“ mit einigen Daten im Ordner „/Output“.

5. Speichern Sie die Datei als **myUSQL.usql** im geöffneten Ordner. Dem Ordner wird außerdem die Konfigurationsdatei „xxx_settings.json“ hinzugefügt.
6. Öffnen Sie „xxx_settings.json“, und konfigurieren Sie die Datei mit den folgenden Eigenschaften:

    - Konto: Ein Data Lake Analytics-Konto in Ihrem Azure-Abonnement, das zum Kompilieren und Ausführen von U-SQL-Aufträgen erforderlich ist. Daher müssen Sie das Computerkonto konfigurieren, bevor Sie U-SQL-Aufträge kompilieren und ausführen.
    - Datenbank: eine Datenbank in Ihrem Konto. Der Standardwert ist **master**.
    - Schema: ein Schema in der Datenbank. Der Standardwert ist **dbo**.
    - Optionale Einstellungen:
        - Priorität: Der Prioritätsbereich liegt zwischen 1 und 1000, wobei 1 die höchste Priorität ist. Der Standardwert ist **1000**.
        - Parallelität: Der Parallelitätsbereich liegt zwischen 1 und 150. Der Standardwert ist die maximal zulässige Parallelität in Ihrem Azure Data Lake Analytics-Konto. 
        
        ![Konfigurationsdatei der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)
      
        > [!NOTE] 
        > Nach dem Speichern der Konfiguration werden die Informationen zu Konto, Datenbank und Schema auf der Statusleiste links unten in der entsprechenden USQL-Datei angezeigt.

**So kompilieren Sie ein U-SQL-Skript**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2. Geben Sie **ADL: Skript kompilieren** ein. Die Kompilierungsergebnisse werden im Fenster **Ausgabe** angezeigt. Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann **ADL: Compile Script** auswählen, um einen U-SQL-Auftrag zu kompilieren. Das Kompilierungsergebnis wird im Bereich **Ausgabe** angezeigt.
 

**So übermitteln Sie ein U-SQL-Skript**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2. Geben Sie **ADL: Auftrag übermitteln** ein.  Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann **ADL: Submit Job** auswählen. 

Nach dem Übermitteln eines U-SQL-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **Ausgabe** angezeigt. Wenn die Übermittlung erfolgreich war, wird auch die Auftrags-URL angezeigt. Sie können die Auftrags-URL in einem Webbrowser öffnen, um den Auftragsstatus in Echtzeit nachzuverfolgen.

Legen Sie zum Aktivieren der Ausgabe von Auftragsdetails **jobInformationOutputPath** in VS Code in der Datei **u-sql_settings.json** fest.
 
## <a name="use-python-r-and-csharp-code-behind-file"></a>Verwenden einer Python-, R- und CSharp-CodeBehind-Datei
Azure Data Lake Tools unterstützt mehrere benutzerdefinierte Codes. Anweisungen finden Sie unter [Entwickeln von U-SQL mit Python, R und CSharp für Azure Data Lake Analytics in Visual Studio Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="use-assemblies"></a>Verwenden von Assemblys

Weitere Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).

Sie können mit Data Lake Tools benutzerdefinierte Codeassemblys im Data Lake Analytics-Katalog registrieren.

**So registrieren Sie eine Assembly**

Eine Assembly können Sie über die Befehle **ADL: Register Assembly** oder **ADL: Register Assembly through Configuration** registrieren.

**So führen Sie die Registrierung mit dem Befehl „ADL: Register Assembly“ durch**
1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2.  Geben Sie **ADL: Register Assembly** ein. 
3.  Geben Sie den lokalen Assemblypfad an. 
4.  Wählen Sie ein Data Lake Analytics-Konto aus.
5.  Wählen Sie eine Datenbank aus.

Ergebnis: Das Portal wird in einem Browser geöffnet, und der Assemblyregistrierungsprozess wird angezeigt.  

Alternativ können Sie den Befehl **ADL: Register Assembly** auch auslösen, indem Sie im Datei-Explorer mit der rechten Maustaste auf die DLL-Datei klicken. 

**So führen Sie die Registrierung mit dem Befehl „ADL: Register Assembly through Configuration“ durch**
1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2.  Geben Sie **ADL: Register Assembly through Configuration** ein. 
3.  Geben Sie den lokalen Assemblypfad an. 
4.  Die JSON-Datei wird angezeigt. Überprüfen und bearbeiten Sie bei Bedarf die Assemblyabhängigkeiten und Ressourcenparameter. Die entsprechenden Anweisungen werden im Fenster **Ausgabe** angezeigt. Speichern Sie die JSON-Datei (STRG+S), um mit der Assemblyregistrierung fortzufahren.

![CodeBehind für Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Assemblyabhängigkeiten: Azure Data Lake Tools erkennt automatisch, ob die DLL Abhängigkeiten aufweist. Die erkannten Abhängigkeiten werden nach ihrer Erkennung in der JSON-Datei angezeigt. 
>- Ressourcen: Sie können Ihre DLL-Ressourcen (z.B. TXT-, PNG- oder CSV-Dateien) als Teil der Assemblyregistrierung hochladen. 

Alternativ können Sie den Befehl **ADL: Register Assembly through Configuration** auch auslösen, indem Sie im Datei-Explorer mit der rechten Maustaste auf die DLL-Datei klicken. 

Der folgende U-SQL-Code veranschaulicht, wie eine Assembly aufgerufen wird. In diesem Beispiel lautet der Name der Assembly *test*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Damit Sie U-SQL-Skripts in Data Lake Analytics kompilieren und ausführen können, müssen Sie eine Verbindung mit Ihrem Azure-Konto herstellen.

**So stellen Sie eine Verbindung mit Azure her**

1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2.  Geben Sie **ADL: Login** ein. Die Anmeldeinformationen werden im Bereich **Ausgabe** angezeigt.

    ![Befehlspalette von Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Informationen zur Geräteanmeldung bei Data Lake Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Klicken Sie bei gedrückter STRG-Taste auf die Anmelde-URL https://aka.ms/devicelogin, um die Anmeldewebseite zu öffnen. Geben Sie den Code **G567LX42V** in das Textfeld ein, und wählen Sie dann **Fortfahren** aus.

   ![Einfügen des Codes zum Anmelden bei den Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Befolgen Sie die Anweisungen, um sich über die Webseite anzumelden. Wenn eine Verbindung hergestellt wurde, wird der Name Ihres Azure-Kontos auf der Statusleiste in der linken unteren Ecke des Fensters **Visual Studio Code** angezeigt. 

    > [!NOTE] 
    > Wenn für Ihr Konto zwei Faktoren aktiviert wurden, empfehlen wir anstelle einer PIN die Verwendung der Telefonauthentifizierung.

Zum Abmelden geben Sie den Befehl **ADL: Logout** ein.

## <a name="list-your-data-lake-analytics-accounts"></a>Auflisten Ihrer Data Lake Analytics-Konten

Um die Verbindung zu testen, können Sie eine Liste Ihrer Data Lake Analytics-Konten abrufen.

**So listen Sie die Data Lake Analytics-Konten in Ihrem Azure-Abonnement auf**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2. Geben Sie **ADL: List Accounts** ein. Die Konten werden im Bereich **Ausgabe** angezeigt.


## <a name="access-the-data-lake-analytics-catalog"></a>Zugreifen auf den Data Lake Analytics-Katalog

Nachdem Sie die Verbindung mit Azure hergestellt haben, können Sie mit den folgenden Schritten auf den U-SQL-Katalog zugreifen.

**So greifen Sie auf Azure Data Lake Analytics-Metadaten zu**

1.  Drücken Sie STRG+UMSCHALT+P, und geben Sie dann **ADL: List Tables** ein.
2.  Wählen Sie eines der Data Lake Analytics-Konten aus.
3.  Wählen Sie eine der Data Lake Analytics-Datenbanken aus.
4.  Wählen Sie eines der Schemas aus. Sie können die Liste der Tabellen anzeigen.

## <a name="view-data-lake-analytics-jobs"></a>Anzeigen von Data Lake Analytics-Aufträgen

**So zeigen Sie Data Lake Analytics-Aufträge an**
1.  Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und wählen Sie **ADL: Show Job** aus. 
2.  Wählen Sie ein Data Lake Analytics- oder ein lokales Konto aus. 
3.  Warten Sie, bis die Liste der Aufträge für das Konto angezeigt wird.
4.  Wählen Sie in der Auftragsliste einen Auftrag aus. Data Lake Tools öffnet die Auftragsdetails im Azure-Portal und zeigt die JobInfo-Datei in VS Code an.

    ![IntelliSense-Objekttypen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Integration von Azure Data Lake Storage

Sie können Befehle zu Azure Data Lake-Speicher für Folgendes verwenden:
 - Durchsuchen der Azure Data Lake-Speicherressourcen [Auflisten des Speicherpfads](#list-the-storage-path) 
 - Anzeigen einer Vorschau der Azure Data Lake-Speicherdatei [Anzeigen einer Vorschau der Speicherdatei](#preview-the-storage-file) 
 - Hochladen der Datei direkt in den Azure Data Lake-Speicher in VS Code [Hochladen von Dateien oder Ordnern](#upload-file-or-folder)
 - Herunterladen der Datei direkt aus dem Azure Data Lake-Speicher in VS Code [Herunterladen der Datei](#download-file)

## <a name="list-the-storage-path"></a>Auflisten des Speicherpfads 

**So listen Sie den Speicherpfad über die Befehlspalette auf**

Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **ADL: List Path** (ADL: Pfad auflisten) aus.

Wählen Sie den Ordner in der Liste aus, oder klicken Sie auf **Pfad eingeben** oder **Browse from Root** (Vom Stamm durchsuchen) (verwendet „Einen Pfad eingeben“ als Beispiel). -> Wählen Sie Ihr **ADLA-Konto** aus. -> Navigieren Sie zum Speicherordnerpfad, oder geben Sie ihn ein (z.B. /output/). -> In der Befehlspalette werden die auf Ihren Eingaben basierenden Pfadinformationen aufgelistet.

![Data Lake Tools für Visual Studio Code – Ergebnis für „List Storage Path“](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Alternativ können Sie den relativen Pfad auch auflisten, indem Sie durch Klicken mit der rechten Maustaste das Kontextmenü öffnen.

**So listen Sie den Speicherpfad durch Klicken mit der rechten Maustaste auf**

Klicken Sie mit der rechten Maustaste auf die Pfadzeichenfolge, um **List Path** (Pfad auflisten) auszuwählen, damit Sie fortfahren können.

![Data Lake Tools für Visual Studio Code – Anzeigen des Kontextmenüs](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Anzeigen einer Vorschau der Speicherdatei

Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **ADL: Preview File** (ADL: Vorschau der Datei anzeigen) aus.

Wählen Sie Ihr **ADLA-Konto** aus. -> Geben Sie einen Dateipfad für den Azure-Speicher ein (z.B. /output/SearchLog.txt). -> Ergebnis: Die Datei wird in VSCode geöffnet.

   ![Data Lake Tools für Visual Studio Code – Ergebnis für „Preview Storage File“](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Eine andere Möglichkeit für die Anzeige einer Vorschau der Datei ist, im Skript-Editor mit der rechten Maustaste auf den vollständigen oder relativen Pfad der Datei zu klicken. 

## <a name="upload-file-or-folder"></a>Hochladen von Dateien oder Ordnern

1. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **Datei hochladen** oder **Ordner hochladen** aus.

2. Wenn Sie auf „Datei hochladen“ geklickt haben, wählen Sie eine Datei oder mehrere Dateien aus. Wenn Sie auf „Ordner hochladen“ geklickt haben, wählen sie den gesamten Ordner aus. Klicken Sie anschließend auf **Hochladen**. Wählen Sie den Speicherordner in der Liste aus, oder klicken Sie auf **Pfad eingeben** oder **Browse from Root** (Vom Stamm durchsuchen). (Als Beispiel wird „Enter a path“ (Pfad eingeben) verwendet.) -> Wählen Sie Ihr **ADLA-Konto** aus. -> Navigieren Sie zum Speicherordnerpfad, oder geben Sie ihn ein (z.B. /output/). -> Klicken Sie auf **Choose Current Folder** (Aktuellen Ordner wählen), um Ihre Uploadziel anzugeben.

   ![Data Lake Tools für Visual Studio Code – Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


   Alternativ können Sie Dateien in den Speicher hochladen, indem Sie im Skript-Editor mit der rechten Maustaste auf den vollständigen oder relativen Pfad der Datei klicken.

Gleichzeitig können Sie den [Uploadstatus](#check-storage-tasks-status) überwachen.


## <a name="download-file"></a>Herunterladen der Datei 
Sie können Dateien herunterladen, indem Sie die Befehle **ADL: Download File** (ADL: Datei herunterladen) oder **ADL: Download File (Advanced)**  (ADL: Datei herunterladen (Erweitert)) eingeben.

**So laden Sie Dateien über „ADL: Download File (Advanced)“ (ADL: Datei herunterladen (Erweitert)) herunter**
1. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **Download File (Advanced)** (Datei herunterladen (Erweitert)) aus.
2. In VS Code wird eine JSON-Datei angezeigt. Sie können Dateipfade eingeben und mehrere Dateien gleichzeitig herunterladen. Die entsprechenden Anweisungen werden im Fenster **Ausgabe** angezeigt. Speichern Sie die JSON-Datei (STRG+S), um mit dem Herunterladen der Datei fortzufahren.

    ![Data Lake Tools für den Visual Studio Code-Download mit Konfiguration](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Ergebnis: Im Fenster **Ausgabe** wird der Dateiuploadstatus angezeigt.

    ![Data Lake Tools für den Visual Studio Code-Download von mehreren Dateiergebnissen](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Gleichzeitig können Sie den [Downloadstatus](#check-storage-tasks-status) überwachen.

**So laden Sie Dateien über „ADL: Download File“ (ADL: Datei herunterladen) herunter**

1. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, klicken Sie auf **Download File** (Datei herunterladen), und wählen sie dann im Dialogfeld **Select Folder** (Ordner auswählen) den Zielordner aus.

2. Wählen Sie den Ordner in der Liste aus, oder klicken Sie auf **Pfad eingeben** oder **Browse from Root** (Vom Stamm durchsuchen) (verwendet „Einen Pfad eingeben“ als Beispiel). -> Wählen Sie Ihr **ADLA-Konto** aus. -> Navigieren Sie zum Speicherordnerpfad, oder geben Sie ihn ein (Z.B. /output/) -> Wählen Sie eine Datei zum Download aus.

   ![Data Lake Tools für Visual Studio Code – Downloadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   
   Alternativ können Sie eine Speicherdateien herunterladen, indem Sie im Skript-Editor mit der rechten Maustaste auf den vollständigen oder relativen Pfad der Datei klicken.

Gleichzeitig können Sie den [Downloadstatus](#check-storage-tasks-status) überwachen.

## <a name="check-storage-tasks-status"></a>Überprüfen des Status der Speicheraufgabe
Der Status wird am unteren Rand der Statusleiste angezeigt, wenn der Download und Upload abgeschlossen ist.
1. Klicken Sie auf die nachstehende Statusleiste, und der Status des Downloads und Uploads wird im Bereich **AUSGABE** angezeigt.

   ![Data Lake Tools für Visual Studio Code – Überprüfen des Speicherstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)

## <a name="vscode-explorer-integration-with-azure-data-lake"></a>Integration des VSCode-Explorers in Azure Data Lake
1. Nach der Anmeldung werden alle Azure-Konten im linken Bereich von **Data Lake-Explorer** angezeigt. Wenn Sie eine Datenbank erweitern, können Sie unter dem Knoten die **Schemas**, **Tabellen**, **Assemblys** usw. anzeigen.

   ![Data Lake-Explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

2. Sie können den Befehl **Assembly registrieren** ausführen, indem Sie mit der rechten Maustaste auf den Knoten **Assemblys** klicken.

    ![Data Lake-Explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

3. Navigieren Sie zu **Speicherkonto**. Sie können eine Datei hoch- oder herunterladen, indem Sie mit der rechten Maustaste auf den Ordner oder die Datei klicken. Im Kontextmenü stehen darüber hinaus die Optionen **Vorschau** (einer Datei), **Herunterladen**, **Relativen Pfad kopieren**, **Vollständigen Pfad kopieren** zur Verfügung.

   ![Data Lake-Explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-download-preview-file.png)

## <a name="open-adl-storage-explorer-in-portal"></a>Öffnen des ADL-Speicher-Explorers im Portal
1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2. Geben Sie **Open Web Azure Storage Explorer** ein, oder klicken Sie im Skript-Editor mit der rechten Maustaste auf einen relativen oder vollständigen Pfad, und wählen Sie dann **Open Web Azure Storage Explorer** aus.
3. Wählen Sie ein Data Lake Analytics-Konto aus.

Data Lake Tools öffnet den Azure-Speicherpfad im Azure-Portal. Sie können den Pfad aus dem Web suchen und eine Vorschau der Datei anzeigen.

## <a name="local-run-and-local-debug-for-windows-users"></a>Lokales Ausführen und lokales Debuggen für Windows-Benutzer
Bei einer lokalen Ausführung des U-SQL-Skripts werden Ihre lokalen Daten und das Skript lokal überprüft, bevor Ihr Code in Data Lake Analytics veröffentlicht wird. Mit der lokalen Debugfunktion können Sie die folgenden Aufgaben ausführen, bevor Ihr Code an Data Lake Analytics übermittelt wird: 
- Debuggen Ihres C#-CodeBehind 
- Ausführen des Codes in einzelnen Schritten 
- Lokales Überprüfen des Skripts

Informationen zum lokalen Ausführen und lokalen Debuggen finden Sie unter [Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Zusätzliche Funktionen

Data Lake Tools für VS Code unterstützt die folgenden Features:

-   Automatische IntelliSense-Vervollständigung: Vorschläge werden in Popupfenstern für Elemente wie z.B. Schlüsselwörter, Methoden und Variablen angezeigt. Unterschiedliche Symbole stellen die verschiedene Objekttypen dar:

    - Scala-Datentyp
    - Komplexer Datentyp
    - Integrierte UDTs
    - .NET-Sammlung und -Klassen
    - C#-Ausdrücke
    - Integrierte C#-UDFs, -UDOs und -UDAAGs 
    - U-SQL-Funktionen
    - U-SQL-Windowingfunktionen
 
    ![IntelliSense-Objekttypen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   Automatische IntelliSense-Vervollständigung für Data Lake Analytics-Metadaten: Data Lake Tools lädt die Data Lake Analytics-Metadateninformationen lokal herunter. Das IntelliSense-Feature füllt automatisch die Objekte anhand der Data Lake Analytics-Metadaten aus. Dazu gehören u.a. Datenbank, Schema, Tabelle, Sicht, Tabellenwertfunktion, Prozeduren und C#-Assemblys.
 
    ![IntelliSense-Metadaten der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense-Fehlerkennzeichnung: Data Lake Tools unterstreicht die Bearbeitungsfehler für U-SQL und C#. 
-   Syntaxhervorhebung: Data Lake Tools verwendet verschiedene Farben, um Elemente wie Variablen, Schlüsselwörter, Datentypen und Funktionen zu unterscheiden. 

    ![Syntaxmarkierungen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nächste Schritte
- [Entwickeln von U-SQL mit Python, R und CSharp für Azure Data Lake Analytics in Visual Studio Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [Lokales Ausführen und lokales Debuggen für Windows von U-SQL mit Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
- [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md)




