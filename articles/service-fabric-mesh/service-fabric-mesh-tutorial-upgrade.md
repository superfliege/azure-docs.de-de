---
title: Tutorial – Aktualisieren einer Azure Service Fabric Mesh-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Service Fabric-Anwendungen mithilfe von Visual Studio aktualisieren.
services: service-fabric-mesh
documentationcenter: .net
author: tylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 7985c8e9e26126040d842ded998a953281daa2ae
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953551"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Tutorial: Erfahren Sie, wie Sie eine Service Fabric-Anwendungen mithilfe von Visual Studio aktualisieren.

Dieses Tutorial ist der vierte Teil einer Reihe und zeigt, wie Sie eine Azure Service Fabric Mesh-Anwendung direkt über Visual Studio aktualisieren. Das Upgrade beinhaltet sowohl ein Code- als auch ein Konfigurationsupdate. Sie werden sehen, dass die Schritte für das Aktualisieren und Veröffentlichen über Visual Studio gleich sind.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Aktualisieren eines Service Fabric-Dienstes mit Visual Studio

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer Service Fabric-App in Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Debuggen einer Service Fabric Mesh-App, die in Ihrem lokalen Entwicklungscluster ausgeführt wird](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Bereitstellen einer Service Fabric Mesh-App](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Aktualisieren einer Service Fabric Mesh-App
> * [Bereinigen von Service Fabric Mesh-Ressourcen](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie die To-Do-App noch nicht bereitgestellt haben, befolgen Sie die Anweisungen unter [Bereitstellen einer Service Fabric Mesh-Anwendung](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Aktualisieren eines Service Fabric-Dienstes mit Visual Studio

In diesem Artikel erfahren Sie, wie Sie Microservices unabhängig voneinander in einer Anwendung aktualisieren können.  In diesem Beispiel ändern wir den `WebFrontEnd`-Dienst, um eine Aufgabenkategorie anzuzeigen. Anschließend aktualisieren wir den bereitgestellten Dienst.

## <a name="modify-the-config"></a>Ändern der Konfiguration

Aktualisierungen können aufgrund von Codeänderungen, Konfigurationsänderungen oder beiden erforderlich sein.  Um eine Konfigurationsänderung vorzunehmen, öffnen Sie die `service.yaml`-Datei des Projekts `WebFrontEnd` (unter dem Knoten **Dienstressourcen**).

Ändern Sie im Abschnitt `resources:` den Wert für `cpu:` von 0.5 in 1.0, in der Erwartung, dass das Web-Front-End stark genutzt wird. Dies sollte jetzt wie folgt aussehen:

```xml
              ...
              resources:
                requests:
                  cpu: 1.0
                  memoryInGB: 1
              ...
```

## <a name="modify-the-model"></a>Ändern des Modells

Um eine Codeänderung vorzunehmen, fügen Sie eine `Category`-Eigenschaft zur `ToDoItem`-Klasse in der `ToDoItem.cs`-Datei hinzu.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Aktualisieren Sie dann die `Load()`-Methode in der gleichen Datei, um die Kategorie auf eine standardmäßige Zeichenfolge festzulegen:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>Ändern des Dienstes

Das Projekt `WebFrontEnd` ist eine ASP.NET Core-Anwendung mit einer Webseite, die To-Do-Listenelemente anzeigt. Öffnen Sie im Projekt `WebFrontEnd` `Index.cshtml` und fügen Sie die folgenden zwei Zeilen hinzu, um die Kategorie der Aufgabe anzuzeigen:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Erstellen und starten Sie die App, um sicherzustellen, dass eine neue Spalte „Kategorie“ auf der Webseite angezeigt wird, die die Aufgaben auflistet.

## <a name="upgrade-the-app-from-visual-studio"></a>Aktualisieren der App über Visual Studio

Unabhängig davon, ob Sie eine Code- oder eine Konfigurationsaktualisierung durchführen (in diesem Fall machen wir beides), um Ihre Service Fabric Mesh-App in Azure zu aktualisieren, klicken Sie mit der rechten Maustaste auf **todolistapp** in Visual Studio und wählen Sie **Veröffentlichen.....**.

Nun wird ein Dialogfeld **Service Fabric-Anwendung veröffentlichen** angezeigt.

![Dialogfeld „Veröffentlichen“ von Service Fabric Mesh in Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Wählen Sie Ihr Azure-Konto und -Abonnement aus. Stellen Sie sicher, dass für **Speicherort** der Speicherort eingestellt ist, den Sie verwendet haben, als Sie die To-Do-App ursprünglich für Azure veröffentlicht haben. In diesem Artikel wird **USA, Osten** verwendet.

Stellen Sie sicher, dass für **Ressourcengruppe** die Ressourcengruppe eingestellt ist, den Sie verwendet haben, als Sie die To-Do-App ursprünglich für Azure veröffentlicht haben.

Stellen Sie sicher, dass für **Azure Container Registry** die Azure Container Registry eingestellt ist, den Sie verwendet haben, als Sie die To-Do-App ursprünglich für Azure veröffentlicht haben.

Drücken Sie im Dialogfeld „Veröffentlichen“ auf die Schaltfläche **Veröffentlichen**, um die To-Do-App in Azure zu aktualisieren.

Sie können den Status der Aktualisierung überwachen, indem Sie den Bereich **Service Fabric-Tools** im Fenster **Ausgabe** von Visual Studio auswählen. Nach Abschluss der Aktualisierung zeigt die Ausgabe der **Service Fabric-Tools** die IP-Adresse und den Port Ihrer Anwendung in Form einer URL an.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der URL, um die Ausführung der Website in Azure anzuzeigen. Es sollte eine Webseite mit einer Spalte „Kategorie“ angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * So aktualisieren Sie eine Service Fabric Mesh-App mit Visual Studio

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Bereinigen von Service Fabric Mesh-Ressourcen](service-fabric-mesh-tutorial-cleanup-resources.md)