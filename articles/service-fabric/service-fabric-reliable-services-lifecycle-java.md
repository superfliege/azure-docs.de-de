---
title: "Übersicht über den Lebenszyklus von Azure Service Fabric Reliable Services | Microsoft-Dokumentation"
description: Informationen zu den verschiedenen Lebenszyklusereignissen in Service Fabric Reliable Services
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Übersicht über den Lebenszyklus von Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java unter Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Bei der Betrachtung der Lebenszyklen von Reliable Services sind vor allem die Grundlagen zum Lebenszyklus relevant. Im Allgemeinen:

* Während des Starts
  * Dienste werden erstellt.
  * Sie haben eine Möglichkeit zum Erstellen und Zurückgeben von null oder mehr Listenern.
  * Alle zurückgegebenen Listener werden geöffnet und ermöglichen so eine Kommunikation mit dem Dienst.
  * Die runAsync-Methode des Diensts wird aufgerufen und ermöglicht dem Dienst die Ausführung langwieriger Aufgaben oder von Hintergrundarbeit.
* Während des Herunterfahrens
  * Das an runAsync übergebene Abbruchtoken wird abgebrochen, und die Listener werden geschlossen.
  * Sobald dies abgeschlossen ist, wird das Dienstobjekt selbst zerstört.

Es gibt Informationen zur genauen Reihenfolge dieser Ereignisse. Insbesondere kann sich die Reihenfolge der Ereignisse geringfügig ändern, je nachdem, ob der zuverlässigen Dienst zustandslos oder zustandsbehaftet ist. Für zustandsbehaftete Dienste müssen wir uns außerdem mit dem Tauschszenario für primäre Replikate befassen. Während dieser Sequenz wird die Rolle des primären Replikats auf ein anderes Replikat (oder wieder zurück) übertragen, ohne dass der Dienst heruntergefahren wird. Schließlich müssen wir uns mit Fehlerbedingungen beschäftigen.

## <a name="stateless-service-startup"></a>Start zustandsloser Dienste
Der Lebenszyklus eines zustandslosen Diensts ist recht einfach. So sieht die Reihenfolge der Ereignisse aus:

1. Der Dienst wird erstellt.
2. Gleichzeitig geschehen zwei Dinge:
    - `StatelessService.createServiceInstanceListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. (`CommunicationListener.openAsync()` wird auf jedem Listener aufgerufen.)
    - Die runAsync-Methode des Diensts (`StatelessService.runAsync()`) wird aufgerufen.
3. Falls vorhanden, wird die eigene onOpenAsync-Methode des Diensts aufgerufen. (Insbesondere wird `StatelessService.onOpenAsync()` aufgerufen. Diese Außerkraftsetzung ist ungewöhnlich, aber verfügbar.)

Beachten Sie unbedingt, dass es keine Reihenfolge zwischen den Aufrufen zum Erstellen und Öffnen der Listener und runAsync gibt. Die Listener werden möglicherweise vor dem Start von runAsync geöffnet. Ebenso wird runAsync möglicherweise aufgerufen, bevor die Kommunikationslistener geöffnet sind oder überhaupt erstellt wurden. Wenn eine Synchronisierung erforderlich ist, wird diese dem Implementierer als Übung überlassen. Gängige Lösungen:

* In einigen Fällen funktionieren Listener erst dann, wenn andere Informationen erstellt oder Aufgaben durchgeführt wurden. Für zustandslose Dienste kann diese Arbeit normalerweise im Konstruktor des Diensts, während des `createServiceInstanceListeners()`-Aufrufs oder im Rahmen der Listenererstellung selbst durchgeführt werden.
* In einigen Fällen lässt sich der Code in runAsync erst starten, wenn die Listener geöffnet sind. In diesem Fall ist zusätzliche Koordination erforderlich. Eine gängige Lösung besteht in einem Flag innerhalb der Listener, das angibt, wann diese abgeschlossen wurden, und das in runAsync vor dem Fortfahren mit der eigentlichen Arbeit überprüft wird.

## <a name="stateless-service-shutdown"></a>Herunterfahren zustandsloser Dienste
Beim Herunterfahren eines zustandslosen Diensts wird dasselbe Muster in umgekehrter Reihenfolge befolgt:

1. Gleichzeitig:
    - Alle offenen Listener werden geschlossen. (`CommunicationListener.closeAsync()` wird für jeden Listener aufgerufen.)
    - Das an `runAsync()` übergebene Abbruchtoken wird abgebrochen. (Es wird überprüft, ob die `isCancelled`-Eigenschaft des Abbruchtokens „TRUE“ zurückgibt und ob die `throwIfCancellationRequested`-Methode eine `CancellationException` auslöst.)
2. Sobald auf jedem Listener `closeAsync()` und auch `runAsync()` abgeschlossen wurden, wird die `StatelessService.onCloseAsync()`-Methode des Diensts aufgerufen, sofern vorhanden (auch dies ist eine ungewöhnliche Außerkraftsetzung).
3. Nach Abschluss von `StatelessService.onCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-startup"></a>Start zustandsbehafteter Dienste
Zustandsbehaftete Dienste befolgen mit wenigen Änderungen ein ähnliches Muster wie zustandslose Dienste. Beim Starten eines zustandsbehafteten Diensts lautet die Reihenfolge der Ereignisse wie folgt:

