---
title: 'Hinzufügen von Key Vault-Unterstützung zu Ihrem ASP.NET-Projekt mit Visual Studio: Azure Key Vault | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie einer ASP.NET- oder ASP.NET Core-Webanwendung Key Vault-Unterstützung hinzufügen.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ghogen
ms.openlocfilehash: de849ae290228826ee500ae1c7e623210e585d34
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58113247"
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

1. Fügen Sie Ihrem Schlüsseltresor in Azure jetzt ein Geheimnis hinzu. Um im Portal an die richtige Stelle zu gelangen, klicken Sie auf den Link für die Verwaltung von Geheimnissen, die in diesem Schlüsseltresor gespeichert sind. Wenn Sie die Seite oder das Projekt geschlossen haben, können Sie im [Azure-Portal](https://portal.azure.com) dorthin wechseln, indem Sie unter **Sicherheit** die Option **Alle Dienste**, dann **Key Vault** und schließlich den erstellten Schlüsseltresor auswählen.

   ![Navigieren zum Portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Wählen Sie im Abschnitt „Key Vault“ für den gerade erstellten Schlüsseltresor die Option **Geheimnisse** aus, und klicken Sie auf **Generieren/Importieren**.

   ![Generieren/Importieren eines Geheimnisses](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Geben Sie ein Geheimnis ein, z.B. „MeinGeheimnis“, und weisen Sie diesem testweise einen beliebigen Zeichenfolgenwert zu. Wählen Sie anschließend die Schaltfläche **Erstellen** aus.

   ![Erstellen eines Geheimnisses](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (Optional) Geben Sie ein weiteres Geheimnis ein, ordnen Sie es dieses Mal jedoch einer Kategorie zu, indem Sie es mit „Geheimnisse--MeinGeheimnis“ benennen. Durch diese Syntax wird eine Kategorie „Geheimnisse“ angegeben, die ein Geheimnis namens „MeinGeheimnis“ enthält.
 
Jetzt können Sie in Code auf Ihre Geheimnisse zugreifen. Die nächsten Schritte unterscheiden sich abhängig davon, ob Sie ASP.NET 4.7.1 oder ASP.NET Core verwenden.

## <a name="access-your-secrets-in-code"></a>Zugreifen auf Ihre Geheimnisse im Code

1. Installieren Sie die beiden NuGet-Bibliotheken [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) und [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

2. Öffnen Sie die Datei „Program.cs“, und aktualisieren Sie den Code mit folgendem Code: 
   ```
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
   ```
3. Öffnen Sie als Nächstes die Datei „About.cshtml.cs“, und schreiben Sie den folgenden Code:
   1. Fügen Sie einen Verweis auf Microsoft.Extensions.Configuration mit dieser using-Anweisung ein.    
       ```
       using Microsoft.Extensions.Configuration
       ```
   2. Fügen Sie diesen Konstruktor hinzu.
       ```
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```
   3. Aktualisieren Sie die OnGet-Methode. Ersetzen Sie den hier gezeigten Platzhalterwert durch den geheimen Namen, den Sie mit den oben angegebenen Befehlen erstellt haben.
       ```
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Führen Sie die App lokal aus, indem Sie zur Infoseite navigieren. Sie sollten Ihren Geheimniswert abgerufen haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, wenn Sie sie nicht mehr benötigen. Dadurch werden der Schlüsseltresor und die zugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Key Vault-Entwicklung finden Sie im [Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).