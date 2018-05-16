---
title: Erste Schritte mit verbundenen Key Vault-Diensten in Visual Studio (ASP.NET Core-Projekte) | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie einer ASP.NET- oder ASP.NET Core-Webanwendung Key Vault-Unterstützung hinzufügen.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 64d1a404eac955f47308f01edd56b3d008e250a0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-core-projects"></a>Erste Schritte mit verbundenen Key Vault-Diensten in Visual Studio (ASP.NET Core-Projekte)

> [!div class="op_single_selector"]
> - [Erste Schritte](vs-key-vault-aspnet-core-get-started.md)
> - [Was ist passiert?](vs-key-vault-aspnet-core-what-happened.md)

Dieser Artikel enthält weiterführende Informationen, nachdem Sie Key Vault in Visual Studio über den Befehl **Verbundene Dienste hinzufügen** einem ASP.NET Core-Projekt hinzugefügt haben. Wenn Sie den Dienst noch nicht zu Ihrem Projekt hinzugefügt haben, können Sie dies jederzeit nachholen, indem Sie die Anweisungen unter [Hinzufügen von Key Vault zu Ihrer Webanwendung mithilfe der Option „Verbundene Dienste“ in Visual Studio](vs-key-vault-add-connected-service.md) befolgen.

Informationen zu den Änderungen, die an Ihrem Projekt vorgenommen werden, wenn der verbundene Dienst hinzugefügt wird, finden Sie unter [Was ist mit dem ASP.NET Core-Projekt passiert?](vs-key-vault-aspnet-core-what-happened.md).

## <a name="after-you-connect"></a>Nach dem Herstellen der Verbindung

1. Fügen Sie Ihrem Schlüsseltresor in Azure ein Geheimnis hinzu. Um im Portal an die richtige Stelle zu gelangen, klicken Sie auf den Link für die Verwaltung von Geheimnissen, die in diesem Schlüsseltresor gespeichert sind. Wenn Sie die Seite oder das Projekt geschlossen haben, können Sie im [Azure-Portal](https://portal.azure.com) dorthin wechseln, indem Sie unter **Sicherheit** die Option **Alle Dienste**, dann **Key Vault** und schließlich den Schlüsseltresor auswählen, den Sie gerade erstellt haben.

   ![Navigieren zum Portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Wählen Sie im Abschnitt „Key Vault“ für den gerade erstellten Schlüsseltresor die Option **Geheimnisse** aus, und klicken Sie auf **Generieren/Importieren**.

   ![Generieren/Importieren eines Geheimnisses](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Geben Sie ein Geheimnis ein, z.B. „MeinGeheimn“, und weisen Sie diesem testweise einen beliebigen Zeichenfolgenwert zu. Klicken Sie anschließend auf die Schaltfläche **Erstellen**.

   ![Erstellen eines Geheimnisses](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (Optional) Geben Sie ein weiteres Geheimnis ein, ordnen Sie es dieses Mal jedoch einer Kategorie zu, indem Sie es **Geheimnisse – MeinGeheimnis** nennen. Diese Syntax gibt die Kategorie **Geheimnisse** an, die ein Geheimnis namens **MeinGeheimnis** enthält.
1. In Ihrem Visual Studio-Projekt können Sie jetzt auf diese Geheimnisse verweisen, indem Sie im Code die folgenden Ausdrücke verwenden:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

Herzlichen Glückwunsch! Sie haben dafür gesorgt, dass Ihre Web-App Key Vault für den Zugriff auf sicher gespeicherte Geheimnisse verwenden kann.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, wenn Sie sie nicht mehr benötigen. Dadurch werden der Schlüsseltresor und die zugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.

# <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Entwicklung mit Key Vault finden Sie im [Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).