1. Der Dienst wird erstellt.
2. `StatefulServiceBase.onOpenAsync()` wird aufgerufen. Dieser Aufruf wird normalerweise nicht im Dienst außer Kraft gesetzt.
3. Folgende Vorgänge finden parallel statt:
    - `StatefulServiceBase.createServiceReplicaListeners()` wird aufgerufen. 
      - Falls es sich bei dem Dienst um einen primären Dienst handelt, werden alle zurückgegebenen Listener geöffnet. `CommunicationListener.openAsync()` wird für jeden Listener aufgerufen.
      - Bei einem sekundären Dienst werden nur als `listenOnSecondary = true` gekennzeichnete Listener geöffnet. Für sekundäre Dienste geöffnete Listener sind eher selten.
    - Falls es sich bei dem Dienst gerade um einen primären Dienst handelt, wird die Methode `StatefulServiceBase.runAsync()` des Diensts aufgerufen.
4. Nachdem alle Replikatlisteneraufrufe von `openAsync()` abgeschlossen wurden und `runAsync()` aufgerufen wurde, wird `StatefulServiceBase.onChangeRoleAsync()` aufgerufen. Dieser Aufruf wird normalerweise nicht im Dienst außer Kraft gesetzt.

Ähnlich wie bei zustandslosen Diensten wird die Reihenfolge, in der die Listener erstellt und geöffnet werden und **runAsync** aufgerufen wird, nicht koordiniert. Wenn Sie eine Koordinierung benötigen, sind die Lösungen nahezu identisch. Es gibt einen zusätzlichen Fall für einen zustandsbehafteten Dienst. Angenommen, die bei den Kommunikationslistenern eingehenden Aufrufe benötigen Informationen aus [zuverlässigen Sammlungen](service-fabric-reliable-services-reliable-collections.md). Da die Kommunikationslistener geöffnet werden könnten, bevor die zuverlässigen Sammlungen gelesen oder geschrieben werden können oder **runAsync** gestartet werden konnte, ist einige zusätzliche Koordination erforderlich. Die einfachste und gängigste Lösung besteht darin, dass die Kommunikationslistener einen Fehlercode zurückgeben, anhand dessen der Client feststellt, dass die Anforderung wiederholt werden muss.

## <a name="stateful-service-shutdown"></a>Herunterfahren zustandsbehafteter Dienste
Wie bei zustandslosen Diensten stimmen die Lebenszyklusereignisse während des Herunterfahrens mit denen während des Starts überein, aber in umgekehrter Reihenfolge. Wenn ein zustandsbehafteter Dienst heruntergefahren wird, treten die folgenden Ereignisse auf:

1. Gleichzeitig:
    - Alle geöffneten Listener werden geschlossen. `CommunicationListener.closeAsync()` wird für jeden Listener aufgerufen.
    - Das an `runAsync()` übergebene Abbruchtoken wird abgebrochen. Ein Aufruf der `isCancelled()`-Methode des Abbruchtokens gibt „true“ zurück, und die `throwIfCancellationRequested()`-Methode des Tokens löst bei Aufruf eine Ausnahme vom Typ `OperationCanceledException` aus.
2. Nach Abschluss von `closeAsync()` für die einzelnen Listener und nach Abschluss von `runAsync()` wird die Methode `StatefulServiceBase.onChangeRoleAsync()` des Diensts aufgerufen. Dieser Aufruf wird normalerweise nicht im Dienst außer Kraft gesetzt.

   > [!NOTE]  
   > Der Abschluss von **runAsync** muss nur abgewartet werden, wenn dieses Replikat ein primäres Replikat ist.

