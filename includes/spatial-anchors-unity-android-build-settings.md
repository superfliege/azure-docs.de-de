---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 662aced6df27febdf29f2645725962763e89cfa2
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752552"
---
Öffnen Sie Unity, und öffnen Sie das Projekt im Ordner `Unity`.

Wählen Sie **Datei** -> **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Wählen Sie im Abschnitt **Plattform** die Option **Android** aus. Ändern Sie dann das **Buildsystem** in **Gradle**, und aktivieren Sie die Option **Projekt exportieren**.

Wählen Sie **Plattform wechseln** aus, um die Plattform in **Android** zu ändern. Unity fordert Sie bei Bedarf zur Installation fehlender unterstützender Android-Komponenten auf.

![Unity-Buildeinstellungen](./media/spatial-anchors-unity/unity-android-build-settings.png)

Schließen Sie das Fenster **Buildeinstellungen**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Herunterladen und Importieren des ARCore SDK für Unity

Laden Sie die Datei `unitypackage` aus den [ARCore SDK-Releases für Unity](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0) herunter. Wählen Sie im Unity-Projekt **Ressourcen** -> **Paket importieren** -> **Benutzerdefiniertes Paket...** und anschließend die Datei `unitypackage` aus, die Sie zuvor heruntergeladen haben. Vergewissern Sie sich im Dialogfeld zum **Importieren des Unity-Pakets**, dass alle Dateien ausgewählt sind, und wählen Sie dann **Importieren** aus.
