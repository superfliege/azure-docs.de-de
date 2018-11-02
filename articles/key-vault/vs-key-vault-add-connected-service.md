---
title: Hinzufügen von Key Vault-Unterstützung zu Ihrem ASP.NET-Projekt mit Visual Studio | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie einer ASP.NET- oder ASP.NET Core-Webanwendung Key Vault-Unterstützung hinzufügen.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: c90ef26c0170db67b1d422701b6969ca3f9c9e38
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958515"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Hinzufügen von Key Vault zu Ihrer Webanwendung mithilfe der Option „Verbundene Dienste“ in Visual Studio

In diesem Tutorial erfahren Sie, wie Sie auf einfache Weise alles hinzufügen, was Sie zum Verwalten Ihrer Geheimnisse mit Azure Key Vault für Webprojekte in Visual Studio benötigen – ganz unabhängig davon, ob Sie ASP.NET Core oder einen beliebigen Typ von ASP.NET-Projekt verwenden. Mit dem Feature „Verbundene Dienste“ in Visual Studio 2017 werden alle NuGet-Pakete und Konfigurationseinstellungen, die für eine Verbindung mit Key Vault in Azure erforderlich sind, von Visual Studio automatisch hinzugefügt. 

Ausführliche Informationen zu den Änderungen, die durch verbundene Dienste in Ihrem Projekt durchgeführt werden, um Key Vault zu aktivieren, finden Sie unter [Verbundener Key Vault-Dienst – Auswirkungen auf mein ASP.NET 4.7.1-Projekt](vs-key-vault-aspnet-what-happened.md) bzw. unter [Verbundener Key Vault-Dienst – Auswirkungen auf mein ASP.NET Core-Projekt](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Voraussetzungen

- **Ein Azure-Abonnement**. Falls Sie über kein Abonnement verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)registrieren.
- **Visual Studio 2017, Version 15.7** mit installierter Workload **Webentwicklung**. [Jetzt herunterladen](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Für ASP.NET (nicht Core) benötigen Sie die .NET Framework 4.7.1-Entwicklungstools, die nicht standardmäßig installiert werden. Um sie zu installieren, starten Sie den Installer für Visual Studio, wählen Sie **Ändern** und dann **Einzelne Komponenten**, erweitern Sie auf der rechten Seite den Eintrag **ASP.NET und Webentwicklung**, und wählen Sie **.NET Framework 4.7.1-Entwicklungstools** aus.
- Ein ASP.NET 4.7.1- oder ein ASP.NET Core 2.0-Webprojekt wird geöffnet.

## <a name="add-key-vault-support-to-your-project"></a>Hinzufügen von Key Vault-Unterstützung zu Ihrem Projekt

1. Wählen Sie im **Projektmappen-Explorer** die Option **Hinzufügen** > **Verbundener Dienst** aus.
   Die Seite „Verbundener Dienst“ wird mit den Diensten angezeigt, die Sie dem Projekt hinzufügen können.
1. Wählen Sie im Menü der verfügbaren Dienste **Schutz für Geheimnisse mit Azure Key Vault**.

   ![Auswahl von „Schutz für Geheimnisse mit Azure Key Vault“](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Wenn Sie sich bei Visual Studio angemeldet haben und Ihrem Konto ein Azure-Abonnement zugeordnet ist, wird eine Seite mit einer Dropdownliste mit Ihren Abonnements angezeigt. Stellen Sie sicher, dass Sie bei Visual Studio angemeldet sind und dass das Konto, mit dem Sie angemeldet sind, dasselbe Konto ist, das Sie für Ihr Azure-Abonnement verwenden.

1. Wählen Sie das Abonnement, das Sie verwenden möchten, und wählen Sie dann einen neuen oder vorhandenen Schlüsseltresor aus, bzw. klicken Sie auf den Link „Bearbeiten“, um den automatisch generierten Namen zu ändern.

   ![Wählen Sie Ihr Abonnement aus.](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Geben Sie den Namen ein, den Sie für den Schlüsseltresor verwenden möchten.

   ![Benennen Sie den Schlüsseltresor um, und wählen Sie eine Ressourcengruppe aus.](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue mit einem automatisch generierten eindeutigen Namen.  Wenn Sie eine neue Gruppe mit einem anderen Namen erstellen möchten, können Sie die Seite im [Azure-Portal](https://portal.azure.com) schließen und neu starten, um die Liste der Ressourcengruppen erneut zu laden.
1. Wählen Sie die Region aus, in der Sie den Schlüsseltresor erstellen möchten. Wenn Ihre Webanwendung in Azure gehostet wird, wählen Sie für eine optimale Leistung die Region aus, in der die Webanwendung gehostet wird.
1. Wählen Sie einen Tarif aus. Weitere Informationen finden Sie unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/).
1. Wählen Sie „OK“, um die Konfigurationsoptionen zu akzeptieren.
1. Wählen Sie **Hinzufügen** um den Schlüsseltresor zu erstellen. Beim Erstellungsvorgang kommt es möglicherweise zu einem Fehler, wenn Sie einen Namen auswählen, der bereits verwendet wurde.  Verwenden Sie in diesem Fall den Link **Bearbeiten** zum Umbenennen des Schlüsseltresors, und versuchen Sie es erneut.

   ![Hinzufügen eines verbundenen Diensts zum Projekt](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Fügen Sie Ihrem Schlüsseltresor in Azure jetzt ein Geheimnis hinzu. Um im Portal an die richtige Stelle zu gelangen, klicken Sie auf den Link für die Verwaltung von Geheimnissen, die in diesem Schlüsseltresor gespeichert sind. Wenn Sie die Seite oder das Projekt geschlossen haben, können Sie im [Azure-Portal](https://portal.azure.com) dorthin wechseln, indem Sie unter **Sicherheit** die Option **Alle Dienste**, dann **Key Vault** und schließlich den Schlüsseltresor auswählen, den Sie gerade erstellt haben.

   ![Navigieren zum Portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Wählen Sie im Abschnitt „Key Vault“ für den gerade erstellten Schlüsseltresor die Option **Geheimnisse** aus, und klicken Sie auf **Generieren/Importieren**.

   ![Generieren/Importieren eines Geheimnisses](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Geben Sie ein Geheimnis ein, z.B. „MeinGeheimn“, und weisen Sie diesem testweise einen beliebigen Zeichenfolgenwert zu. Klicken Sie anschließend auf die Schaltfläche **Erstellen**.

   ![Erstellen eines Geheimnisses](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (Optional) Geben Sie ein weiteres Geheimnis ein, ordnen Sie es dieses Mal jedoch einer Kategorie zu, indem Sie es mit „Geheimnisse--MeinGeheimnis“ benennen. Durch diese Syntax wird eine Kategorie „Geheimnisse“ angegeben, die ein Geheimnis namens „MeinGeheimnis“ enthält.
 
Jetzt können Sie in Code auf Ihre Geheimnisse zugreifen. Die nächsten Schritte unterscheiden sich abhängig davon, ob Sie ASP.NET 4.7.1 oder ASP.NET Core verwenden.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Zugreifen auf Ihre Geheimnisse in Code (ASP.NET Core-Projekte)

Die Verbindung zu Key Vault wird beim Systemstart von einer Klasse eingerichtet, die über eine gewisse Erweiterung des Startverhaltens (beschrieben in [Optimieren einer App von einem externen Assembly in ASP.NET Core mit IHostingStartup](/aspnet/core/fundamentals/host/platform-specific-configuration)) [Microsoft.AspNetCore.Hosting.IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) implementiert. Die Startup-Klasse verwendet zwei Umgebungsvariablen, welche die Verbindungsinformationen für Key Vault enthalten: ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED = „true“ und ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT, eingestellt auf die Key Vault-URL. Diese Umgebungsvariablen werden beim Ausführen des Prozesses **Verbundenen Dienst hinzufügen** der Datei „launchsettings.json“ hinzugefügt.

So greifen Sie auf Ihre Geheimnisse zu

1. In Visual Studio können Sie in Ihrem ASP.NET Core-Projekt jetzt auf diese Geheimnisse verweisen, indem Sie im Code die folgenden Ausdrücke verwenden:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. Fügen Sie auf einer CSHTML-Seite, beispielsweise „About.cshtml“, im oberen Bereich der Datei die Anweisung @inject hinzu, um eine Variable einzurichten, mit der Sie auf die Key Vault-Konfiguration zugreifen können.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Durch einen Test können Sie sicherstellen, dass der Wert des Geheimnisses verfügbar ist, indem Sie ihn auf einer der Seiten anzeigen. Verwenden Sie @config zum Verweisen auf die Konfigurationsvariable.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Erstellen Sie die Webanwendung, und führen Sie sie aus. Navigieren Sie zur About-Seite, und sehen Sie sich den Geheimniswert an.

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Zugreifen auf Ihre Geheimnisse in Code (ASP.NET 4.7.1-Projekte)

Die Verbindung zu Key Vault wird mithilfe von Informationen, die beim Ausführen des Prozesses **Verbundenen Dienst hinzufügen** der Datei „web.config“ hinzugefügt wurden, von der ConfigurationBuilder-Klasse eingerichtet.

So greifen Sie auf Ihre Geheimnisse zu

1. Nehmen Sie in der Datei „Web.config“ die folgenden Änderungen vor. Die Schlüssel sind Platzhalter, die vom AzureKeyVault ConfigurationBuilder durch die Werte von Geheimnissen in Key Vault ersetzt werden.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. Fügen Sie im HomeController in der Controllermethode „About“ die folgenden Zeilen hinzu, um das Geheimnis abzurufen und im ViewBag-Objekt zu speichern.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. Fügen Sie in der Ansicht „About.cshtml“ Folgendes hinzu, um den Wert des Geheimnisses anzuzeigen (nur zu Testzwecken).

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

1. Führen Sie Ihre App lokal aus, um zu überprüfen, ob Sie anstelle des Dummywerts aus der Konfigurationsdatei den Geheimniswert, den Sie im Azure-Portal eingegeben haben, lesen können.

Veröffentlichen Sie Ihre App anschließend in Azure.

## <a name="publish-to-azure-app-service"></a>Veröffentlichen in Azure App Service

1. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Veröffentlichen** aus. Sie werden auf einem Bildschirm aufgefordert, ein **Veröffentlichungsziel auszuwählen**. Wählen Sie auf der linken Seite **App Service** aus, und klicken Sie dann auf **Neu erstellen**.

   ![Veröffentlichen in App Service](media/vs-key-vault-add-connected-service/AppServicePublish1.PNG)

1. Stellen Sie auf dem Bildschirm **App Service erstellen** sicher, dass das Abonnement und die Ressourcengruppe die gleichen Elemente sind, in denen Sie den Schlüsseltresor erstellt haben, und klicken Sie dann auf **Erstellen**.

   ![Erstellen eines App Service](media/vs-key-vault-add-connected-service/AppServicePublish2.PNG)

1. Nachdem Ihre Webanwendung erstellt wurde, wird der Bildschirm **Veröffentlichen** angezeigt. Notieren Sie sich die URL für Ihre veröffentlichte, in Azure gehostete Webanwendung. Wenn neben **Schlüsseltresor** der Wert **Kein** angezeigt wird, müssen Sie App Service noch mitteilen, mit welchem Schlüsseltresor eine Verbindung hergestellt werden soll. Klicken Sie auf den Link **Schlüsseltresor hinzufügen**, und wählen Sie den erstellten Schlüsseltresor aus.

   ![Schlüsseltresor hinzufügen](media/vs-key-vault-add-connected-service/AppServicePublish3.PNG)

   Wenn die Option **Schlüsseltresor verwalten** angezeigt wird, können Sie darauf klicken, um die aktuellen Einstellungen anzuzeigen, Berechtigungen zu bearbeiten oder Ihre Geheimnisse im Azure-Portal zu ändern.

1. Wählen Sie als Nächstes den Link mit der Website-URL aus, um Ihre Webanwendung im Browser zu öffnen. Überprüfen Sie, ob Sie den richtigen Wert aus dem Schlüsseltresor sehen.

Herzlichen Glückwunsch, Sie haben sichergestellt, dass Ihre Web-App Key Vault für den Zugriff auf sicher gespeicherte Geheimnisse verwenden kann, wenn sie in Azure ausgeführt wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, wenn Sie sie nicht mehr benötigen. Dadurch werden der Schlüsseltresor und die zugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Key Vault-Entwicklung finden Sie im [Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).