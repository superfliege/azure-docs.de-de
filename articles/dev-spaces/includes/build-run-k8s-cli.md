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
ms.openlocfilehash: fbbd66dea73747acaf1c267f7d3ba7b1bb17baa2
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939162"
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

`Service 'webfrontend' port 'http' is available at <url>` 

Öffnen Sie diese URL in einem Browserfenster. Dort sollten Sie sehen, dass die Web-App geladen wird. Wenn der Container ausgeführt wird, wird die Ausgabe von `stdout` und `stderr` an das Terminalfenster gestreamt.

> [!Note]
> Beim ersten Ausführen kann es mehrere Minuten dauern, bis das öffentliche DNS bereit ist. Wenn die öffentliche URL nicht aufgelöst wird, können Sie die alternative URL http://localhost:<portnumber> verwenden, die in der Konsolenausgabe angezeigt wird. Bei Verwendung der localhost-URL kann es so aussehen, als ob der Container lokal ausgeführt wird, während er stattdessen unter AKS ausgeführt wird. Um die Interaktion mit dem Dienst auf Ihrem lokalen Computer zu erleichtern, erstellt Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container. Sie können später zurückkehren und versuchen, die öffentliche URL zu verwenden, wenn der DNS-Eintrag bereit ist.
