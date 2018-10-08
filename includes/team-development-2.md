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
ms.openlocfilehash: 8580e19f1955becb2cfaee13742054af1a9edb7f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410445"
---
### <a name="run-the-service"></a>Ausführen des Diensts

1. Drücken Sie F5 (oder geben Sie im Terminalfenster `azds up` ein), um den Dienst auszuführen. Der Dienst wird automatisch im neu ausgewählten Bereich `default/scott` ausgeführt. 
1. Sie können bestätigen, dass Ihr Dienst in einem eigenen Bereich ausgeführt wird, indem Sie `azds list-up` erneut ausführen. Zuerst werden Sie bemerken, dass eine Instanz von `mywebapi` jetzt im Bereich `default/scott` ausgeführt wird (die Version im Bereich `default` wird noch ausgeführt, aber nicht aufgelistet). Wenn Sie `azds list-uris` ausführen, werden Sie feststellen, dass der Zugriffspunkt-URL für `webfrontend` der Text „scott.s.“ vorangestellt wurde. Diese URL ist eindeutig für den Bereich `default/scott`. Die spezielle URL bewirkt, dass bei Anforderungen, die an die „Scott-URL“ gesendet werden, zuerst versucht wird, diese an Dienste im Bereich `default/scott` weiterzuleiten. Ist dies nicht möglich, erfolgt ein Fallback auf Dienste im Bereich `default`.

```
Name                      DevSpace  Type     Updated  Status
------------------------  --------  -------  -------  -------
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               default   Service  26m ago  Running
```

```
Uri                                                            Status
-------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                    Tunneled
http://webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```

![](../articles/dev-spaces/media/common/space-routing.png)

Mit dieser integrierten Funktion von Azure Dev Spaces können Sie Code in einer freigegebenen Umgebung testen, ohne dass die einzelnen Entwickler den kompletten Dienststapel in ihrem Bereich neu erstellen müssen. Für dieses Routing muss Ihr App-Code Weitergabeheader weiterleiten, wie im vorherigen Schritt dieses Leitfadens erläutert.

### <a name="test-code-in-a-space"></a>Testen von Code in einem Bereich
Wenn Sie Ihre neue Version von `mywebapi` mit `webfrontend` testen möchten, öffnen Sie in Ihrem Browser die URL des öffentlichen Zugriffspunkts für `webfrontend`, und navigieren Sie zur Seite „Info“. Ihre neue Meldung sollte angezeigt werden.

Entfernen Sie jetzt den Teil „scott.s.“ aus der URL, und aktualisieren Sie den Browser. Jetzt sollten Sie das frühere Verhalten beobachten (Version `mywebapi`, die in `default` ausgeführt wird).
