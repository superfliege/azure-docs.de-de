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
ms.openlocfilehash: 2f2177b289ba9d13ba635bddcd61748e1e8ab0d1
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39063093"
---
### <a name="run-the-service"></a>Ausführen des Diensts

1. Drücken Sie F5 (oder geben Sie im Terminalfenster `azds up` ein), um den Dienst auszuführen. Der Dienst wird automatisch im neu ausgewählten Bereich `default/scott` ausgeführt. 
1. Sie können bestätigen, dass Ihr Dienst in einem eigenen Bereich ausgeführt wird, indem Sie `azds list-up` erneut ausführen. Zuerst werden Sie bemerken, dass eine Instanz von `mywebapi` jetzt im Bereich `default/scott` ausgeführt wird (die Version im Bereich `default` wird noch ausgeführt, aber nicht aufgelistet). Zweitens sehen Sie, dass der Zugriffspunkt-URL für `webfrontend` der Text „scott.s.“ vorangestellt wurde. Diese URL ist eindeutig für den Bereich `default/scott`. Die spezielle URL bewirkt, dass bei Anforderungen, die an die „Scott-URL“ gesendet werden, zuerst versucht wird, diese an Dienste im Bereich `default/scott` weiterzuleiten. Ist dies nicht möglich, erfolgt ein Fallback auf Dienste im Bereich `default`.

```
Name         Space          Chart              Ports   Updated     Access Points
-----------  --------       -----------------  ------  ----------  -------------
mywebapi     default/scott  mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default        webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie Code in einer freigegebenen Umgebung testen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing muss Ihr App-Code Weitergabeheader weiterleiten, wie im vorherigen Schritt dieses Leitfadens erläutert.

### <a name="test-code-in-a-space"></a>Testen von Code in einem Bereich
Wenn Sie Ihre neue Version von `mywebapi` mit `webfrontend` testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für `webfrontend`, und navigieren Sie zur Seite „Info“. Ihre neue Meldung sollte angezeigt werden.

Entfernen Sie jetzt den Teil „scott.s.“ aus der URL, und aktualisieren Sie den Browser. Jetzt sollten Sie das frühere Verhalten beobachten (Version `mywebapi`, die in `default` ausgeführt wird).
