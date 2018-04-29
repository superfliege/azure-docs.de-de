---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d407b015699925a6a3ce6ef9108ace1e9900303f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
## <a name="set-up-your-project"></a>Einrichten des Projekts

In diesem Abschnitt erstellen Sie ein neues Projekt, um zu veranschaulichen, wie Sie eine Windows Desktop .NET-Anwendung (XAML) mit *Mit Microsoft anmelden* so integrieren können, dass Sie Web-APIs abfragen kann, die ein Token erfordern.

Die Anwendung, die Sie anhand dieser Anleitung erstellen, zeigt eine Schaltfläche zum Aufrufen eines Diagramms, einen Ergebnisbereich und eine Abmeldeschaltfläche an.

> [!NOTE]
> Möchten Sie stattdessen das Visual Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip), und fahren Sie mit dem [Konfigurationsschritt](#register-your-application) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.
>

Gehen Sie zum Erstellen der Anwendung wie folgt vor:
1. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
2. Wählen Sie unter **Vorlagen** die Option **Visual C#** aus.
3. Wählen Sie abhängig von der verwendeten Visual Studio-Version entweder **WPF-App** oder **WPF-Anwendung** aus.

## <a name="add-msal-to-your-project"></a>Hinzufügen von MSAL zu Ihrem Projekt
1. Klicken Sie in Visual Studio auf **Tools** > **NuGet-Paket-Manager**> **Paket-Manager-Konsole**.
2. Fügen Sie im Fenster „Paket-Manager-Konsole“ den folgenden Azure PowerShell-Befehl ein:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Dieser Befehl installiert die Microsoft-Authentifizierungsbibliothek. MSAL übernimmt die Erfassung, Zwischenspeicherung und Aktualisierung von Benutzertoken für den Zugriff auf die durch Azure Active Directory v2 geschützten APIs.
    >

## <a name="add-the-code-to-initialize-msal"></a>Hinzufügen des Codes zum Initialisieren der MSAL
In diesem Schritt erstellen Sie eine Klasse zur Handhabung der Interaktion mit MSAL (beispielsweise die Handhabung von Token).

1. Öffnen Sie die Datei *App.xaml.cs*, und fügen Sie der Klasse den Verweis für MSAL hinzu:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Aktualisieren Sie die App-Klasse wie folgt:

    ```csharp
    public partial class App : Application
    {
        //Below is the clientId of your app registration. 
        //You have to replace the below with the Application Id for your app registration
        private static string ClientId = "your_client_id_here";

        public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

    }
    ```

## <a name="create-the-application-ui"></a>Erstellen der Anwendungsbenutzeroberfläche

In diesem Abschnitt erfahren Sie, wie eine Anwendung einen geschützten Back-End-Server wie Microsoft Graph abfragen kann. 

Die Datei *MainWindow.xaml* sollte als Teil Ihrer Projektvorlage automatisch erstellt werden. Öffnen Sie diese Datei, und ersetzen Sie den Knoten *\<Grid>* Ihrer Anwendung durch den folgenden Code:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