3. Nach Abschluss der Methode `StatefulServiceBase.onChangeRoleAsync()` wird die Methode `StatefulServiceBase.onCloseAsync()` aufgerufen. Dieser Aufruf ist eine ungewöhnliche Außerkraftsetzung, aber verfügbar.
3. Nach Abschluss von `StatefulServiceBase.onCloseAsync()` wird das Dienstobjekt zerstört.

## <a name="stateful-service-primary-swaps"></a>Tausch des primären Replikats bei zustandsbehafteten Diensten
Während ein zustandsbehafteter Dienst ausgeführt wird, werden nur für die primären Replikate dieser zustandsbehafteten Dienste die Kommunikationslistener geöffnet und die Methode **runAsync** aufgerufen. Die sekundären Replikate werden erstellt, erhalten jedoch keine weiteren Aufrufe. Während ein zustandsbehafteter Dienst ausgeführt wird, kann sich das Replikat ändern, das derzeit das primäre ist. Was bedeutet dies im Hinblick auf die für ein Replikat sichtbaren Lebenszyklusereignisse? Das für das zustandsbehaftete Replikat sichtbare Verhalten hängt davon ab, ob das Replikat während des Tauschs höher oder tiefer gestuft wird.

### <a name="for-the-primary-thats-demoted"></a>Für das tiefer gestufte primäre Replikat
Für das tiefer gestufte primäre Replikat muss Service Fabric die Verarbeitung von Nachrichten und jegliche Hintergrundverarbeitung beenden. Folglich ähnelt dieser Schritt dem Herunterfahren des Diensts. Ein Unterschied besteht darin, dass der Dienst nicht zerstört oder geschlossen wird, weil er als sekundäres Replikat erhalten bleibt. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - Alle geöffneten Listener werden geschlossen. `CommunicationListener.closeAsync()` wird für jeden Listener aufgerufen.
    - Das an `runAsync()` übergebene Abbruchtoken wird abgebrochen. Eine Überprüfung der `isCancelled()`-Methode des Abbruchtokens gibt „true“ zurück, und die Methode `throwIfCancellationRequested()` des Tokens löst eine Ausnahme vom Typ `OperationCanceledException` aus, sofern sie aufgerufen wird.
2. Nach Abschluss von `closeAsync()` für die einzelnen Listener und nach Abschluss von `runAsync()` wird die Methode `StatefulServiceBase.onChangeRoleAsync()` des Diensts aufgerufen. Dieser Aufruf wird normalerweise nicht im Dienst außer Kraft gesetzt.

### <a name="for-the-secondary-thats-promoted"></a>Für das höher gestufte sekundäre Replikat
Analog dazu muss das höher gestufte sekundäre Replikat für Service Fabric mit dem Lauschen auf Nachrichten beginnen und alle Hintergrundaufgaben starten, die abgeschlossen werden müssen. Demzufolge ähnelt dieser Prozess dem Erstellen des Diensts, mit der Ausnahme, dass das Replikat selbst bereits vorhanden ist. Die folgenden APIs werden aufgerufen:

1. Gleichzeitig:
    - `StatefulServiceBase.createServiceReplicaListeners()` wird aufgerufen, und alle zurückgegebenen Listener werden geöffnet. `CommunicationListener.openAsync()` wird für jeden Listener aufgerufen.
    - Die Methode `StatefulServiceBase.runAsync()` des Diensts wird aufgerufen.
2. Nachdem alle Replikatlisteneraufrufe von `openAsync()` abgeschlossen wurden und `runAsync()` aufgerufen wurde, wird `StatefulServiceBase.onChangeRoleAsync()` aufgerufen. Dieser Aufruf wird normalerweise nicht im Dienst außer Kraft gesetzt.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Allgemeine Probleme beim Herunterfahren zustandsbehafteter Dienste und beim Tieferstufen des primären Replikats
Service Fabric ändert das primäre Replikat eines zustandsbehafteten Diensts aus verschiedenen Gründen. Zu den gängigsten Gründen zählen [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md) und [Anwendungsupgrade](service-fabric-application-upgrade.md). Während dieser Vorgänge (sowie beim normalen Herunterfahren des Diensts – etwa, wenn der Dienst gelöscht wird) muss der Dienst das Abbruchtoken (`cancellationToken`) berücksichtigen. 

Bei Diensten ohne ordnungsgemäße Abbruchbehandlung können mehrere Probleme auftreten. Diese Vorgänge werden verlangsamt, da Service Fabric auf eine ordnungsgemäße Beendigung der Dienste wartet. Dies kann letztendlich zu nicht erfolgreichen Upgrades mit Timeout und Rollback führen. Wenn das Abbruchtoken nicht berücksichtigt wird, können auch unausgeglichene Cluster entstehen. Cluster sind unausgeglichen, da Knoten ausgelastet werden, die Dienste aber nicht ausgeglichen werden können, weil ihre Verschiebung zu lange dauert. 

