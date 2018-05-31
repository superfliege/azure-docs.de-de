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
ms.openlocfilehash: 01b830f8a5e6e5f957b36bc2d6ef035754c06157
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34367372"
---
### <a name="run-the-service"></a>Ausführen des Diensts

1. Drücken Sie F5 (oder geben Sie im Terminalfenster `azds up` ein), um den Dienst auszuführen. Der Dienst wird automatisch im neu ausgewählten Bereich `scott` ausgeführt. 
1. Sie können bestätigen, dass Ihr Dienst in einem eigenen Bereich ausgeführt wird, indem Sie `azds resource list` erneut ausführen. Zuerst werden Sie bemerken, dass eine Instanz von `mywebapi` jetzt im Bereich `scott` ausgeführt wird (die Version im Bereich `default` wird noch ausgeführt, aber nicht aufgelistet). Zweitens sehen Sie, dass der Zugriffspunkt-URL für `webfrontend` der Text *scott.s.* vorangestellt wurde. Diese URL ist eindeutig für den Bereich `scott` und gibt an, dass bei Anforderungen, die an die „Scott-URL“ gesendet werden, zuerst versucht wird, diese an Dienste im Bereich `scott` zu senden. Ist dies nicht möglich, erfolgt ein Failback an Dienste im Bereich `default`.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie End-to-End-Tests für Code in einer gemeinsamen Umgebung ausführen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing muss Ihr App-Code Weitergabeheader weiterleiten, wie im vorherigen Schritt dieses Leitfadens erläutert.

## <a name="test-code-in-a-space"></a>Testen von Code in einem Bereich
Wenn Sie Ihre neue Version von `mywebapi` in Verbindung mit `webfrontend` testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für ein Web-Front-End, und navigieren Sie zur Seite „Info“. Ihre neue Meldung sollte angezeigt werden.

Entfernen Sie jetzt den Teil „scott.s.“ aus der URL, und aktualisieren Sie den Browser. Jetzt sollten Sie wieder das vorherige Verhalten beobachten (Version `mywebapi`, die in `default` ausgeführt wird).