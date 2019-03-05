---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 5f1e0153b1f919bc9d7e921d2a1b3ae745b2b01f
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753473"
---
## <a name="open-the-sample-project-in-unity"></a>Öffnen des Beispielprojekts in Unity

[!INCLUDE [Clone Sample Repo](spatial-anchors-clone-sample-repository.md)]

## <a name="to-set-up-for-an-android-device"></a>Durchführen der Einrichtung für ein Android-Gerät

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

## <a name="to-set-up-for-an-ios-device"></a>Durchführen der Einrichtung für ein iOS-Gerät

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Geben Sie ebenfalls im Bereich **Inspector** die `Sharing Anchors Service url` (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) als Wert für `Base Sharing Url` ein, und ersetzen Sie `index.html` durch `api/anchors`. Diese sollte also wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

Speichern Sie die Szene, indem Sie **Datei** -> **Speichern** wählen.

## <a name="to-deploy-to-an-android-device"></a>Durchführen der Bereitstellung auf einem Android-Gerät

Schalten Sie das Android-Gerät ein, melden Sie sich an, und stellen Sie dafür per USB-Kabel eine Verbindung mit dem PC her.

Öffnen Sie **Buildeinstellungen**, indem Sie **Datei** -> **Buildeinstellungen** wählen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

Stellen Sie sicher, dass das Kontrollkästchen **Projekt exportieren** nicht aktiviert ist. Klicken Sie auf **Erstellen und ausführen**. Sie werden zum Speichern Ihrer `.apk`-Datei aufgefordert und können einen beliebigen Namen dafür wählen.

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) wählen. Mit der ersten Option können Sie einen Anker erstellen, der später auf demselben oder einem anderen Gerät angeordnet werden kann. Bei der zweiten Option können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App vorher entweder auf demselben oder einem anderen Gerät ausgeführt haben.

## <a name="to-deploy-to-an-ios-device"></a>Durchführen der Bereitstellung für ein iOS-Gerät

Öffnen Sie **Buildeinstellungen**, indem Sie **Datei** -> **Buildeinstellungen** wählen.

Fügen Sie unter **Scenes In Build** (Szenen im Build) ein Häkchen neben der Szene `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` ein, und entfernen Sie die Häkchen für alle anderen Szenen.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Befolgen Sie die Anweisungen in der App. Sie können die Option **Create & Share Anchor** (Anker erstellen und freigeben) oder **Locate Shared Anchor** (Freigegebenen Anker suchen) wählen. Mit der ersten Option können Sie einen Anker erstellen, der später auf demselben oder einem anderen Gerät angeordnet werden kann. Bei der zweiten Option können Sie nach zuvor freigegebenen Ankern suchen, wenn Sie die App vorher entweder auf demselben oder einem anderen Gerät ausgeführt haben.

Beenden Sie die App in Xcode, indem Sie **Beenden** wählen.
