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
ms.openlocfilehash: 484567dd9d9c3d050e7be25bd685a5b8d3de0687
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825529"
---
## <a name="build-and-run-code-in-kubernetes"></a>Erstellen und Ausführen von Code in Kubernetes
Führen Sie Ihren Code jetzt aus! Führen Sie im Terminalfenster diesen Befehl aus dem **Codestammordner** des Web-Front-Ends aus:

```cmd
azds up
```

Achten Sie auf die Ausgabe des Befehls, Sie werden während der Ausführung verschiedene Punkte bemerken:
- Quellcode wird mit dem Entwicklungsbereich in Azure synchronisiert.
- In Azure wird ein Containerimage erstellt, wie von den Docker-Objekten in Ihrem Codeordner angegeben.
- Es werden Kubernetes-Objekte erstellt, die das Containerimage verwenden, wie vom Helm-Diagramm in Ihrem Codeordner angegeben.
- Es werden Informationen zu den Endpunkten des Containers angezeigt. In unserem Fall erwarten wir eine öffentliche HTTP-URL.
- Wenn die oben genannten Phasen erfolgreich abgeschlossen sind, sollten Sie die Ausgabe von `stdout` (und `stderr`) sehen, wenn der Container startet.

> [!Note]
> Bei der ersten Ausführung von `up` dauern diese Schritte etwas länger, spätere Ausführungen erfolgen dann aber schneller.

### <a name="test-the-web-app"></a>Testen der Web-App
Suchen Sie in der Konsolenausgabe nach Informationen zur öffentlichen URL, die vom Befehl `up` erstellt wurde. Sie weist folgendes Format auf: 

`Running at public URL: http://<servicename>-<cluster-name>.<guid>.<region>.aksapp.io` 

Öffnen Sie diese URL in einem Browserfenster. Dort sollten Sie sehen, dass die Web-App geladen wird. Wenn der Container ausgeführt wird, wird die Ausgabe von `stdout` und `stderr` an das Terminalfenster gestreamt.
