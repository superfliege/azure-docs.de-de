---
title: 'Beispiel: „Projekt: Akustik“'
titlesuffix: Azure Cognitive Services
description: In dieser exemplarischen Vorgehensweise wird die Unity-Beispielszene für Project Acoustics mit der Bereitstellung auf dem Desktop und in einer VR beschrieben.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: sample
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: f5ea565e68579dfad601d1037daeb4113e3daa43
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901157"
---
# <a name="unity-sample-walkthrough"></a>Exemplarische Vorgehensweise zu Unity
Dies ist eine exemplarische Vorgehensweise zum Project Acoustics-Beispiel. Weitere Informationen dazu, was Project Acoustics ist, finden Sie unter [Introduction to Project Acoustics (Einführung in Project Acoustics)](what-is-acoustics.md). Unterstützung beim Hinzufügen des Project Acoustics-Pakets zu einem bereits vorhandenen Unity-Projekt finden Sie im [Leitfaden für erste Schritte](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Voraussetzungen für die Ausführung des Beispielprojekts
* Unity 2018.2 und höher mit Skriptlaufzeitversion .NET 4.x
* Unity-Editor für Windows (64-Bit)
* Das Beispiel unterstützt Windows Desktop, UWP und Android-Ziele mit Head-Mounted Displays (HMDs).
* Ein Azure Batch-Abonnement ist für Bake-Vorgänge erforderlich.

## <a name="sample-project-setup"></a>Einrichtung des Beispielprojekts
Laden Sie das Paket **MicrosoftAcoustics.Sample.unitypackage** herunter, und importieren Sie es. Beim Importieren werden Projekteinstellungen wie **Spatializer** und **Skriptlaufzeitversion** entsprechend der Anforderungen des Plug-Ins aktualisiert. Nach dem Importieren wird in der Unity-Konsole eine Fehlermeldung von **AcousticsGeometry.cs** zum Ändern der Skriptlaufzeitversion in **.NET 4.x Equivalent** (.NET 4.x-Entsprechung) angezeigt. Diese Einstellung wird im Rahmen des Paketimports geändert. Danach muss Unity neu gestartet werden, damit die Änderung wirksam wird. Starten Sie Unity jetzt neu.

## <a name="running-the-sample"></a>Ausführen des Beispiels
Das Beispiel enthält die Demoszene **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Diese Szene verfügt über drei Soundquellen. Standardmäßig wird nur eine Soundquelle wiedergegeben, und die anderen zwei sind angehalten. Diese befinden sich unter **Soundquellen** in der **Hierarchie**. Zur Unterstützung beim Erstellen eines allgemeinen Navigationsskripts ist die Hauptkamera ein untergeordnetes Element des Objekts „CameraHolder“. 

![Hierarchieansicht](media/SampleHierarchyView.png)

Die Szene wurde bereits erstellt und enthält eine ACE-Datei, die dem Prefab **MicrosoftAcoustics** in der **Hierarchie** zugeordnet ist. 

Hören Sie sich an, wie die Szene klingt, indem Sie im Unity-Editor auf die Wiedergabeschaltfläche klicken. Navigieren Sie auf dem Desktop mit den Tasten W, A, S und D sowie mit der Maus. Um zu vergleichen, wie die Szene mit und ohne Akustik klingt, drücken Sie so lange die Taste **R**, bis der Überlagerungstext rot wird und besagt "Akustik: Deaktiviert". Um Tastenkombinationen für weitere Steuerelemente anzuzeigen, drücken Sie **F1**. Alle Steuerelemente können auch verwendet werden, indem Sie mit der rechten Maustaste klicken, um die auszuführende Aktion auszuwählen, und dann durch Klicken mit der linken Maustaste die Aktion ausführen.

## <a name="targeting-other-platforms"></a>Verwenden von anderen Plattformen als Ziel
Das Beispiel enthält Einstellungen zum Ausführen unter Windows Desktop, UWP, Windows Mixed Reality, Android und Oculus Go. Standardmäßig ist das Projekt für Windows Desktop konfiguriert. Wenn Sie eine VR-Plattform als Ziel verwenden möchten, wechseln Sie zu den Playereinstellungen (**Edit > Project Settings > Player** (Bearbeiten > Projekteinstellungen > Player)), suchen Sie die **XR Settings** (VR-Einstellungen), und aktivieren Sie das Kontrollkästchen **Virtual Reality Supported** (Virtuelle Realität unterstützen).

![Aktivieren von VR](media/VRSupport.png)  

Schließen Sie ein VR-Headset an Ihren PC an. Wechseln Sie zu **File > Build Settings** (Datei > Buildeinstellungen), und klicken Sie auf **Build and Run** (Erstellen und Ausführen), um das Beispiel für Ihr VR-Headset bereitzustellen. Navigieren Sie mit den Motion-Controllern für Ihr Headset durch die Szenen, oder verwenden Sie die Tasten W, A, S und D auf der Tastatur.    
Wenn Sie Android und Oculus Go als Ziel verwenden möchten, wählen Sie im Menü **Build Settings** (Buildeinstellungen) die Option „Android“ aus. Klicken Sie auf **Switch Target** (Ziel wechseln) und dann auf **Build and Run** (Erstellen und Ausführen). Dadurch wird die Beispielszene für das angeschlossene Android-Gerät bereitgestellt. Weitere Informationen zur Unity-Entwicklung für Android finden Sie in der [Dokumentation zu Unity](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Android-Zielgeräte](media/TargetAndroid.png)  

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Azure-Kontos](create-azure-account.md) für eigene Bake-Vorgänge
* Untersuchen des [Entwurfsprozesses](design-process.md)

