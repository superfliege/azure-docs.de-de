---
title: Azure AD v2 Windows Desktop erste Schritte - Setup | Microsoft Docs
description: "Wie Windows Desktop .NET (XAML)-Anwendungen eine API, die erfordern Zugriffstoken von Azure Active Directory-v2-Endpunkt aufrufen können"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4065727aef04d7969d438c6ef79127bb44568be1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a>Richten Ihres Projekts ein

Dieser Abschnitt enthält schrittweise Anleitungen zum Erstellen eines neuen Projekts veranschaulicht, wie Sie die Integration einer .NET für Windows-Desktop-Anwendung (XAML) mit *melden Sie sich mit Microsoft* damit Web-APIs abgefragt werden kann, die ein Token benötigt.

Die Anwendung erstellt wird, wird in diesem Handbuch stellt eine Schaltfläche, um das Diagramm und Ergebnisse auf dem Bildschirm und eine Abmeldeschaltfläche anzeigen.

> Möchten Sie stattdessen Visual Studio-Projekt für dieses Beispiel herunterladen? [Herunterladen ein Projekts](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) und fahren Sie mit der [Konfigurationsschritt](#create-an-application-express) im Codebeispiel wird vor der Ausführung zu konfigurieren.


### <a name="create-your-application"></a>Erstellen Sie die Anwendung
1. In Visual Studio:`File` > `New` > `Project`<br/>
2. Klicken Sie unter *Vorlagen*wählen`Visual C#`
3. Wählen Sie `WPF App` (oder *WPF-Anwendung* je nach Ihrer Visual Studio-Version)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Die Microsoft Authentication Library (MSAL) zu Ihrem Projekt hinzufügen
1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Einfügen Sie kopieren und im Fenster Paket-Manager-Konsole die folgenden:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Das obige installiert die Microsoft Authentication Library (MSAL). MSAL handles erfassen, Zwischenspeichern und Aktualisieren von Benutzer Toskens verwendet, um APIs von Azure Active Directory v2 geschützt zuzugreifen.

## <a name="add-the-code-to-initialize-msal"></a>Fügen Sie den Code zum Initialisieren der MSAL
Dieser Schritt hilft Ihnen die erstellen Sie einer Klasse, um die Interaktion mit MSAL-Bibliothek, z. B. Behandlung von Token zu behandeln.

1. Öffnen der `App.xaml.cs` Datei, und fügen Sie den Verweis für MSAL Bibliothek der Klasse:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aktualisieren Sie die App-Klasse, die die folgenden:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>Erstellen Sie Ihre Anwendung-Benutzeroberfläche
Der folgende Abschnitt wird gezeigt, wie eine Anwendung auf einen geschützten Back-End-Server wie Microsoft Graph Abfragen kann. Eine Datei "MainWindow.xaml" sollte als Teil der Projektvorlage automatisch erstellt werden. Diese Datei wird diese Datei öffnen, und klicken Sie dann die Anweisungen unten befolgen:

Ersetzen Sie der Anwendungsverzeichnis `<Grid>` mit werden die folgenden:

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
