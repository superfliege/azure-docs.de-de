---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 69dc0e1c14bc88cdbf0aa48700f95058ba759cc0
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140263"
---
1. Klicken Sie in der Projektmappenansicht (oder in Visual Studio im **Projektmappen-Explorer**) mit der rechten Maustaste auf den **Komponentenordner**, klicken Sie auf **Get More Components...** (Weitere Komponenten...), suchen Sie nach der Komponente **Google Cloud Messaging Client**, und fügen Sie sie dem Projekt hinzu.
2. Öffnen Sie die Projektdatei "ToDoActivity.cs", und fügen Sie die folgende "using"-Anweisung hinzu:

    ```csharp
    using Gcm.Client;
    ```

3. Fügen Sie der **ToDoActivity** -Klasse den folgenden neuen Code hinzu: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Dies ermöglicht Ihnen den Zugriff auf die mobile Clientinstanz vom Pushhandler-Dienstprozess aus.
4. Fügen Sie der **OnCreate**-Methode den folgenden Code hinzu, nachdem **MobileServiceClient** erstellt wurde:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

Die **ToDoActivity** ist damit für das Hinzufügen von Pushbenachrichtigungen vorbereitet.