Da es sich um zustandsbehaftete Dienste handelt, ist es auch wahrscheinlich, dass sie die [zuverlässigen Sammlungen](service-fabric-reliable-services-reliable-collections.md) verwenden. Beim Tieferstufen eines primären Replikats wird in Service Fabric als eine der ersten Maßnahmen der Schreibzugriff auf den zugrunde liegenden Zustand entzogen. Dies führt zu weiteren Problemen, die Einfluss auf den Lebenszyklus des Diensts haben können. Die Sammlungen geben Ausnahmen auf der Grundlage des Timings und abhängig davon zurück, ob das Replikat verschoben oder heruntergefahren wird. Diese Ausnahmen müssen korrekt behandelt werden. Von Service Fabric ausgelöste Ausnahmen können dauerhaft [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) oder vorübergehend [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) sein. Dauerhafte Ausnahmen müssen protokolliert und ausgelöst werden. Vorgänge mit vorübergehenden Ausnahmen können hingegen mit einer Wiederholungslogik wiederholt werden.

Die Behandlung von Ausnahmen, die auf die Verwendung von `ReliableCollections` in Verbindung mit Dienstlebenszyklusereignissen zurückzuführen sind, ist ein wichtiger Bestandteil der Tests und Prüfungen für einen zuverlässigen Dienst. Es empfiehlt sich, den Dienst während der Durchführung von Upgrades und [Chaostests](service-fabric-controlled-chaos.md) immer unter Last auszuführen, bevor Sie ihn in einer Produktionsumgebung bereitstellen. Mit diesen einfachen Schritten können Sie sicherstellen, dass Ihr Dienst ordnungsgemäß implementiert ist und Lebenszyklusereignisse korrekt behandelt.

## <a name="notes-on-service-lifecycle"></a>Hinweise zum Dienstlebenszyklus
* Sowohl die `runAsync()`-Methode als auch die `createServiceInstanceListeners/createServiceReplicaListeners`-Aufrufe sind optional. Ein Dienst kann eine dieser Optionen, beide oder keine von beiden aufweisen. Wenn der Dienst beispielsweise alle Vorgänge infolge von Benutzeraufrufen ausführt, muss `runAsync()` nicht implementiert werden. Nur die Kommunikationslistener und der zugehörige Code sind erforderlich. Ebenso ist das Erstellen und Zurückgeben von Kommunikationslistenern optional, da der Dienst möglicherweise nur Hintergrundaufgaben ausführt und daher nur `runAsync()` implementieren muss.
* Ein Dienst kann `runAsync()` erfolgreich abschließen und dann zurückkehren. Dies wird nicht als Fehlerbedingung betrachtet, sondern stellt die Hintergrundarbeit für den Abschluss des Diensts dar. Für zustandsbehaftete zuverlässige Dienste wird `runAsync()` erneut aufgerufen, wenn der Dienst vom primären Replikat tiefer gestuft und dann wieder auf das primäre Replikat höher gestuft wird.
* Wenn ein Dienst durch eine unerwartete Ausnahme von `runAsync()` aus beendet wird, ist dies ein Fehler. Das Dienstobjekt wird heruntergefahren, und ein Integritätsfehler wird gemeldet.
* Zwar gibt es keine zeitliche Begrenzung für die Rückgabe dieser Methoden, aber Sie verlieren sofort die Möglichkeit zum Schreiben und können daher Ihre eigentlichen Arbeiten nicht abschließen. Es wird empfohlen, dass Sie sie so schnell wie möglich nach dem Empfang der Abbruchanforderung zurückgeben. Wenn der Dienst nicht in einem angemessenen Zeitraum auf diese API-Aufrufe reagiert, kann Service Fabric das Beenden des Diensts erzwingen. Dies geschieht normalerweise nur während Anwendungsupgrades oder beim Löschen eines Diensts. Das Timeout beträgt standardmäßig 15 Minuten.
* Fehler im `onCloseAsync()`-Pfad führen zu einem Aufruf von `onAbort()`. So erhält der Dienst eine letzte Gelegenheit zum Bereinigen und Freigeben aller beanspruchten Ressourcen.

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Reliable Services](service-fabric-reliable-services-introduction.md)
* [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start-java.md)
* [Erweiterte Verwendung von Reliable Services](service-fabric-reliable-services-advanced-usage.md)
