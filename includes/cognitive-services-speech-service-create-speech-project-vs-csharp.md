---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 8ff8e8341b6f39f66c2cc8014d41d3d3a2918d2b
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454467"
---
1. Starten Sie Visual Studio 2017.
 
1. Stellen Sie sicher, dass die Workload **.NET-Desktopumgebung** verfügbar ist. Wählen Sie **Tools** \> **Tools und Features abrufen** in der Visual Studio-Menüleiste aus, um den Visual Studio-Installer zu öffnen. Wenn diese Workload bereits aktiviert ist, schließen Sie das Dialogfeld. 

    Aktivieren Sie andernfalls das Kontrollkästchen neben **.NET-Desktopentwicklung,** , und klicken Sie dann auf die Schaltfläche **Ändern** in der unteren rechten Ecke des Dialogfelds. Die Installation des neuen Features nimmt etwas Zeit in Anspruch.

    ![Aktivieren der .NET-Desktopentwicklung](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Erstellen Sie eine neue Visual C#-Konsolenanwendung. Erweitern Sie im Dialogfeld links **Neues Projekt** **Installiert** \> **Visual C#** \> **Windows Desktop**, und wählen Sie dann **Konsolen-App (.NET Framework)** aus. Geben Sie als Projektnamen *helloworld* ein.

    ![Erstellen einer Visual C#-Konsolen-App (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Erstellen einer Visual C#-Konsolen-App (.NET Framework)")

1. Installieren Sie das [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget). Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

    ![Klicken Sie mit der rechten Maustaste auf „NuGet-Pakete für Projektmappen verwalten“](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "NuGet-Pakete für Projektmappe verwalten")

1. Wählen Sie rechts oben im Feld **Paketquelle** die Option **Nuget.org** aus. Suchen Sie nach dem Paket `Microsoft.CognitiveServices.Speech`, und installieren Sie es im Projekt **helloworld**.

    ![NuGet-Paket Microsoft.CognitiveServices.Speech installieren](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "NuGet-Paket installieren")

1. Akzeptieren Sie die angezeigten Lizenzbedingungen, um die Installation des NuGet-Pakets zu starten.

    ![Akzeptieren der Lizenzbedingungen](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Akzeptieren der Lizenzbedingungen")

    Nachdem das Paket installiert wurde, wird eine Bestätigung in der Paket-Manager-Konsole angezeigt.

1. Erstellen Sie über den Konfigurations-Manager eine Plattformkonfiguration, die Ihrer Computerarchitektur entspricht. Wählen Sie **Build** > **Konfigurations-Manager** aus.

    ![Starten des Konfigurations-Managers](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Starten des Konfigurations-Managers")

1. Fügen Sie im Dialogfeld **Konfigurations-Manager** eine neue Plattform hinzu. Wählen Sie in der Dropdownliste **Aktive Projektmappenplattform** den Eintrag **Neu** aus.

    ![Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Hinzufügen einer neuen Plattform im Fenster des Konfigurations-Managers")

1. Wenn Sie Windows 64-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x64`. Wenn Sie Windows 32-Bit ausführen, erstellen Sie eine neue Plattformkonfiguration mit dem Namen `x86`.

    ![Fügen Sie unter Windows 64-Bit eine neue Plattform mit dem Namen „x64“ hinzu](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "x64-Plattform hinzufügen")


