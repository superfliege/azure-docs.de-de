---
title: Konfigurieren von Postman für Azure Media Services-REST-API-Aufrufe
description: Erfahren Sie, wie Sie Postman für Azure Media Services-REST-API-Aufrufe konfigurieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.author: juliako
ms.openlocfilehash: b110a57526d6e23ba53afce0a91d735fad6d247c
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215888"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Konfigurieren von Postman für Media Services-REST-API-Aufrufe

In diesem Artikel wird veranschaulicht, wie Sie **Postman** so konfigurieren, dass das Tool für den Aufruf von Azure Media Services-REST-APIs verwendet werden kann. In diesem Artikel wird veranschaulicht, wie Umgebungs- und Sammlungsdateien in **Postman** importiert werden. Die Sammlung enthält gruppierte Definitionen von HTTP-Anforderungen, die Azure Media Services-REST-APIs aufrufen. Die Umgebungsdatei enthält Variablen, die von der Sammlung verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md) Merken Sie sich den Namen der Ressourcengruppe und den Namen des Media Services-Kontos. 
- Beschaffen Sie die Informationen, die für den [Zugriff auf APIs](access-api-cli-how-to.md) benötigt werden.
- Installieren Sie den REST-Client von [Postman](https://www.getpostman.com/), um die REST-APIs auszuführen, die in einigen der AMS REST-Tutorials gezeigt werden. 

    Wir verwenden **Postman**, aber jedes REST-Tool wäre geeignet. Andere Alternativen sind: **Visual Studio Code** mit dem REST-Plug-In oder **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Herunterladen von Postman-Dateien

Klonen Sie ein GitHub-Repository mit der Postman-Sammlung und den Umgebungsdateien.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Konfigurieren von Postman

In diesem Abschnitt wird Postman konfiguriert.

### <a name="configure-the-environment"></a>Konfigurieren der Umgebung 

1. Öffnen Sie **Postman**.
2. Wählen Sie auf der rechten Seite des Bildschirms die Option **Manage environment** (Umgebung verwalten) aus.

    ![Verwalten der Umgebung](./media/develop-with-postman/postman-import-env.png)
4. Klicken Sie im Dialogfeld **Manage environment** (Umgebung verwalten) auf **Import**.
2. Navigieren Sie zur Datei `Azure Media Service v3 Environment.postman_environment.json`, die beim Klonen von `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` heruntergeladen wurde.
6. Die Umgebung **Azure Media Service v3 Environment** wird hinzugefügt.

    > [!Note]
    > Aktualisieren Sie die Zugriffsvariablen mit Werten aus dem Abschnitt **Zugreifen auf die Media Services-API**.

7. Doppelklicken Sie auf die ausgewählte Datei, und geben Sie Werte ein, die Sie durch Ausführen der Schritte für den [Zugriff auf die API](#access-the-media-services-api) erhalten.
8. Schließen Sie das Dialogfeld.
9. Wählen Sie in der Dropdownliste die Umgebung **Azure Media Service v3 Environment** aus.

    ![Auswählen der Umgebung](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Konfigurieren der Sammlung

1. Klicken Sie auf **Import**, um die Sammlungsdatei zu importieren.
1. Navigieren Sie zur Datei `Media Services v3.postman_collection.json`, die beim Klonen von `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` heruntergeladen wurde.
3. Wählen Sie die Datei **Media Services v3.postman_collection.json** aus.

    ![Importieren einer Datei](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Abrufen des Azure AD-Tokens 

Bevor Sie mit dem Ändern von AMS v3-Ressourcen beginnen, müssen Sie das Azure AD-Token für die Dienstprinzipalauthentifizierung abrufen und festlegen.

1. Klicken Sie im linken Fenster von Postman auf „Step 1: Get AAD Auth token“ (Schritt 1: AAD-Authentifizierungstoken abrufen).
2. Klicken Sie anschließend auf „Get Azure AD Token for Service Principal Authentication“ (Azure AD-Token für Dienstprinzipalauthentifizierung abrufen).
3. Klicken Sie auf **Senden**.

    Der folgende Vorgang vom Typ **POST** wird gesendet:

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Die zurückgegebene Antwort enthält das Token und legt die Umgebungsvariable „AccessToken“ auf den Tokenwert fest. Klicken Sie zum Anzeigen des Codes, der „AccessToken“ festlegt, auf die Registerkarte **Tests**. 

    ![Abrufen des AAD-Tokens](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Hochladen, Codieren und Streamen von Videos mit REST](stream-files-tutorial-with-rest.md).  
