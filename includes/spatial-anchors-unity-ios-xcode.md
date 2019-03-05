---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b802c9dbd0cef65325cb03538b68b49c57b85bb3
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56891003"
---
Wählen Sie **Build** aus, um ein Dialogfeld zu öffnen. Wählen Sie anschließend einen Ordner zum Exportieren des Xcode-Projekts aus.

Nach Abschluss des Exportvorgangs wird ein Ordner mit dem exportierten Xcode-Projekt angezeigt.

### <a name="open-the-xcode-project"></a>Öffnen des Xcode-Projekts

Führen Sie im Ordner des exportierten Xcode-Projekts den folgenden Befehl aus, um die erforderlichen CocoaPods für das Projekt zu installieren:

```bash
pod install --repo-update
```

Nun können Sie `Unity-iPhone.xcworkspace` öffnen, um das Projekt in Xcode zu öffnen:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Sollte ein Fehler vom Typ `library not found for -lPods-Unity-iPhone` auftreten, haben Sie wahrscheinlich anstelle der Datei vom Typ `.xcworkspace` die Datei vom Typ `.xcodeproj` geöffnet. Öffnen Sie die Datei vom Typ `.xcworkspace`, und wiederholen Sie den Vorgang.

Wählen Sie den Stammknoten **Unity-iPhone** aus, um die Projekteinstellungen anzuzeigen, und wählen Sie die Registerkarte **General** (Allgemein) aus.

Wählen Sie unter **Signing** (Signierung) die Option **Automatically manage signing** (Signierung automatisch verwalten) aus. Wählen Sie im angezeigten Dialogfeld die Option **Enable Automatic** (Automatik aktivieren) aus, um die Buildeinstellungen zurückzusetzen.

Vergewissern Sie sich unter **Deployment Info** (Bereitstellungsinformationen), dass das Bereitstellungsziel (**Deployment Target**) auf `11.0` festgelegt ist.

### <a name="deploy-the-app-to-your-ios-device"></a>Bereitstellen der App auf Ihrem iOS-Gerät

Verbinden Sie das iOS-Gerät mit dem Mac, und legen Sie das **aktive Schema** auf Ihr iOS-Gerät fest.

![Auswählen des Geräts](./media/spatial-anchors-unity/select-device.png)

Wählen Sie **Build and then run the current scheme** (Aktuelles Schema erstellen und dann ausführen) aus.

![Bereitstellen und Ausführen](./media/spatial-anchors-unity/deploy-run.png)