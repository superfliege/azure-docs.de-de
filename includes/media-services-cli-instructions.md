---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 8439da94c770bee313a1ae1d1da5df30683cd2ad
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155712"
---
## <a name="cli-shell"></a>CLI-Shell

Es wird empfohlen, [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) zu verwenden, um CLI-Befehle auszuführen. **Cloud Shell** ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Allgemeine Tools sind in Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Es bietet Ihnen die Flexibilität, die Shell-Benutzeroberfläche auszuwählen, die sich am besten für Ihre Arbeitsweise eignet. Linux-Benutzer können Bash-Benutzeroberflächen verwenden, während Windows-Benutzer PowerShell nutzen können.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Anmeldung

Um mit der Verwendung der CLI-Shell (in der Cloud oder lokal) zu beginnen, führen Sie `az login` aus, um eine Verbindung mit Azure herzustellen.

Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Anmeldeseite. Andernfalls müssen Sie eine Browserseite öffnen und die Anweisungen zur Befehlszeile befolgen, um einen Autorisierungscode einzugeben, nachdem Sie in Ihrem Browser zu https://aka.ms/devicelogin navigiert sind.

### <a name="specify-location-of-files"></a>Angeben des Speicherorts für Dateien

Bei vielen Media Services-CLI-Befehlen können Sie einen Parameter mit einem Dateinamen übergeben. Bei Verwendung von **Cloud Shell** können Sie die Datei auf Ihr Clouddrive hochladen (mithilfe von Bash oder PowerShell). 

![Hochladen von Dateien]

Gleich, ob Sie eine lokale Befehlszeilenschnittstelle oder **Cloud Shell** verwenden, Sie müssen den Dateipfad gemäß dem verwendeten Betriebssystem bzw. der Cloud Shell (Bash oder PowerShell) angeben. Im Anschluss finden Sie einige Beispiele:

Relativer Pfad zur Datei (alle Betriebssysteme)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Absoluter Dateipfad unter den Betriebssystemen Linux/Mac und Windows

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`

Verwenden Sie `{file}`, wenn der Befehl nach einem Pfad zur Datei fragt. Beispiel: `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Verwenden Sie `@{file}`, wenn der Befehl die angegebene Datei lädt. Beispiel: `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Hochladen von Dateien]: ./media/media-services-cli/upload-download-files.png
