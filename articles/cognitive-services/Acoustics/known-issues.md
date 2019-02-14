---
title: Bekannte Probleme mit dem Project Acoustics-Plug-In
titlesuffix: Azure Cognitive Services
description: Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.
services: cognitive-services
author: kylestorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: 2f7f7db31debd7663be44b79c42ab151a96ae7e5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55869036"
---
# <a name="known-issues"></a>Bekannte Probleme
Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Beim Umbenennen einer Szene gehen akustische Parameter verloren

Wenn Sie eine Szene umbenennen, werden nicht alle zu dieser Szene gehörenden akustischen Parameter automatisch an die neue Szene übertragen. Sie bleiben jedoch in der alten ASSET-Datei erhalten. Suchen Sie nach der Datei **Szenenname_Akustikparameter.asset** im Verzeichnis **Editor** neben Ihrer Szenendatei. Benennen Sie Ihre Datei entsprechend dem neuen Szenennamen um.

## <a name="runtime-voxels-are-a-different-size-than-scene-preview-voxels"></a>Voxel haben zur Laufzeit eine andere Größe als während der Szenenvorschau

Wenn Sie auf der Registerkarte **Tests** eine **Berechnung** durchführen und die Voxel anzeigen, anschließend einen Bake-Vorgang durchführen und die Voxel zur Laufzeit für dieselbe Szene anzeigen, weisen die Voxel unterschiedliche Größen auf. Bei den Voxel vor dem Bake-Vorgang handelt es sich um die in der Simulation verwendeten Voxel. Die zur Laufzeit angezeigten Voxel werden für die Interpolation zwischen Testpunkten verwendet. Dies kann eine Unstimmigkeit verursachen, bei der Portale zur Laufzeit offen zu sein scheinen, jedoch tatsächlich nicht offen sind.

## <a name="unity-crashes-when-closing-project"></a>Unity stürzt beim Schließen des Projekts ab

Die neuesten Versionen von Unity (2018.2 und höher) haben den bekannten Fehler, dass Unity abstürzt, wenn Sie Ihr Projekt schließen. Dieses Problem wird mit [diesem Unity-Problem](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project) nachverfolgt.

## <a name="trouble-deploying-to-android"></a>Probleme bei der Bereitstellung für Android
Wenn Sie Project Acoustics unter Android verwenden möchten, ändern Sie Ihr Buildziel in Android. Bei einigen Versionen von Unity tritt beim Bereitstellen von Audio-Plug-Ins ein Fehler auf. Stellen Sie sicher, dass Sie keine Version verwenden, die von [diesem Fehler](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) betroffen ist.

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Die Fehlermeldung „could not find metadata file System.Security.dll“ (Die Metadatendatei „System.Security.dll“ wurde nicht gefunden) wird angezeigt

Stellen Sie sicher, dass für die Scripting Runtime Version in den Player-Einstellungen **.NET 4.x Equivalent** (.NET 4.x-Entsprechung) festgelegt ist, und starten Sie Unity neu.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ich habe beim Herstellen einer Verbindung mit Azure Authentifizierungsprobleme

Vergewissern Sie sich, dass Sie die richtigen Anmeldeinformationen für Ihr Azure-Konto verwendet haben, dass Ihr Konto den im Bake-Vorgang angeforderten Knotentyp unterstützt und dass Ihre Systemuhr stimmt.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>Das Abbrechen eines Bake-Vorgangs lässt die Registerkarte für den Bake-Vorgang im Zustand „Wird gelöscht“.
Project Acoustics bereinigt alle Azure-Ressourcen für einen Auftrag nach erfolgreichem Abschluss oder Abbruch, was bis zu 5 Minuten dauern kann.

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit der [Integration von Akustik in das Unity-Projekt](getting-started.md)

