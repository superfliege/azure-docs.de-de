---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 563c2bd561328561d30acee6910b70d53ef64c6b
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305131"
---
## <a name="set-up-your-device"></a>Einrichten des Geräts

### <a name="set-up-an-android-device"></a>Einrichten eines Android-Geräts

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Einrichten eines iOS-Geräts

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Geben Sie im Bereich **Inspector** die `Sharing Anchors Service url` (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) als Wert für `Base Sharing Url` ein, und ersetzen Sie `index.html` durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

Speichern Sie die Szene, indem Sie **Datei** > **Speichern** wählen.

## <a name="to-deploy-the-app-to-an-android-device"></a>So stellen Sie die App auf einem Android-Gerät bereit

Melden Sie sich auf Ihrem Android-Gerät an, und verbinden Sie es mit einem USB-Kabel mit Ihrem Computer.

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

Stellen Sie sicher, dass für **Projekt exportieren** kein Häkchen gesetzt ist. Wählen Sie **Erstellen und ausführen** aus. Sie werden aufgefordert, die Datei vom Typ `.apk` zu speichern. Sie können einen beliebigen Namen auswählen.

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) auswählen. Mit der ersten Option können Sie einen Anker erstellen, der später auf demselben oder einem anderen Gerät gesucht werden kann. Bei der zweiten Option können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App bereits entweder auf demselben oder einem anderen Gerät ausgeführt haben.

## <a name="to-deploy-the-app-to-an-ios-device"></a>So stellen Sie die App auf einem iOS-Gerät bereit

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) auswählen. Mit der ersten Option können Sie einen Anker erstellen, der später auf demselben oder einem anderen Gerät gesucht werden kann. Bei der zweiten Option können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App bereits entweder auf demselben oder einem anderen Gerät ausgeführt haben.

Beenden Sie die App in Xcode, indem Sie **Beenden** auswählen.
