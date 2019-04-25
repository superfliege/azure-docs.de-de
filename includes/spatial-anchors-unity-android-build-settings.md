---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 228f445dda2724985154723a292adb8215a5ad68
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012117"
---
Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Wählen Sie im Abschnitt **Plattform** die Option **Android** aus. Ändern Sie das **Buildsystem** in **Gradle**, und wählen Sie **Projekt exportieren** (Export Project) aus.

Wählen Sie **Plattform wechseln** aus, um die Plattform in **Android** zu ändern. Bei Bedarf werden Sie in Unity zur Installation fehlender unterstützender Android-Komponenten aufgefordert.

![Fenster für Unity-Buildeinstellungen](./media/spatial-anchors-unity/unity-android-build-settings.png)

Schließen Sie das Fenster **Buildeinstellungen**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Herunterladen und Importieren des ARCore SDK für Unity

Laden Sie die Datei `unitypackage` aus den [ARCore SDK-Releases für Unity 1.7](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0) herunter. Wählen Sie im Unity-Projekt die Optionen **Ressourcen** > **Paket importieren** > **Benutzerdefiniertes Paket** und anschließend die zuvor heruntergeladene Datei `unitypackage` aus. Vergewissern Sie sich im Dialogfeld zum **Importieren des Unity-Pakets**, dass alle Dateien ausgewählt sind, und wählen Sie dann **Importieren** aus.
