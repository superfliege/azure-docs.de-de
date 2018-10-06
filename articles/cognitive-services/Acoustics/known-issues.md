---
title: Bekannte Probleme mit dem Acoustics-Plug-In – Cognitive Services
description: Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.
services: cognitive-services
author: kylestorck
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: c19b19cab33ae868f11ded0b7ce87dac99269596
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431986"
---
# <a name="known-issues"></a>Bekannte Probleme
Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Beim Umbenennen einer Szene gehen akustische Parameter verloren

Wenn Sie eine Szene umbenennen, werden nicht alle zu dieser Szene gehörenden akustischen Parameter automatisch an die neue Szene übertragen. Sie bleiben jedoch in der alten ASSET-Datei erhalten. Suchen Sie nach der Datei **Szenenname_Akustikparameter.asset** im Verzeichnis **Editor** neben Ihrer Szenendatei. Benennen Sie Ihre Datei entsprechend dem neuen Szenennamen um.

## <a name="the-default-path-for-the-acousticsdata-folder-in-probes-tab-is-an-absolute-path"></a>Beim Standardpfad für den Ordner „AcousticsData“ auf der Registerkarte „Tests“ handelt es sich um einen absoluten Pfad

Standardmäßig sollte hier eigentlich ein relativer Pfad angezeigt werden, um die gemeinsame Nutzung von Projekten durch mehrere Projektmitarbeiter zu erleichtern. Sie können dieses Problem umgehen, indem Sie den Pfad in ein relatives Projektverzeichnis ändern.

## <a name="runtime-voxels-are-a-different-size-than-scene-preview-voxels"></a>Voxel haben zur Laufzeit eine andere Größe als während der Szenenvorschau

Wenn Sie auf der Registerkarte **Tests** eine **Berechnung** durchführen und die Voxel anzeigen, anschließend einen Bake-Vorgang durchführen und die Voxel zur Laufzeit für dieselbe Szene anzeigen, weisen die Voxel unterschiedliche Größen auf. Bei den Voxel vor dem Bake-Vorgang handelt es sich um die in der Simulation verwendeten Voxel. Die zur Laufzeit angezeigten Voxel werden für die Interpolation zwischen Testpunkten verwendet. Dies kann eine Unstimmigkeit verursachen, bei der Portale zur Laufzeit offen zu sein scheinen, jedoch tatsächlich nicht offen sind.

## <a name="uwp-builds-not-working"></a>UWP-Builds funktionieren nicht

Bei den neuesten Versionen von Unity (2018.2 und höher) werden UWP-Builds nicht erfolgreich ausgeführt. Die Ausführungsphase des Builds blockiert, und die Fehlermeldung „Unity extensions are not yet initialized“ (Unity-Erweiterungen noch nicht initialisiert) wird angezeigt. Dieses Problem wird mit [diesem Unity-Problem](https://fogbugz.unity3d.com/default.asp?1070491_1rgf14bakv5u779d) nachverfolgt.

## <a name="unity-crashes-when-closing-project"></a>Unity stürzt beim Schließen des Projekts ab

Die neuesten Versionen von Unity (2018.2 und höher) haben den bekannten Fehler, dass Unity abstürzt, wenn Sie Ihr Projekt schließen. Dieses Problem wird mit [diesem Unity-Problem](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project) nachverfolgt.

## <a name="trouble-deploying-to-android"></a>Probleme bei der Bereitstellung für Android
Wenn Sie Project Acoustics unter Android verwenden möchten, ändern Sie Ihr Buildziel in Android. Bei einigen Versionen von Unity tritt beim Bereitstellen von Audio-Plug-Ins ein Fehler auf. Stellen Sie sicher, dass Sie keine Version verwenden, die von [diesem Fehler](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) betroffen ist.

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Die Fehlermeldung „could not find metadata file System.Security.dll“ (Die Metadatendatei „System.Security.dll“ wurde nicht gefunden) wird angezeigt

Stellen Sie sicher, dass für die Scripting Runtime Version in den Player-Einstellungen **.NET 4.x Equivalent** (.NET 4.x-Entsprechung) festgelegt ist, und starten Sie Unity neu.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ich habe beim Herstellen einer Verbindung mit Azure Authentifizierungsprobleme

Vergewissern Sie sich, dass Sie die richtigen Anmeldeinformationen für Ihr Azure-Konto verwendet haben, dass Ihr Konto den im Bake-Vorgang angeforderten Knotentyp unterstützt und dass Ihre Systemuhr stimmt.

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit der [Integration von Akustik in das Unity-Projekt](getting-started.md)

