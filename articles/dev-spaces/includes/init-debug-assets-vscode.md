---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: e4bde4f521e0e19e7acd36260c98cfe80973e284
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198234"
---
## <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Initialisieren von Debugressourcen mit der VS Code-Erweiterung
Sie müssen zuerst Ihr Codeprojekt konfigurieren, damit Visual Studio Code (VS Code) mit unserer Entwicklungsumgebung in Azure kommunizieren kann. Die VS Code-Erweiterung für Azure Dev Spaces enthält einen Befehl zum Einrichten der Debugkonfiguration. 

Öffnen Sie die **Befehlspalette** (über das Menü **Ansicht | Befehlspalette**), verwenden Sie die automatische Vervollständigung für die Eingabe, und wählen Sie den Befehl `Azure Dev Spaces: Create configuration files for connected development` aus. 

Dadurch wird die Debugkonfiguration für Azure Dev Spaces unter dem `.vscode`-Ordner hinzugefügt.

![](../media/common/command-palette.png)

> [!Important]
> Aufgrund eines Fehlers müssen Sie VS Code schließen und erneut öffnen, bevor Sie fortfahren können.