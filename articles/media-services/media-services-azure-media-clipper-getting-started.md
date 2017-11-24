---
title: Erste Schritte mit Azure Media Clipper | Microsoft-Dokumentation
description: Erste Schritte mit Azure Media Clipper, einem Tool zum Erstellen von Medienclips aus Medienobjekten
services: media-services
keywords: Clip;Schneiden;Subclip;Codierung;Medien
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8a4f2c79131664ca0d078fa58c6a75b54243e705
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Erstellen von Clips mit Azure Media Clipper
Dieser Abschnitt zeigt Ihnen die grundlegenden Schritte für den Einstieg in Azure Media Clipper. In den darauffolgenden Abschnitten wird die Konfiguration von Azure Media Clipper erläutert.

- Fügen Sie der Kopfzeile Ihres Dokuments zunächst die folgenden Links für Azure Media Player und Azure Media Clipper hinzu. Es empfiehlt sich, in den URLs explizit eine bestimmte Version von Clipper und Azure Media Player anzugeben. Verwenden Sie in Produktionsumgebungen nicht die letzte Version dieser Ressourcen, da diese sich ggf. ändern können.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Fügen Sie anschließend die folgenden Klassen zu dem div-Element hinzu, in dem Sie Clipper instanziieren möchten.

```javascript
<div id="root" class="azure-subclipper" />
```

Um das dunkle Design zu aktivieren, fügen Sie optional die dark-skin-Klasse hinzu:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Als Nächstes instanziieren Sie Clipper mit dem folgenden API-Aufruf:

```javascript
var subclipper = new subclipper({
    selector: '#root',
    logLevel: 'info',
    restVersion: '2.0',
    minimumMarkerGap: 6,
    submitSubclipCallback: onSubmitSubclip,
    singleBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with single H264Layers
                }, {
                    // Audio Codec
                }]
    },
    multiBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with multiple H264Layers
                }, {
                    // Audio Codec
                }]
    },
    keymap: {
      // See below for more info
    },
   // Enable the Video Assets panel in the widget to automatically load assets (input contract)
    assetsPanelLoaderCallback: onLoadAssets,
    height: 600,
    subclippingMode: 'all',
    filterAssetsTypes: true,
    speedLevels: [
        { name: '4x', value: 4.0 },
        { name: '3x', value: 3.0 },
        { name: '2x', value: 2.0 },
        { name: 'normal', value: 1.0 },
        { name: '0.75x', value: 0.75 },
        { name: '0.5x', value: 0.5 },
    ],
    resetOnJobDone: false,
    autoplayVideo: true,
    language: 'en'    
});
```

