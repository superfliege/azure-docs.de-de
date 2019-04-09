---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 397a8a9b07b4d7a88d0345399ac4abcc3e738a82
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631186"
---
## <a name="set-up-your-device"></a>Einrichten des Geräts

Öffnen Sie in Unity das Projekt im Ordner `Unity`.

![Unity-Fenster](./media/spatial-anchors-unity/unity-window.png)

### <a name="set-up-an-android-device"></a>Einrichten eines Android-Geräts

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Einrichten eines iOS-Geräts

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Geben Sie im Bereich **Inspector** die `Sharing Anchors Service url` (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) als Wert für `Base Sharing Url` ein, und ersetzen Sie `index.html` durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

Speichern Sie die Szene, indem Sie **Datei** > **Speichern** wählen.

## <a name="deploy-to-your-device"></a>Bereitstellen auf Ihrem Gerät

### <a name="deploy-to-android-device"></a>Bereitstellen auf einem Android-Gerät

Melden Sie sich auf Ihrem Android-Gerät an, und verbinden Sie es mit einem USB-Kabel mit Ihrem Computer.

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

Stellen Sie sicher, dass für **Projekt exportieren** kein Häkchen gesetzt ist. Wählen Sie **Erstellen und ausführen** aus. Sie werden aufgefordert, die Datei vom Typ `.apk` zu speichern. Sie können einen beliebigen Namen auswählen.

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) auswählen. Im ersten Szenario können Sie einen Anker erstellen, der später auf dem gleichen oder auf einem anderen Gerät gefunden werden kann. Im zweiten Szenario können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App bereits entweder auf dem gleichen oder einem anderen Gerät ausgeführt haben. Nachdem Sie sich für ein Szenario entschieden haben, führt Sie die App durch die erforderlichen Schritte. So werden Sie beispielsweise dazu aufgefordert, auf Ihrem Gerät zu navigieren, um Umgebungsinformationen zu sammeln. Später platzieren Sie dann unter anderem einen Anker und warten, bis er hochgeladen wurde.

### <a name="deploy-to-an-ios-device"></a>Bereitstellen auf einem iOS-Gerät

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) auswählen. Im ersten Szenario können Sie einen Anker erstellen, der später auf dem gleichen oder auf einem anderen Gerät gefunden werden kann. Im zweiten Szenario können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App bereits entweder auf dem gleichen oder einem anderen Gerät ausgeführt haben. Nachdem Sie sich für ein Szenario entschieden haben, führt Sie die App durch die erforderlichen Schritte. So werden Sie beispielsweise dazu aufgefordert, auf Ihrem Gerät zu navigieren, um Umgebungsinformationen zu sammeln. Später platzieren Sie dann unter anderem einen Anker und warten, bis er hochgeladen wurde.

Beenden Sie die App in Xcode, indem Sie **Beenden** auswählen.
