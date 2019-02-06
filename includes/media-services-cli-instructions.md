---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/25/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b335cf996de41f4eea15af1899a0c6654c2f679f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104982"
---
## <a name="open-cli-shell"></a>Öffnen der CLI-Shell

Es wird empfohlen, [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) zu verwenden, um CLI-Befehle auszuführen. **Cloud Shell** ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Allgemeine Tools sind in Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wählen Sie einfach die Schaltfläche „Kopieren“ aus, um den Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen. Cloud Shell kann auf mehrere Arten geöffnet werden:

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Anmeldung

Um mit der Verwendung der CLI-Shell (in der Cloud oder lokal) zu beginnen, führen Sie `az login` aus, um eine Verbindung mit Azure herzustellen.

Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Anmeldeseite. Andernfalls müssen Sie eine Browserseite öffnen und die Anweisungen zur Befehlszeile befolgen, um einen Autorisierungscode einzugeben, nachdem Sie in Ihrem Browser zu https://aka.ms/devicelogin navigiert sind.

### <a name="specify-location-of-files"></a>Angeben des Speicherorts für Dateien

Bei vielen Media Services-CLI-Befehlen können Sie einen Parameter mit einem Dateinamen übergeben. 

Wenn Sie **Azure Cloud Shell** verwenden, laden Sie die Datei zur **Azure Cloud Shell** hoch. Die Schaltfläche zum Hoch- und Herunterladen von Dateien befindet sich im oberen Bereich des Shellfensters. Verweisen Sie dann folgendermaßen auf die Datei: `@{FileName}.` 

![Hochladen von Dateien]

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal verwenden, geben Sie den gesamten Dateipfad an. Beispiel: `@c:\tracks.json`.


[Hochladen von Dateien]: ./media/media-services-cli/upload-download-files.png
