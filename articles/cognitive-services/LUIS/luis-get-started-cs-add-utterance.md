---
title: 'Schnellstart: Ändern des Modells und Trainieren der LUIS-App mit C# – Azure Cognitive Services | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung für C# fügen Sie einer Home Automation-App Beispieläußerungen hinzu, und Sie trainieren die App. Bei Beispieläußerungen handelt es sich um Konversationstext von Benutzern, der einer Absicht zugeordnet ist. Indem Sie Beispieläußerungen für Absichten bereitstellen, teilen Sie LUIS mit, welche Arten des vom Benutzer angegebenen Texts zu welcher Absicht gehören.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: fee0f9009e7ce6cef839010f68ae3487067152b9
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43770063"
---
# <a name="quickstart-change-model-using-c"></a>Schnellstart: Ändern des Modells mit C#

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Aktuelle [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/)
* C#-Programmiersprache installiert
* NuGet-Pakete [JsonFormatterPlus](https://www.nuget.org/packages/JsonFormatterPlus) und [CommandLine](https://www.nuget.org/packages/CommandLineParser/)

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Erstellen des Schnellstartcodes 

Erstellen Sie in Visual Studio mithilfe von .Net Framework eine neue App für eine **klassische Windows-Desktopkonsole**. 

![Visual Studio-Projekttyp](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

### <a name="add-the-systemweb-dependency"></a>Hinzufügen der System.Web-Abhängigkeit

Für das Visual Studio-Projekt ist **System.Web** erforderlich. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise** und dann auf **Verweis hinzufügen**.

![Hinzufügen des Verweises auf „System.web“](./media/luis-quickstart-cs-add-utterance/system.web.png)

### <a name="add-other-dependencies"></a>Hinzufügen anderer Abhängigkeiten

Für das Visual Studio-Projekt sind **JsonFormatterPlus** und **CommandLineParser** erforderlich. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und wählen Sie **NuGet-Pakete verwalten** aus. Suchen Sie nach den beiden Paketen, und fügen Sie sie hinzu. 

![Hinzufügen von Abhängigkeiten von Drittanbietern](./media/luis-quickstart-cs-add-utterance/add-dependencies.png)


### <a name="write-the-c-code"></a>Schreiben des C#-Codes
Die Datei **Program.cs** sollte wie folgt aussehen:

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp3
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Fügen Sie die Abhängigkeiten hinzu.

   [!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=1-11 "Add the dependencies")]


Fügen Sie die LUIS-IDs und Zeichenfolgen zur **Program**-Klasse hinzu.

   [!code-csharp[Add the LUIS IDs and strings](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=19-30&dedent=8 "Add the LUIS IDs and strings")]

Fügen Sie der **Program**-Klasse die Klasse zum Verwalten von Befehlszeilenparametern hinzu.

   [!code-csharp[Add class to manage command line parameters.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=32-46 "Add class to manage command-line parameters.")]

Fügen Sie der **Program**-Klasse die GET-Anforderungsmethode hinzu.

   [!code-csharp[Add the GET request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=49-59 "Add the GET request.")]


Fügen Sie der **Program**-Klasse die POST-Anforderungsmethode hinzu. 

   [!code-csharp[Add the POST request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=60-76 "Add the POST request.")]

Fügen Sie der **Program**-Klasse Beispieläußerungen aus der Dateimethode hinzu.

   [!code-csharp[Add example utterances from file.
](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=77-86 "Add example utterances from file.
")]

Trainieren Sie das Modell, nachdem die Änderungen angewendet wurden. Fügen Sie der **Program**-Klasse eine Methode hinzu.

   [!code-csharp[After the changes are applied to the model, train the model.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=87-96 "After the changes are applied to the model, train the model.")]

Das Training wird möglicherweise nicht sofort abgeschlossen. Überprüfen Sie den Status, um festzustellen, ob es abgeschlossen ist. Fügen Sie der **Program**-Klasse eine Methode hinzu.

   [!code-csharp[Training may not complete immediately, check status to verify training is complete.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=97-103 "Training may not complete immediately, check status to verify training is complete.")]

Fügen Sie zum Verwalten von Befehlszeilenargumenten den Hauptcode hinzu. Fügen Sie der **Program**-Klasse eine Methode hinzu.

   [!code-csharp[To manage command line arguments, add the main code.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=104-137 "To manage command-line arguments, add the main code.")]

### <a name="copy-utterancesjson-to-output-directory"></a>Kopieren von „utterances.json“ in das Ausgabeverzeichnis

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `utterances.json`, und wählen Sie **Eigenschaften** aus. Markieren Sie im Fenster mit den Eigenschaften **Buildaktion** von `Content` und **In Ausgabeverzeichnis kopieren** von `Copy Always`.  

![Markieren der JSON-Datei als Inhalt](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="build-code"></a>Erstellen des Codes

Erstellen Sie den Code in Visual Studio. 

## <a name="run-code"></a>Ausführen des Codes

Führen Sie die Anwendung im Verzeichnis „/bin/Debug“ des Projekts über eine Befehlszeile aus. 

```CMD
ConsoleApp\bin\Debug> ConsoleApp1.exe --add utterances.json --train --status
```

Die folgende Befehlszeile zeigt das Ergebnis des Aufrufs der API zum Hinzufügen von Äußerungen an. 

[!include[Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Entfernen Sie nach dem Durchführen dieses Schnellstarts alle Dateien, die Sie im Rahmen der Schnellstartanleitung erstellt haben. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Programmgesteuertes Erstellen einer LUIS-App](luis-tutorial-node-import-utterances-csv.md) 