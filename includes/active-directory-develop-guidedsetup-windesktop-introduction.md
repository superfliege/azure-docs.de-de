# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Aufrufen der Microsoft Graph-API aus einer Windows Desktop-App

Diese Anleitung veranschaulicht, wie eine native Windows Desktop .NET (XAML)-Anwendung ein Zugriffstoken abrufen und die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt anfordern.

Am Ende dieser Anleitung kann Ihre Anwendung eine geschützte API sowohl mit persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen aufrufen, die mit Azure Active Directory arbeiten.  

> Für diese Anleitung ist Visual Studio 2015 Update 3 oder Visual Studio 2017 erforderlich.  Sie haben beides nicht? [Laden Sie Visual Studio 2017 kostenlos herunter](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Die in dieser Anleitung erstellte Beispielanwendung ermöglicht einer Windows Desktop-Anwendung das Abfragen einer Microsoft Graph-API oder Web-API, die Token von einem Azure Active Directory-v2-Endpunkt akzeptiert. In diesem Szenario wird ein Token über den Autorisierungsheader HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Verarbeiten des Beziehens von Token für den Zugriff auf geschützte Web-APIs

Nach der Authentifizierung des Benutzers empfängt die Beispielanwendung ein Token, mit dem eine Microsoft Graph-API oder Web-API abgefragt werden kann, die von Microsoft Azure Active Directory v2 geschützt wird.

APIs wie Microsoft Graph erfordern ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen, z.B. zum Lesen eines Benutzerprofils, Zugreifen auf den Kalender eines Benutzers oder Senden einer E-Mail. Ihre Anwendung kann ein Zugriffstoken mithilfe der MSAL anfordern, um auf diese Ressourcen durch Angeben von API-Bereichen zuzugreifen. Dieses Zugriffstoken wird dann dem HTTP-Autorisierungsheader für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt. 

Die MSAL verwaltet das Zwischenspeichern und Aktualisieren von Zugriffstoken, damit Ihre Anwendung dies nicht übernehmen muss.


### <a name="nuget-packages"></a>NuGet-Pakete

In dieser Anleitung werden die folgenden NuGet-Pakete verwendet:

|Bibliothek|Beschreibung|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|

