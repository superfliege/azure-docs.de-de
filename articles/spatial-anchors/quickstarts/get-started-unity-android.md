---
title: 'Schnellstart: Erstellen einer Android-Unity-App mit Azure Spatial Anchors | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine Android-App mit Unity erstellen, indem Sie Spatial Anchors verwenden.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: e92c812ffc8b72fe79248c602e48ff01ef9fefcb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961008"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>Schnellstart: Erstellen einer Android-Unity-App mit Azure Spatial Anchors

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit [Azure Spatial Anchors](../overview.md) eine Android-Unity-App erstellen. Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine mit Unity erstellte ARCore-Android-App, mit der ein räumlicher Anker gespeichert und abgerufen werden kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Spatial Anchors-Kontos
> * Vorbereiten von Unity-Buildeinstellungen
> * Herunterladen und Importieren des ARCore-SDK für Unity
> * Konfigurieren des Bezeichners und Kontoschlüssels für das Spatial Anchors-Konto
> * Exportieren des Android Studio-Projekts
> * Bereitstellen und Ausführen auf einem Android-Gerät

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

- Ein Windows- oder macOS-Computer mit Installation von <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a> und <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3+</a>.
- Ein <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">für Entwickler geeignetes</a> und <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore-fähiges</a> Android-Gerät.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Öffnen des Beispielprojekts in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Speichern Sie die Szene, indem Sie **Datei** -> **Speichern** wählen.

## <a name="export-the-android-studio-project"></a>Exportieren des Android Studio-Projekts

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Wählen Sie **Exportieren**, um ein Dialogfeld zu öffnen. Wählen Sie anschließend einen Ordner zum Exportieren des Android Studio-Projekts aus.

Nach Abschluss des Exportvorgangs wird ein Ordner mit dem exportierten Android Studio-Projekt angezeigt, der den Unterordner **HelloAR U3D** enthält.

## <a name="deploy-the-android-application"></a>Bereitstellen der Android-Anwendung

Öffnen Sie Android Studio, und wählen Sie **Open an existing Android Studio project** (Vorhandenes Android Studio-Projekt öffnen). Wählen Sie anschließend den Unterordner **HelloAR U3D** des exportierten Android Studio-Projekts aus, und klicken Sie auf **OK**.

Beim Öffnen wird eine Eingabeaufforderung mit dem Hinweis angezeigt, dass der Gradle-Wrapper verwendet werden soll. Wählen Sie **OK**, um den Gradle-Wrapper zu verwenden und das Projekt zu öffnen.

Schalten Sie das Android-Gerät ein, melden Sie sich an, und stellen Sie dafür per USB-Kabel eine Verbindung mit dem PC her.

Wählen Sie in der Android Studio-Symbolleiste die Option **Ausführen**.

![Bereitstellen und Ausführen von Android Studio](./media/get-started-unity-android/android-studio-deploy-run.png)

Wählen Sie das Android-Gerät im Dialogfeld **Select Deployment Target** (Bereitstellungsziel auswählen) aus, und wählen Sie anschließend **OK**, um die App auf dem Android-Gerät auszuführen.

Befolgen Sie in der App die Anleitung zum Anordnen und Abrufen eines Ankers.

> [!NOTE]
> Falls beim Ausführen der App die Kamera nicht als Hintergrund angezeigt wird (sondern stattdessen beispielsweise ein leerer blauer Bereich oder andere Texturen), müssen Sie die Ressourcen wahrscheinlich erneut in Unity importieren. Beenden Sie die App. Wählen Sie in Unity im Menü am oberen Rand die Option **Assets > Re-import all** (Ressourcen > Alle erneut importieren). Führen Sie die App dann erneut aus.

Beenden Sie die App, indem Sie in der Android Studio-Symbolleiste **Beenden** wählen.

![Android Studio: Beenden](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)