Folgende Parameter sind im Aufruf der Initialisierungsmethode erforderlich:
- `selector` {REQUIRED, string}: CSS-Selektor des passenden HTML-Elements, in dem das Widget gerendert werden soll.
- `restVersion` {REQUIRED, string}: Die Zielversion der Azure Media Services-REST-API. Die REST-Version definiert das Format der vom Widget generierten Ausgabe. Derzeit wird nur 2.0 unterstützt.
- `submitSubclipCallback` {REQUIRED, promise}: Die aufgerufene Rückruffunktion, wenn auf die Schaltfläche „Senden“ des Widgets geklickt wird. Die Rückruffunktion sollte die vom Widget generierte Ausgabe erwarten (eine Renderauftragskonfiguration oder eine Filterdefinition). Weitere Informationen finden Sie in der Rückruffunktion zum Senden des Subclips.
- `logLevel` {OPTIONAL, {'info', 'warn', 'error'}}: Die Protokollierungsebene, die in der Browserkonsole angezeigt werden soll. Standardwert: Fehler
- `minimumMarkerGap` {OPTIONAL, int}: Die Mindestlänge eines Subclips (in Sekunden). Hinweis: Der Wert sollte mindestens 6 betragen; dies ist außerdem der Standardwert.
- `singleBitrateMp4Profile` {OPTIONAL, JSON object}: Das MP4-Profil mit Einzelbitrate, das für die vom Widget generierte Renderauftragskonfiguration verwendet werden soll. Wenn kein Profil angegeben ist, wird das [MP4-Standardprofil mit Einzelbitrate](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p) verwendet.
- `multiBitrateMp4Profile` {OPTIONAL, JSON object}: Das MP4-Profil mit Mehrfachbitrate, das für die vom Widget generierte Renderauftragskonfiguration verwendet werden soll. Wenn kein Profil angegeben ist, wird das [MP4-Standardprofil mit Mehrfachbitrate](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p) verwendet.
- `keymap` {OPTIONAL, json object}: Ermöglicht die Anpassung der Tastenkombinationen des Widgets. Weitere Informationen finden Sie unter [Konfigurieren von Azure Media Clipper-Tastenkombinationen](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback` {OPTIONAL, promise}: Die Rückruffunktion, die aufgerufen wird, um jedes Mal, wenn ein Benutzer zum Ende des Medienobjektbereichs scrollt, (asynchron) eine neue Seite mit Medienobjekten in den Bereich zu laden. Weitere Informationen finden Sie in der Rückruffunktion zum Laden des Medienobjektbereichs.
- `height` {OPTIONAL, number}: Die Gesamthöhe des Widgets. Die Mindesthöhe beträgt 600 px ohne Medienobjektbereich und 850 px mit Medienobjektbereich.
- `subclippingMode` (OPTIONAL, {'all', 'render', 'filter'}): Die zulässigen Modi für die Subcliperstellung. Der Standardwert lautet „Alle“.
- `filterAssetsTypes` (OPTIONAL, bool): filterAssetsTypes ermöglicht Ihnen, die Filterdropdownliste im Medienobjektbereich ein- und auszublenden. Der Standardwert lautet „true“.
- `speedLevels` (OPTIONAL, array): speedLevels ermöglicht das Festlegen verschiedener Geschwindigkeiten für die Videowiedergabe. Weitere Informationen finden Sie in der [Dokumentation zu Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions).
- `resetOnJobDone` (OPTIONAL, bool): resetOnJobDone ermöglicht Clipper, die Subcliperstellung auf den ursprünglichen Zustand zurückzusetzen, wenn ein Auftrag erfolgreich übermittelt wurde.
- `autoplayVideo` (OPTIONAL, bool): autoplayVideo ermöglicht Clipper, das Video nach dem Laden automatisch wiederzugeben. Der Standardwert lautet „true“.
- `language` {OPTIONAL, string}: „language“ legt die Sprache des Widgets fest. Wenn dieser Parameter nicht angegeben ist, versucht das Widget, die Nachrichten basierend auf der Browsersprache zu lokalisieren. Wenn im Browser keine Sprache ermittelt werden kann, wird standardmäßig Englisch verwendet. Weitere Information finden Sie im Abschnitt mit den unterstützten Sprachen.
- `languages` {OPTIONAL, JSON}: Der languages-Parameter ersetzt das Standardwörterbuch der Sprachen durch ein vom Benutzer definiertes Wörterbuch. Weitere Information finden Sie im Abschnitt mit den unterstützten Sprachen.
- `extraLanguages` (OPTIONAL, JSON): Der extraLanguages-Parameter fügt dem Standardwörterbuch neue Sprachen hinzu. Weitere Information finden Sie im Abschnitt mit den unterstützten Sprachen.

## <a name="typescript-definition"></a>TypeScript-Definition
Eine [TypeScript](https://www.typescriptlang.org/)-Definitionsdatei für Clipper finden Sie [hier](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media Clipper-API
Dieser Abschnitt dokumentiert die von Clipper bereitgestellte API-Benutzeroberfläche.

- `load(assets)`: Lädt eine Liste mit Medienobjekten in den Medienobjektbereich (sollte nicht zusammen mit `assetsPanelLoaderCallback` verwendet werden). Informationen zum Laden von Medienobjekten in Clipper finden Sie in diesem [Artikel](media-services-azure-media-clipper-load-assets.md).
- `setLogLevel(level)`: Legt die Protokollierungsebene fest, die in der Browserkonsole angezeigt werden soll. Mögliche Werte: `info`, `warn`, `error`.
- `setHeight(height)`: Legt die Gesamthöhe des Widgets in Pixeln fest. Die Mindesthöhe beträgt 600 px ohne Medienobjektbereich und 850 px mit Medienobjektbereich.
- `version`: Ruft die Widgetversion ab.

## <a name="configuring-azure-media-clipper"></a>Konfigurieren von Azure Media Clipper
Dies sind die nächsten Schritte für die Konfiguration von Azure Media Clipper:
- [Laden von Medienobjekten in Azure Media Clipper](media-services-azure-media-clipper-load-assets.md)
- [Konfigurieren von benutzerdefinierten Tastenkombinationen](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Übermitteln von Schnittaufträgen aus Clipper](media-services-azure-media-clipper-submit-job.md)

## <a name="supported-languages"></a>Unterstützte Sprachen
Das Clipper-Widget ist in 18 Sprachen verfügbar. Um die Widgetsprache festzulegen, müssen Sie während der Initialisierung den `language`-Parameter definieren. Übergeben Sie die Zeichenfolge für den gewünschten Sprachcode aus der folgenden Liste:
- Chinesisch (vereinfacht): zh-hans
- Chinesisch (traditionell): zh-hant
- Tschechisch: cs
- Niederländisch, Flämisch: nl
- Englisch: en
- Französisch: fr
- Deutsch: de
- Ungarisch: hu
- Italienisch: it
- Japanisch: ja
- Koreanisch: ko
- Polnisch: pl
- Portugiesisch (Brasilien): pt-br
- Portugiesisch (Portugal): pt-pt
- Russisch: ru
- Spanisch: es
- Schwedisch: sv
- Türkisch: tr

Um ein benutzerdefiniertes Sprachwörterbuch festzulegen oder das standardmäßige Sprachwörterbuch zu erweitern, müssen Sie den Parameter `languages` bzw. `extraLanguages` definieren. Übergeben Sie ein benutzerdefiniertes Wörterbuch in folgendem JSON-Format:

```javascript
{
      "{language-code}":
          "{message-id}": "{message}"
          ...
      }
      ...
}
```

Folgendes Beispiel definiert die lokalisierten englischen Zeichenfolgen:

```javascript
export default {
  'VideoPlayer.noPreview': 'No video preview',
  'VideoPlayer.loadAsset': 'You must provide a valid asset',
  'AssetsPanel.name': 'Name',
  'AssetsPanel.type': 'Asset type',
  'AssetsPanel.actions': 'Actions',
  'AssetsPanel.loading': 'Loading...',
  'AssetsPanel.duration': 'Duration',
  'AssetsPanel.resolution': 'Resolution',
  'AssetsPanel.pluralFiles': '{0} assets',
  'AssetsPanel.searchFiles': 'Search assets',
  'AssetsPanel.showTypes': 'Show:',
  'AssetsPanel.typesInfo': 'Rendered assets are actual MP4 files. Dynamic manifest filters are filters applied to a parent asset\'s video segment playlist.',
  'AssetsPanel.filterTypes': 'Filters',
  'AssetsPanel.assetTypes': 'Assets',
  'AssetsPanel.assetsAll': 'All',
  'AssetsPanel.addAsset': 'Add asset to the end',
  'AssetsPanel.addFilter': 'Add filter to the timeline',
  'AssetsPanel.invalidAsset': 'The metadata of this asset is not compatible with the other assets in the timeline',
  'AssetsPanel.addAssetWarning': 'Subclipping on assets with different resolutions may cause resolution autoscaling.',
  'AssetsPanel.live': 'LIVE',
  'AssetsPanel.unknown': 'UNKNOWN',
  'AssetsPanel.minimGapNotMet': 'The asset duration must be greater than the minimum clip duration ({0} seconds)',
  'VideoPlayer.openAdvancedSettings': 'Advanced settings',
  'VideoPlayer.singleBitrate': 'Single-bitrate MP4 (rendered)',
  'VideoPlayer.multiBitrate': 'Multi-bitrate MP4 (rendered)',
  'VideoPlayer.dynamicManifest': 'Dynamic manifest filter',
  'VideoPlayer.ErrorWithMessage': 'There was an error in the video player, code {0}, message: {1}',
  'Common.cancel': 'Cancel',
  'Common.OK': 'OK',
  'AdvancedSettings.framerate': 'Frame rate',
  'Dropdown.select': 'Select an option...',
  'InputAsset.RemoveInput': 'Remove source',
  'Zoom.startTime': 'Start time',
  'Zoom.endTime': 'End time',
  'VideoPlayer.subclips': 'Subclips:',
  'VideoPlayer.length': 'Clip length:',
  'Accordion.scrollLeft': 'Scroll to the left',
  'Accordion.scrollRight': 'Scroll to the right',
  'AdvancedSettings.title': 'Advanced settings',
  'AdvancedSettings.subclipName': 'Subclip name',
  'AdvancedSettings.subclipType': 'Subclipping mode',
  'AdvancedSettings.includeAudioTracks': 'Include audio tracks',
  'AdvancedSettings.subclipTypeInfo': 'Single-bitrate and multi-bitrate MP4s are frame accurate rendered assets. Dynamic manifest filters are group-of-pictures (GOP) accurate filters applied to a parent asset. Creating filters does not create a new asset and does not require encoding. Subclipping jobs on live assets are valid as long as their mark times are within the archive window of the parent asset. Filters are valid as long as the parent asset exists and mark times are within its archive window.',
  'AdvancedSettings.frameRateInfo': 'We autodetect frame rate under most scenarios. however, If we cannot autodetect, choose a frame rate from the dropdown for the selected asset(s).',
  'AdvancedSettings.frameRateError': 'Unable to determine frame rate',
  'AdvancedSettings.subclipNameInfo': 'Choose a name for your subclip.',
  'AdvancedSettings.singleAudioTrack': '1 audio track selected',
  'AdvancedSettings.allAudioTracks': 'All audio tracks selected',
  'AdvancedSettings.someAudioTracks': '{0} audio tracks selected',
  'AdvancedSettings.includeAllAudioTracks': 'Include all audio tracks',
  'AssetsPanel.loadingError': 'Failed to retreive assets from server.',
  'AssetsPanel.retry': 'Retry?',
  'CommandBar.prevFrameTitle': 'Back up one frame',
  'CommandBar.prevKeyFrameTitle': 'Back up one GOP',
  'CommandBar.cleanJob': 'Remove all assets',
  'CommandBar.cleanJobTitle': 'Remove all assets from timeline',
  'CommandBar.cleanJobMessage': 'This will empty all video clips from your timeline.',
  'CommandBar.update': 'Update filter',
  'CommandBar.createFilter': 'Create filter',
  'CommandBar.submit': 'Submit subclipper job',
  'CommandBar.jobErrorTitle': 'Operation failed',
  'CommandBar.jobErrorMessage': 'Your subclip failed to submit. Please try again.',
  'CommandBar.markInTitle': 'Set in at playhead',
  'CommandBar.markInPosition': 'Mark in timecode',
  'CommandBar.markOutTitle': 'Set out at playhead',
  'CommandBar.markOutPosition': 'Mark out timecode',
  'CommandBar.nextFrameTitle': 'Advance one frame',
  'CommandBar.nextKeyFrameTitle': 'Advance one GOP',
  'CommandBar.play': 'Play video',
  'CommandBar.pause': 'Pause video',
  'CommandBar.playPreviewTitle': 'Play subclip preview',
  'CommandBar.pausePreviewTitle': 'Pause subclip preview',
  'CommandBar.redoTitle': 'Redo last action',
  'CommandBar.removeAsset': 'Remove current asset',
  'CommandBar.undoTitle': 'Undo last action',
  'VideoPlayer.errorTitle': 'Error',
  'VideoPlayer.errorMessage': 'There was an error loading the selected asset.',
  'Timeline.markIn': 'Mark in bracket',
  'Timeline.markOut': 'Mark out bracket',
  'Timeline.playHead': 'Play head',
};
```