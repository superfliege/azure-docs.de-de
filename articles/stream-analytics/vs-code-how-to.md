---
title: Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)
description: In diesem Artikel wird veranschaulicht, wie ein Azure Stream Analytics-Auftrag in ein lokales Projekt exportiert wird, Aufträge aufgelistet und Auftragsentitäten angezeigt werden können. Außerdem erfahren Sie, wie Sie eine CI/CD-Pipeline für Ihren Stream Analytics-Auftrag einrichten.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: conceptual
ms.openlocfilehash: 640a81fcb94194e2e907e9339f016bd35e279fdd
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080676"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)

Die Azure Stream Analytics-Erweiterung für Visual Studio Code bietet Entwicklern eine einfache Verwaltung Ihrer Stream Analytics-Aufträge. Mit der Azure Stream Analytics-Erweiterung ist Folgendes möglich:

- [Erstellen](quick-create-vs-code.md), Starten und Aufträge beenden
- Exportieren vorhandener Aufträge in ein lokales Projekt
- Lokales Ausführen von Abfragen
- Einrichten von CI-/CD-Pipelines

## <a name="export-a-job-to-a-local-project"></a>Exportieren eines Auftrags in ein lokales Projekt

Suchen Sie den zu exportierenden Auftrag im **Stream Analytics-Explorer** in Visual Studio Code, um einen Auftrag in ein lokales Projekt zu exportieren. Wählen Sie dann einen Ordner für Ihr Projekt aus. Das Projekt wird in den von Ihnen ausgewählten Ordner exportiert, und Sie können den Auftrag weiterhin über Visual Studio Code verwalten. Weitere Informationen zur Verwendung von Visual Studio Code zum Verwalten von Stream Analytics-Aufträgen finden Sie im Visual Studio Code-[Schnellstartanleitung](quick-create-vs-code.md).

![Exportieren von ASA-Aufträgen in Visual Studio Code](./media/vs-code-how-to/export-job.png)

## <a name="run-queries-locally"></a>Lokales Ausführen von Abfragen

Mithilfe von Azure Stream Analytics-Erweiterungen für Visual Studio Code können Sie Ihre Stream Analytics-Aufträge lokal mit Beispieldaten testen.

1. Nachdem Sie Ihren Stream Analytics-Auftrag erstellt haben, verwenden Sie **STRG+UMSCHALT+P** zum Öffnen der Befehlspalette. Geben Sie dann **ASA ein, und wählen Sie den Eintrag aus: Hinzufügen der Eingabe**.

    ![Hinzufügen von ASA-Eingaben in Visual Studio Code](./media/vs-code-how-to/add-input.png)

2. Wählen Sie **Local Input** (Lokale Eingabe) aus.

    ![Hinzufügen von lokalen ASA-Eingaben in Visual Studio Code](./media/vs-code-how-to/add-local-input.png)

3. Wählen Sie **+ New Local Input** (+ Neue lokale Eingabe) aus.

    ![Hinzufügen einer neuen lokalen ASA-Eingabe in Visual Studio Code](./media/vs-code-how-to/add-new-local-input.png)

4. Geben Sie die gleichen Eingabealias ein, die Sie in Ihrer Abfrage verwendet haben.

    ![Hinzufügen eines neuen lokalen ASA-Eingabealias](./media/vs-code-how-to/new-local-input-alias.png)

5. Geben Sie in der Datei **LocalInput_DefaultLocalStream.json** den Dateipfad ein, in dem sich Ihre lokale Datendatei befindet.

    ![Eingeben von lokalen Dateipfaden in Visual Studio](./media/vs-code-how-to/local-file-path.png)

6. Gehen Sie zurück zum Abfrage-Editor, und wählen Sie **Lokal ausführen** aus.

    ![Auswählen von „Lokal ausführen“ im Abfrage-Editor](./media/vs-code-how-to/run-locally.png)

## <a name="set-up-a-cicd-pipeline"></a>Einrichten von CI-/CD-Pipelines

Sie können Continuous Integration und Continuous Deployment für Azure Stream Analytics-Aufträge mithilfe des npm-Pakets **asa-cicd-Tools** aktivieren. Das npm-Paket enthält die Tools für die automatische Bereitstellung von Stream Analytics-Projekten in Visual Studio Code. Es kann ohne die Installation von Visual Studio Code unter Windows, macOS und Linux verwendet werden.

Nachdem Sie [das Paket heruntergeladen haben](https://usqldownload.blob.core.windows.net/ext/asa/asa-cicd-0.0.1-preview-beta.tar), verwenden Sie den folgenden Befehl, um die Azure Resource Manager-Vorlagen auszugeben. Wenn **outputPath** nicht angegeben ist, werden die Vorlagen in den Ordner **Bereitstellen** im **bin**-Ordner des Projekts platziert.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Azure Stream Analytics-Cloudauftrags in Visual Studio Code (Vorschauversion)](quick-create-vs-code.md)