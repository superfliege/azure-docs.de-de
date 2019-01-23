---
title: Definieren und Verwenden von Inhaltsmoderationsworkflows – Content Moderator
titlesuffix: Azure Cognitive Services
description: Sie können den Workflow-Designer und die APIs von Azure Content Moderator verwenden, um benutzerdefinierte Workflows und Schwellenwerte basierend auf Ihren Inhaltsrichtlinien zu definieren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 265d5a2bd6a256d287e6ae164e883ade783f960f
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262403"
---
# <a name="define-test-and-use-workflows"></a>Definieren, Testen und Verwenden von Workflows

Sie können den Workflow-Designer und die APIs von Azure Content Moderator verwenden, um benutzerdefinierte Workflows und Schwellenwerte basierend auf Ihren Inhaltsrichtlinien zu definieren.

Die Workflows „verbinden“ sich über Konnektoren mit der Content Moderator-API. Sie können andere APIs verwenden, wenn ein Konnektor für diese API verfügbar ist. In diesem Beispiel verwendet wir den Content Moderator-Konnektor, der standardmäßig enthalten ist.

## <a name="browse-to-the-workflows-section"></a>Aufrufen des Abschnitts „Workflows“

Wählen Sie auf der Registerkarte **Einstellungen** die Option **Workflows**.

  ![Workflow-Einstellung](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Starten eines neuen Workflows

Wählen Sie **Workflow hinzufügen**.

  ![Workflow hinzufügen](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Zuweisen von Name und Beschreibung

Benennen Sie Ihren Workflow, geben Sie eine Beschreibung ein und wählen Sie, ob der Workflow Bilder oder Text behandelt.

  ![Workflowname und -beschreibung](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Definieren der Auswertungskriterien („Bedingung“)

Im folgenden Screenshot sehen Sie die Felder und die If-Then-Else-Auswahl, die Sie für Ihre Workflows definieren müssen. Wählen Sie einen Konnektor. Dieses Beispiel verwendet **Content Moderator**. Je nach gewähltem Konnektor ändern sich die verfügbaren Optionen für die Ausgabe.

  ![Workflowbedingung definieren](images/ocr-workflow-step-2-condition.PNG)

Nachdem Sie den gewünschten Konnektor und dessen Ausgabe ausgewählt haben, wählen Sie einen Operator und den Wert für die Bedingung aus.

## <a name="define-the-action-to-take"></a>Definieren der auszuführenden Aktion

Wählen Sie die zu ergreifende Aktion und die zu erfüllende Bedingung aus. Das folgende Beispiel erstellt eine Bildüberprüfung, weist einen Tag `a` zu und hebt ihn für die angezeigte Bedingung hervor. Sie können auch mehrere Bedingungen kombinieren, um die gewünschten Ergebnisse zu erhalten. Fügen Sie optional einen alternativen (Else) Pfad hinzu.

  ![Workflowaktion definieren](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>Speichern Ihres Workflows

Speichern Sie den Workflow abschließend, und notieren Sie den Workflownamen. Sie benötigen den Namen, um einen Moderationsauftrag über die Überprüfungs-API zu starten.

## <a name="test-the-workflow"></a>Testen des Workflows

Nachdem Sie nun einen benutzerdefinierten Workflow definiert haben, testen Sie ihn mit Beispielinhalten.

Wählen Sie die entsprechende Schaltfläche **Workflow ausführen**.

  ![Workflowtest](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Hochladen einer Datei

Speichern Sie das [Beispielbild](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) auf Ihrem lokalen Laufwerk. Um den Workflow zu testen, wählen Sie **Datei(en) auswählen** und laden Sie das Bild hoch.

  ![Bilddatei hochladen](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Workflow nachverfolgen

Sie können den Workflow während der Ausführung nachverfolgen.

  ![Workflowausführung nachverfolgen](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Überprüfen von für die Moderation durch Personen markierten Bildern

Um die Bildüberprüfung anzuzeigen, navigieren Sie zur Registerkarte **Bild** unter **Überprüfen**.

  ![Überprüfen von Bildern](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Nächste Schritte 

Um den Workflow vom Code aufzurufen, verwenden Sie benutzerdefinierte Workflows mit dem [ `Job`-API-Konsole Schnellstart](../try-review-api-job.md) und [.NET SDK-Schnellstart](../moderation-jobs-quickstart-dotnet.md).
